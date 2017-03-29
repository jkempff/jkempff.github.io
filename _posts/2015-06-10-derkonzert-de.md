---
layout: post
title: Creating derkonzert.de
category: frontend
---

> This article refers to an older version of derkonzert.de, check out [this article instead](http://ju.am/article/how-derkonzert-works-part-1), to learn about the current version.

## "Scheiße, die spielen heute?"

..translates to: "Damn it, that band's concert is today?". This happens way too often.

In munich, there are alot of concerts. But the problem is, there often are only few, that are truely interesting, or satisfy my music taste.

All big platforms that list concerts a) are limited in genres, or b) show everything, including hundrets of events which you arent interested in.

So you are forced to check out multiple websites (or magazines) to find the needles in the haystack.

No fun.

### the solution

So I had the idea to create a list of needles, of only those events that I would go to.

At the same time, I played around with [Reactjs][reactjs] and decided to make it my react test-project. It may not be the perfect playground, since there hardly are any user interactions or state changes, but who cares.

Finding a name wasn't easy, and I have to give credits to my boss Bernd, who actually came up with *derkonzert*!

### the setup

The project is separated into three parts:

* the backend, where all data is stored and administrated and where notifications get triggered
* the app, which is the website the visitors see
* the notifications (this isn't a real part, but it must be noted here): e.g. the awesome [yo app][justyo]

### the backend

This is a light, silverstripe php framework instance, that spits out JSON formatted concerts and an rss feed (mainly used for mailchimp).

One thing I learned here, the existing RESTFul modules all somehow didn't match my needs, whether it was inconsistent formatting, or their weird CRUD implementations (which is *not* using silverstripes `PermissionProvider`..). So I wrote one tiny controller to accomplish all I needed. In no time, Silverstripe FTY. That said, it's *not a true RESTful service*.

I also learned about Silverstripes `CliController` class which basically is just a usual `Controller`, but only can be called via the cli. Perfect for my cron jobs that e.g. trigger the YO API.

### the frontend

This part of *derkonzert* is an isomorphic ReactJs app, which means, the same code that runs on the server, runs in the client (the browser).

The only difference between the servers and the client's code is, that on the server I start a simple [express node.js server][expressjs] and on the client, I only start the [React Router][reactrouter], which, by the way, is a pretty neat routing solution.

There are a couple of benefits, building a webapp this way:

* the user actually gets markup within the first request and doesnt have to wait for the scripts to come in
* the search engine bot, well, also gets something to read
* the developer doesnt have to switch between different programming languages or templating styles
* and my favorite: the no-js user actually gets a full functional website! Go ahead, disable javascript and visit [derkonzert.de][derkonzert]! You even can submit concerts.

Of course, I could have accomplish this with other tools, but I really liked it, to have all my module functionality and markup at the same place.
There was no "Okay, now, which template was it again?" at all.

### the notifications

Well, all there is to say about YO: it's super simple. Do your POST, thats it.
There is a daily cronjob, that triggers YO's API, if new and featured concerts has been published.

## thoughts

Now that [derkonzert][derkonzert] is up and running for some time, I am super happy about all the incoming concert submissions! Sometimes one would'nt match my taste, but I'll publish it anyways, it just won't show up in the 'featured' list.

**One thing** that some people asked me: Do I see, who actually submitted a concert? *No*, I cannot see who submitted something.

## feedback

If you have any kind of feedback, leave me a note, I am happy about any kind of response!

[justyo]: https://www.justyo.co/
[expressjs]: http://expressjs.com/
[reactrouter]: https://github.com/rackt/react-router
[reactjs]: https://facebook.github.io/react/
[silverstripe]: http://www.silverstripe.org/
[derkonzert]: http://derkonzert.de/