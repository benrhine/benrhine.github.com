---
layout: post
title:  "API Gateway 001 - Questions?"
date:   2023-05-09
excerpt: "Can a hosted API gateway be a suitable replacement for Spring Controllers?"
image: "https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExMjM3YzgxNmJiMGIzYzI2MmJiOGU5ODZkNjFhYWM5ZDExNDg3YWM3ZSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/3oz8xtBx06mcZWoNJm/giphy.gif"
---

## API Gateway 001 - Questions?

This post is more of some general musings of a topic I have not been able to find a clear answer to in my googling. So,
before I get to my question I'll fill you in a bit on what I've been learning. Most recently I've been focusing on trying
to up level my DevOps skills as it seems that these days being able to write applications in multiple languages using a 
variety of frameworks is still not enough to keep the corporate world happy. Not only do they want you to be able to 
build the product they also want you to be able to do all the infrastructure and product management (see the above example
image of dev hard at work). So to improve myself
I've been going through one area at a time in AWS trying to get a deep understanding of how it works with the when and why
you would use that particular component. Additionally, I am trying to distill all those learnings into the [Serverless Framework](https://www.serverless.com)
so that they are all easily repeatable on future projects (ie probably expect some future posts on that work).

*Note: At this point this article is only referring to the Aws API Gateway as I have yet to check out Googles or Azures offerings.*

### The Question
Now, on to my question... **"Can a hosted API gateway be a suitable replacement for Spring Controllers"**? I've searched high, and
I've searched low but have not been able to get a good answer to this. Searching in this vein turns up items about "can 
the API gateway go directly to Kubernetes ingress controllers" or "Spring Cloud Gateway" (which is itself a API Gateway,
but it's more like a separate application you need to maintain) but no direct answer about how
to best leverage a hosted API Gateway with Spring. Since I haven't been able to find a clear answer here is what I have figured
out so far ...

### API Gateway Pass-through
This first approach is kind of boring and weather you are hosting your app using a variant of Lambda or ECS it is essentially
a pass through function. Meaning that when used this way the API Gateway is acting more like an aggregator than anything
as it allows for you to have a unified api across many independent APIs (possibly separate applications). Outside
potentially aggregating multiple applications it is more or less just mapping the unified API naming convention to whatever
internal API convention you may have in your application. While this step may be necessary in order to make your apps APIs
accessible from AWS this seems a bit like an under utilization. Outside if this is required on Aws (I'm pretty sure) why have
this extra in-between layer? Could we make this simpler?

### Service Only with Lambda Container
I thought I should mention this for completeness but I won't really get into detail on this step.
This approach I have not tried yet personally but its kind of like the in-between step between what we just talked about
and what we are about to talk about. This way is essentially exactly the same as the last one where you have a container
just this time you are hosting it in lambda rather than ECS and you are directly passing the APIs through.

### Service Only with Lambda
When using *just java* **(no Spring)** I've found that it is entirely possible to build a lambda handler **instead** of a standard
mvc controller and then with a standard service and repo build a full application service (ie lambda handler -> service -> repo -> DB).

Now, maybe I'm just behind in discovering this and this is common practice somewhere and I just can't get my google-fu
right to find it but with the amount of things I've read recently I would have expected more speaking to this sort of design.
My personal thought on why I'm not seeing much is that most companies are not here yet, there are a few really forward facing
companies that are doing this but most are not. So lets take a sidebar and discuss a bit on why I think that ...

1. At first glance the idea of pushing a function level microservice starts to look complicated. You could have 100's of
lambda services, and you have to tie them all together with the API Gateway, and it easily begins to look like a management nightmare.
2. They don't know what tools are available to help with this problem.
   - serverless framework
   - terraform
3. It's just easier to containerize the exising applications and accept the minimal overhead of extra in-between layers
   - If I had to guess this is probably the number one reason as most applications are not so performance sensitive that they can't absorb a few milliseconds.

From my personal experimentation and learnings I can say that the tools certainly are available and also seem to mostly
resolve that hard to manage concern. Enter serverless and terraform. While terraform may be the swiss army knife of the
cloud infrastructure world it also has a steep learning curve so for the time being I've chosen to work with the [Serverless Framework](https://www.serverless.com)
as it uses the declarative yml format which most Java devs should be familiar with and have found it "almost" easy to learn.
For clarification when I say "almost" I have learned how to translate all the following to a less than 175 line yml file
in less than a week...

- A single codebase
- No Spring
- A single serverless.yml file

Will produce

- 3 RESTful API Lambda functions with DynamoDB read/write
  - Each function has 3 different usage plans with independent limits
  - Each function has a secure api key
- 3 HTTP API Lambda functions with DynamoDB read/write
  - Each function has full OAuth from Auth0
- After warm up call sub 20 ms read/write time

While that may sound overly simple that's actually quite a bit orchestrated in very little time especially considering
I had never touched serverless before about 5 days before this post. So back to my question ...
**"Can a hosted API gateway be a suitable replacement for Spring Controllers"**? I think my above musings clarify that
it absolutely can be done and that it can actually be done with relative ease which I think changes the question a bit to
**"Should you use a hosted API gateway as a replacement for a Spring Controller"**? Take that as I'm asking, because I don't
know. Comment on this, email me, I would love to know your thoughts. Is there anything I'm missing about when thinking
through this design?

The one downfall I do see from taking the proposed approach is you loose the ability to have Spring interceptors which in
some cases can be quite important. There is some functionality you may be able to get out of the API Gateway or calling
another Lambda first, but it's not a direct replacement.


### Authentication
Now, while I think the design I've set forth is pretty cool (at least I think it is) but like I've already said I have 
questions and so far very few answers. Which means at this point I'm not sure if I would advocate for the above until I have further
clarity. With that being said I do think I have found a major reason to consider an API Gateway (even if it was configured
as mostly pass-through plus the following) and that reason is authentication extraction. What do I mean by this? I mean that it is possible
for the API Gateway to handle validating your JWT tokens which means you could build an entire Java application
without having to mess with security in the app (outside of role based permissions). Anyone who has used Spring Security
before knows that getting it working is always a bit tricky and can
take a substantial amount of time to get it right so why not bring that up to the API Gateway? To put it in perspective I configured full
OAuth 2.0 including role based permissions from Auth0 and had the deployment automated in serverless in less than 4 hours.
I'm not sure if I've every fully configured Spring Security from scratch in less than several days.

In my mind this is a potentially massive win, it makes securing your endpoints so much simpler. It also makes your application
code easier to follow and should make your testing rubric a bt simpler. It also means your app is not tied to your security
provider so if there is ever a reason to change the authentication you don't have to change the application.

### Conclusion
In short this is what has been on my mind, I think its opening the door to some very cool designs in the near future.
Leave a comment, drop me a line would love to know your thoughts.



















