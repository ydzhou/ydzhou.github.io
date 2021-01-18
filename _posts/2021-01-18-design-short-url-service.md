---
layout: post
title: Design a Short URL Service in AWS
---

## High-level Architecture

The overview is shown as below. We are taking full advantage of AWS techniques to provide high flexibility and high robust. AWS can also significantly reduce operation maintanance due to its cloud computing nature. Serving globally can be easily achieved due to superb regionalization support in AWS. I will go through each component and explain in details on how this design works.

![Overview]({{ site.baseurl }}/assets/su0.svg)

Firstly, both Route53 and ELB is to provide a consistent entry-point for customers to hit our service or website. Both components are working globally and can handle sudden traffic peaks well.

When the requests came through ELB, it is load-balanced into an available web server. The web server fleet is managed by AWS EKS for easy service discovery, auto-scaling, deployment and native monitoring with CloudWatch. The web server contain several handlers to either display the website content, or fulfill further function such as shortening the URL or redirecting to the origin URL.

I have a dedicated service fleet to shorten the URL because it is a good practice to do separation of ownership. In microservice world, you want each service to do their own functions and it is easy to maintain in a large scale.

If customer clicks the short URL function on our website, corresponding handler in the web service will make a call into short URL service through k8s service discovery. short URL will run business logic, basically generating a unique ID and then store this ID with its origin URL into our database. Next time when customer types our shorten URL in the web browser and hitting the server again, we will read the origin URL from database and redirect this request.

We choose dynamoDB as database because it is hard to scale up the database server. Relational DB is generally bad for high throughput and large scale application. However dynamoDB is not good at performance. Considering reading a short URL can be more often than writing, I am adding a Redis cache for reading and reading only to provide extreme performance.

### Data Scheme

#### DynamoDB

short-url table

1. ID (string) : this is the primary key. it is the short ULR without our domain. e.g. www.shortURL.com/asdfasdf, then ID is asdfasdf
2. URL (string) : this is the orgin url

#### Redis

Similar to dynamoDB, with key to be ID and value to be URL.
