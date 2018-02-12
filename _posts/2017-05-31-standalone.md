---
layout: post
title: mod_swift ✂️ ApacheExpress
tags: apache linux swift server side raspberry
---

Breaking news: **mod_swift** got turned into a small standalone project. And 
a much enhanced [ApacheExpress](http://apacheexpress.io/)
is becoming its own - separate - project.

Our 
[original mod_swift technology demo](https://github.com/ApacheExpress/ApacheExpress/tree/standalone-demo-2017-04-24) 
contained various things:

- mod_swift, the C module which adds the `LoadSwiftModule` directive
- Module maps etc to expose the Apache C API to Swift
- ExExpress
- ApacheExpress
- a set of examples

All setup as a 'demo' environment to showcase how Apache modules can be written
in Swift, right within Xcode.

As it turns out, Swift is actually a great language to write Apache modules in!
And ApacheExpress is a pretty neat framework on top.
So in the last few months we worked on turning **mod_swift** 
from being just a neat technology demo 
into being a reliable hosting environment for Swift web applications.

During that,
[ApacheExpress](http://apacheexpress.io/)
functionality has also grown a lot and evolved into a rather powerful web 
application framework (stay tuned!).
Hence we felt that we need to separate the two.
On one side
`mod_swift` being just an integration point between Swift and Apache that is
framework independent.
On the other side 
[ApacheExpress](http://apacheexpress.io/) as a higher level framework with an
Express like API.

## The new mod_swift

The [new mod_swift](https://github.com/modswift) is not setup as a fancy demo 
but as an infrastructure component.
If you want a little like [Phusion Passenger](https://www.phusionpassenger.com),
but not really, because **mod_swift** is used to create fully native Apache 
modules instead of hosting foreign webapps within Apache.

While 
[you can actually build Apache modules with just **mod_swift**](http://docs.mod-swift.org/usage/),
this has a very low level API and may be a little inconvenient.
The expectation is that people are usually going to use a highler level API on
top of that ([ApacheExpress](http://apacheexpress.io/), or others).

So what does the new mod_swift provide:

- mod_swift.so, the C Apache module which adds the `LoadSwiftModule` directive
- Module maps, shims and a pkg-config to expose the Apache C API to Swift
- [CApache](https://github.com/modswift/CApache), a Swift Package Manager module
  you can import to get access to the above (requires a mod_swift installation)
- [Apache](https://github.com/modswift/Apache), optional, low-level Swift 
  wrappers for CApache
- Swift Package Manager scripts that allow you to build and configure&run
  Swift Apache modules:
  - `swift apache build`: runs `swift build` and turns the results into an
    Apache module (`mods_helloworld.so`)
  - `swift apache serve`: automagically creates an Apache configuration for the
    module (including a dev-SSL setup and HTTP/2, if available)
- a [Homebrew tap](https://github.com/modswift/homebrew-mod_swift) to quickly
  install it on macOS
- [Documentation](http://docs.mod-swift.org/) :-)

In short:

```shell
$ mkdir mods_helloworld && cd mods_helloworld
$ swift apache init
The Swift Apache build environment looks sound.

  module:    mods_helloworld
  config:    debug
  product:   /Users/helge/mods_helloworld/.build/mods_helloworld.so
  apxs:      /usr/local/bin/apxs
  mod_swift: /usr/local/opt/mod_swift

$ swift apache build
Cloning https://github.com/modswift/Apache.git
HEAD is now at 37f3038 Travis: use `swift build`
Resolved version: 0.2.0
Cloning https://github.com/modswift/CApache.git
HEAD is now at aa7d5b5 Tabs to spaces
Resolved version: 1.0.0
Compile Swift Module 'Apache' (8 sources)
Compile Swift Module 'mods_helloworld' (1 sources)

$ swift apache serve
Note: DocRoot /usr/local/var/www/htdocs
Starting Apache on port 8042/8442:
GET /helloworld/ 200 715 - 0ms
```

Note of interest: `0ms`, the duration of the request. Yes, it is that fast ;-)

### Installation

Head over to our new [documentation website](http://docs.mod-swift.org) for
all the details, but on macOS w/ Homebrew do this to get up&running:

```shell
brew tap modswift/mod_swift
brew install mod_swift
```

To check whether it worked:

```shell
$ swift apache validate
The Swift Apache build environment looks sound.

srcroot:   /Users/helge/dev/Swift/Apex3
module:    mods_Apex3
config:    debug
product:   /Users/helge/dev/Swift/Apex3/.build/mods_Apex3.so
apxs:      /usr/local/bin/apxs
mod_swift: /usr/local
swift:     3.1.0
cert:      self-signed-mod_swift-localhost-server.crt
http/2:    yes
```

## The new ApacheExpress

We split off **mod_swift** because we think it is generally useful as a
standalone component for various Swift server projects. Plugging into
"[The Number One HTTP Server On The Internet](https://httpd.apache.org)"
just makes more sense than trying to replicate its rich and proven 
functionality.

<a href="http://apacheexpress.io/" target="apex">
  <img src="http://zeezide.com/img/ApexIcon1024.svg"
       align="right" width="128" height="128" style="padding: 0.5em;"/></a>
However, we also have a neat, higher level, framework in the works:
[ApacheExpress](http://apacheexpress.io/).
It is based on what was included in the
[`mod_swift technology demo`]([https://github.com/ApacheExpress/ApacheExpress/tree/standalone-demo-2017-04-24]),
but much enhanced and interfaces with our
[ZeeQL](http://zeeql.io/) database access framework.<br />
Work is still in progress to finish that up, stay tuned.

## Summary

We think this is pretty cool stuff,
but we love feedback.
Join the mailing list, Slack channel or just drop us
an email to tell us why this is crap (or not?): [Slack](http://slack.noze.io).

<hr>

*P.S.: The original, self-contained, demo is still available in a branch:
[`standalone-demo-2017-04-24`](https://github.com/ApacheExpress/ApacheExpress/tree/standalone-demo-2017-04-24).*
