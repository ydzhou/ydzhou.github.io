---
layout: post
title: Design a Short URL Service in AWS
---

## High-level Architecture

The overview is shown as below. We are taking full advantage of AWS techniques to provide high flexibility and high robust. AWS can also significantly reduce operation maintanance due to its cloud computing nature. Serving globally can be easily achieved due to superb regionalization support in AWS. I will go through each component and explain in details on how this design works.

![Overview]({{ site.baseurl }}/assets/su0.svg)

Firstly, both Route53 and ELB is to provide a consistent entry-point for customers to hit our service or website. Both components are working globally and can handle sudden traffic peaks well.

When the requests came through ELB, it directs into our web server fleet. The server fleet is managed by AWS EKS for easy service discovery, auto-scaling, deployment and native monitoring with CloudWatch. The web server fleet will finds an available server to handle the request, which is basically displaying the website.

I have a separate service dedicated to shorten the URL because it is a good pattern to harden the separation of ownership. In microservice world, you want each service to do their own functions and it is easy to maintain in a large scale.

If Customer clicks the short URL function, corresponding handler in the web service will make a call into short URL service through k8s service discovery. short URL will run business logic, basically generating a unique ID and then store this ID with its origin URL into our database. Next time when customer fetch the orgin URL using this ID, we can find them in the database.

We choose dynamoDB as database because it is hard to scale up the database server. Relational DB is generally bad for high throughput and large scale application. Also, dynamoDB's performance can be medicore sometimes. Thus we add a layer of cache using AWS Redis to speed up the read. 

### Data Scheme

#### DynamoDB

short-url table

1. ID (string) : this is the primary key. it is the short ULR without our domain. e.g. www.shortURL.com/asdfasdf, then ID is asdfasdf
2. URL (string) : this is the orgin url

#### Redis

Similar to dynamoDB, with key to be ID and value to be URL.
