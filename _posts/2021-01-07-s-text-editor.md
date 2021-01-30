---
layout: default
title: 用 Go 写了一个简单的终端文字编辑软件
---

I build a simple text editor in the terminal [here](https://github.com/ydzhou/ste).

## Overview

Similar to MVC pattern, a text editor has three independent module:

1. controller: to handle customer keyboard input and dispatch to further processing.
2. model: to store what customer is working on and process necessary business logic.
3. view: to render user interface of the editor

![Overview]({{ site.baseurl }}/assets/ste0.svg)

You can think of the editor is in a infinite loop. First we clear the screen and render the viewable text document content in `stdout`. The content view is decided by both the postition of the cursor and size of the terminal screen. Then we listen on the key input at `stdin` and modify the text document or execute corresponding commands.

```
for {
    editor.display.DrawScreen(e.buf, e.cursorX, e.cursorY)
    if editor.process() {
        break
    }
}
```

## How to Handle Key Input

We listen to `Stdin` by one rune. For regular key, it takes only one rune and will be captured and sent to processing. However, for special key, the escape key, it can take multiple runes. The good thing is that, those escape key starts with rune of value 27. Therefore if we listen to a rune of value 27, we need to continue trying to read through the rest of `Stdin`. For example, if a customer pressed arrow up key, it will send three runes. We need to read them all and decide which arrow this keypress is by looking at the 3rd rune. 65 is the arrow up. Once we figure out which key a customer has pressed, we can continue either putting regular keys into the document or executing special actions such as move cursors around or quitting the editor.

```
if r == 27 {
    if e.reader.Buffered() == 0 {
		return ESCAPE
    }
    for i := 0; i < 3; i++ {
        r, _, err = e.reader.ReadRune()
        if err != nil {
            return UNKNOWN
        }
        if i == 1 {
            switch r {
            case 65: return ARROW_UP
            case 66: return ARROW_DOWN
            case 67: return ARROW_RIGHT
            case 68: return ARROW_LEFT
            }
        }
    }
}
```

After getting the index, we first expand the array of runes by the `K` number of runes we want to insert. Then we shift the runes that after target index by `K` and place the new runes.

```
func (b *Buffer) Insert(x, y int, data []rune) {
    idx := b.getIdx(x, y)
    if idx == len(b.txt) {
        b.txt = append(b.txt, data...)
        b.txt = append(b.txt, '\n')
        b.lineNum++
        return
    }
    txt := make([]rune, idx)
    copy(txt, b.txt[:idx])
    txt = append(txt, data...)
    txt = append(txt, b.txt[idx:]...)
    b.txt = txt
    b.dirty = true
}
```

## How to Store and Modify Document

We need some ways to store the text that are currently being processed. Giving strong computation power of modern machine, I use a single "big" string to store the text. I am going through how each actions will modify this string to process text.

### Insert a word

If a customer type a word at current cursor, this word will be inserted in the cursor position. This can be done by inserting the word into the string. Since cursor position is in 2 demision, we need to convert it into the corresponding index of our string as below:

```
func (b *Buffer) getIdx(x, y int) int {
    idx := 0
    for idx, _ = range b.txt {
        if x == 0 && y == 0 {
            break
        }
        if x > 0 && b.txt[idx] == rune('\n') {
            x--
            continue
        }
        if x == 0 {
            y--
        }
        if b.txt[idx] == rune('\n') {
            break
        }
    }
    return idx
}
```

### Insert a new line

Inserting a new line is similar to insert a regular word by putting `\n` in the corresponding position

### Insert a tab

Tab is quite troublesome since it is hard to determine its position just based on cursor. One way is to convert tab as spaces and store them in the buffer, yet it can break some file if it requires tab to execute, such as Fortran or Assemble. The other way is to store tab as `\t` and then restraint cursor to never moved in-between spaces that belong to a single tab.

### Delete a word

Similar to insertion, we can simply shrink the string to drop word at corresponding position. We do not need to worry about deleting the newline since we consider it as a regular word.

### Move cursor around

Cursor position is tracked so that we can do insertion or deletion at the correct index. One thing to notice is that, we need to fix the position if customer is moving the cursor out of bound. For example, if customer tries to move the cursor to the right at the end of line, the cursor is actually re-positioned to the beginning of next line. Another thing to notice is that deleting a line breaker or insert a new line, we need to move the cusor to the end of previous line or to the beginning of the next line, respectively.

One more thing is that we handle scrolling in display module and allow cursor to move freely within the document.

## How to Display Document

The editor view is rendered by converting our text object, a list of runes into actually document. We render special character such as tab and line breaker in the screen. We also need to determine which part of document can be rendered, so that we can allow customer to scroll around.

### Scrolling

We keep tracking two variables `offsetX` and `offsetY`. So the current viewable area is `[offsetX, offsetX + viewX, offsetY, offsetY + viewY]`. `viewX` and `viewY` are the length and width of current terminal. If the cursor is moving outside of this area, we need to update `offsetX` and `offsetY`.

```
func (d *Display) scroll(cursorX int, cursorY int) {
    if cursorX >= d.offsetX+d.viewX {
        d.offsetX = cursorX - d.viewX + 1
    } else if cursorX < d.offsetX {
        d.offsetX = cursorX
    }
    if cursorY >= d.offsetY+d.viewY {
        d.offsetY = cursorY - d.viewY + 1
    } else if cursorY < d.offsetY {
        d.offsetY = cursorY
    }
}
```

Then we can use `offsetX` to get `viewX` number of lines to display and for each line, we only display character after index `offsetY`.

## Learning and Findings

1. Go has native support to process UTF8 by using runes
2. Terminal control in Go need to be tailored for each operating system. For example, control flag to access term of OSX (freebsd) and Linux is different.
4. Converting 2D postition in view and 1D index in text object can cause a lot of effort.
