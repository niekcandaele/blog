---
title: Redis hackathon 2021
date: "2021-05-16T07:26:03.284Z"
categories: [code]
---

The [Redis Hackathon 2021](https://redislabs.com/hackathon-2021/) had a simple objective: build something using Redis.

Redis is most known as a caching datastore. However, did you know they also have support for [storing and retrieving JSON documents](https://redislabs.com/modules/redisjson-quick-start/), [full-text search](https://redislabs.com/modules/redisearch-quick-start/), [time series data](https://redislabs.com/modules/redistimeseries-quick-start/), [graph databases](https://redislabs.com/modules/redisgraph-quick-start/), [AI inferenceing/serving engine](https://oss.redislabs.com/redisai/quickstart/) and [distributed, programmable engine similar to Lambda functions](https://oss.redislabs.com/redisgears/)?

I didn't!

We set out to create an application using these new Redis modules. We had a month to come up with something cool!

We ended up creating [Feature Creep](https://github.com/niekcandaele/feature-creep).

Check out our demo video!

[![Video](https://img.youtube.com/vi/Kmqr_M1B9R4/0.jpg)](https://www.youtube.com/watch?v=Kmqr_M1B9R4 "Video Title")

We learned a ton by creating this application, if you are interested in the technical details, please see the readme of the repo on Github. Following here are some thoughts on what I would do differently if I would redo this hackathon. Please keep in mind these are the thoughts of someone who was exposed to these modules for only a little over a month and definitely not an expert!

## Thoughts

We made it our unofficial goal to only use Redis for data storage. As with a lot of applications, most of our data was highly relational. We tried to shoehorn all of this into the RediJSON module. While this worked, using a relational database with proper foreign keys would have made our life easier.

Redis streams are really awesome! I wish I realized that a bit earlier than 24 hours before the deadline 😃. Streams allow you to implement event-driven programming in an easy way. It even has features that allow messages to be only handled once (instead of fanning out) which essentially allows you to turn streams into a queue. If I had seen this sooner, a lot more of the application would take advantage of streams.

Redis Gears is a really great idea. It allows you to do background parsing of data or just background jobs in general. It scales transparently with your database. Redis Labs compare it to AWS Lambda, however there are a lot of differences.

First of all, Gears only support Python right now. This is understandable of course, but for us (primarily javascript developers) this meant a lot of context switching. It also means that there is no code reuse in our application between the main codebase and the Gears functions.

On the topic of code reuse, the way you load functions into Gears is not great.

`RG.PYEXECUTE "<function>" [UNBLOCKING] [REQUIREMENTS "<dep> ..."]`

Gears functions need to be encoded as a string. We had some issues with that (because we messed up that encoding). It would be nice if there was an interface for this that handles edge cases and just in general makes loading functions easier. This also means that it is difficult for a function to import helper functions from another file. You can pass the `REQUIREMENTS` argument but as far as I know, you can only load dependencies from a PyPi repository. This is quite cumbersome, especially when you're doing a hackathon and want to move as fast as possible.

In general, this applies to all the modules we used, I noticed there was very little tooling/support around the modules. For instance, we used [IORedis](https://github.com/luin/ioredis) which is a very popular library for working with Redis in Javascript. It does not have support for the modules, and it looks like [the official recommendation is to use raw commands](https://github.com/luin/ioredis/issues/525). We got it to work, and with some abstractions we were able to make a easy to use interface for the JSON functions. It would have been nice to have proper support. Perhaps not in the IORedis package though.

## Conclusion

In the end I really enjoyed getting to know all the Redis modules. While I was not able to dive too deep into each of them, I will definitely consider these as solutions for future projects.
