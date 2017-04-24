---
layout: post
title: 🍓🍰 Running Server Side Swift on RaspberryPi
tags: apache linux swift server side raspberry
---

<img src="http://zeezide.com/img/rpi-swift.svg?2"
     align="right" 
     style="padding: 0 0 0.5em 0.5em; width: 5em; height: 5em;" />
  
Running Swift server applications in a huge datacenter?
That is for noobs.
Running Swift server applications on a 
[Raspberry Pi](https://www.raspberrypi.org),
*that* is 1337!
And now you can ...

> “Modern servers often have more than 128GB of memory!”

Ha! Modern servers often have *less than 1GB* of memory and often consume less
than 3W of power.
You know:

<a href="http://www.switchdoc.com/2015/12/how-to-mount-and-use-an-i2c-compass-on-your-raspberry-piarduino-project/" target="ext"><img src="https://encrypted-tbn2.gstatic.com/images?q=tbn:ANd9GcRNf14IT4s_6EPzGOiWg5szAeFPKhNbLPGDK1wZn1saUcNwAbeJ" width="46%"/></a> vs. <a href="http://www.vanadiumcorp.com/news/grid-storage/416-apple-to-build-200mw-solar-farm-to-power-data-center" target="ext"><img src="http://core0.staticworld.net/images/article/2017/01/apple-solar-1-100705825-orig.jpg" width="46%"/></a>

<br>

## Run Apache with mod_swift on a Raspberry Pi

It is easy to run mod_swift and ApacheExpress on a Raspi. This, and all the
other demos, including mod_dbd database access and the TodoMVC backend:

<img src="https://github.com/AlwaysRightInstitute/mod_swift/raw/master/DocRoot/mod_swift-mustache-screenshot.jpg"
     align="center" />
     
... all on a Raspberry.

### Raspi

You got no Raspberry Pi?
[Get one](https://www.amazon.com/Raspberry-Model-A1-2GHz-64-bit-quad-core/dp/B01CD5VC92/ref=sr_1_1?s=pc&ie=UTF8&qid=1492700091&sr=1-1&keywords=raspberry+pi+3&refinements=p_89%3ARaspberry+Pi)!
It can host all the servers you need at home and it is just ~$40.
I host my music library for the Sonos speakers on it, some home
automation stuff, including Graphite, etc.
A Raspi 3 is plenty fast.
Best of all: you can run Swift on it!

### Docker

Since it makes everything easier, we assume you have 
[Docker](https://www.docker.com) 
running on your Raspi (and your Mac).
Here is a how-to that describes how to setup a Docker base image:

- [Setup Docker on Raspi](https://github.com/helje5/dockSwiftOnARM/wiki/Setup-Docker-on-Raspi)

And since your are using Swift, you are most likely using a Mac as your
desktop machine, hence this will be handy:

- [Remote Control Raspi Docker](https://github.com/helje5/dockSwiftOnARM/wiki/Remote-Control-Raspi-Docker)

Also get [Docker for Mac](https://docs.docker.com/docker-for-mac/install/)
of course.

### Run the demo

Simple, don't forget to map the ports:

    docker run --name mod_swift -d -p 8042:8042 \
               modswift/mod_swift-demo

And just invoke:

    http://IPofYourRaspi:8042/
    
To play with the TodoMVC stuff, you need to invoke the web interface manually
(the embedded links direct the frontend to localhost):

    http://todobackend.com/client/index.html?http://IPofYourRaspi:8042/todomvc/

### Cows

This is all nice, but does it come with any cows? Fear not, yes, the cows
module and an Express demo rendering 400+ cows are included. So is leftpad.

```
         (__)
       /   @@      ______
      |  /\_|     |      \
      |  |___     |       |
      |   ---@    |_______|
      |  |   ----   |    |
      |  |_____
*____/|________|
CompuCow After an All-niter
```

### QEmu

Some well informed may ask: Do I even need a Raspi to try that?
Docker-for-Mac includes QEmu support and can directly run Raspi images!
That is indeed true.

Unfortunately QEmu doesn't seem to emulate everything required for Swift,
so neither Swift Package Manager nor this stuff work in QEmu
(`Unsupported syscall: 389`).


## Summary

Hey, we love feedback. Join the mailing list, Slack channel or just drop us
an email to tell us why this is crap (or not?): [Slack](http://slack.noze.io).

