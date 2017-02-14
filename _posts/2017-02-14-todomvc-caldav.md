---
layout: post
title: 📆 CalDAV (and CardDAV) for Server Side Swift
tags: apache linux swift server side caldav carddav
---

As part of mod_swift we have been shipping 
[mods_todomvc](https://github.com/AlwaysRightInstitute/mod_swift/tree/master/mods_todomvc).
Which is an implementation of a [Todo-Backend](http://todobackend.com), a simple
JSON API to access and modify a list of todos.
Today we take Todo-Backend one step further: Access the todos using 
[CalDAV](http://caldav.org)!

## Todo-Backend

Checkout the website of [Todo-Backend](http://todobackend.com) to learn more
about the idea behind this project.
In short it defines a simple JSON API to maintain a list of todos and a lot of
example implementations in all kinds of languages and server frameworks.
We ship a mod_swift based implementation called
[mods_todomvc](https://github.com/AlwaysRightInstitute/mod_swift/tree/master/mods_todomvc).

mods_todomvc uses a simple in-memory store for the todos. It passes all the 
Todo-Backend tests. Todo-Backend comes with a small web client to test
the backend, this is how it looks like:

<center><img src="http://noze.io/images/screenshot-todo-mvc-redis-1.jpeg" alt="" 
             style="border: 1px solid #DADADA; padding: 4px;"/></center>

You can toggle the state, create news ones, delete them, change the name,
reorder them.

The JSON endpoint required to drive this is pretty simple. A bit of JSON
rendering and parsing, some GET, POST, PATCH and DELETE operations.
The implementation can be found in
[TodoMVCMain.swift](https://github.com/AlwaysRightInstitute/mod_swift/blob/master/mods_todomvc/Sources/TodoMVCMain.swift)
and is pretty straight forward 'Express Code'.

## Enter CalDAV ...

Todo-Backend is a nice thing, but we thought we take it a little further.
Instead of just implementing the trivial JSON-REST API,
we also added support for CalDAV.
[CalDAV](http://caldav.org/) is the IETF standard
([RFC 4791](https://tools.ietf.org/html/rfc4791)) for managing calendars and
todo lists.

What does that mean? Well it means, you can access *mods_todomvc*
not only using the TodoMVC web frontend shown above, but - wait for it -
with any existing todo list application that supports CalDAV.
Most importantly this covers the iOS Reminders application as well as the macOS
one, there are many more. Look at that:

<img src="http://zeezide.de/img/finished-todo-mvc-iOS-reminders-list-cut.png" />

Everything the TodoMVC web client / API can do is supported.
That is, you can create/delete todos, you can set the todo title,
you can mark them as done/undone, and you can reorder the list of todos.
You can't do other VTODO things, like setting priorities, attaching links, etc.
After all this is still hooked up to the very simple TodoMVC model.

There is only a single account. All people using your server would share the
same todos. Isn't that great?

### How to Configure iOS Reminders for TodoMVC

To configure go this route: 
iOS settings / Calendar / Accounts / Add Account / Other / Add CalDAV Account.
Enter any user/password/description and put `http://yourmac.local:8042/` into
the Server field.

On macOS: System Preferences / Internet Accounts / Add Other Account... /
CalDAV account / Popup 'Advanced'. Enter any user/password,
Server Address is `http://yourmac.local:8042/`,
Server Path is `/todomvc/`, put 8042 into port and deselect 'Use SSL'.

### Bonus: CardDAV Addressbook

Low hanging fruits: a read-only CardDAV addressbook. 
Access it from the iOS 'Contacts' application or any other CardDAV client.
It carries three builtin vCards, great stuff!
To configure go this route: 
iOS settings / Contacts / Accounts / Add Account / Other / Add CardDAV Account.
Enter any user/password/description and put `http://yourmac.local:8042/` into
the Server field. 
(on macOS use `Manual` and `http://yourmac.local:8042/todomvc/`)

## Implementation

The CalDAV support in this demo is by no means a fully conforming CalDAV
server. But it gets the job done and carries all the basics you would need
to build a proper server :-)

It is a little more code than the JSON protocol, but reasonable, especially
considering the gains w/ supporting an actual standard.
A lot of the code in there is generic and could be moved into a proper
framework.
Please do the right thing and use proper standards. It is not *that* hard!

Also included is a small wrapper for the XML parser that is part of Apache.


<hr />
Like it? You are welcome!

There is a [Slack team](http://slack.noze.io/) and a
[Mailing list](https://groups.google.com/d/forum/mod_swift)
you can join for discussion.

Have fun! *hh*
