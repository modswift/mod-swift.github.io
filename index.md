---
layout: default
title: mod_swift ✭ Server Side Swift the right way!
tags: apache swift server side mod_swift
---

<p>
  <img src="https://img.shields.io/badge/apache-2-yellow.svg" />
  <img src="https://img.shields.io/badge/swift-3-blue.svg" />
  <img src="https://img.shields.io/badge/swift-4-blue.svg" />
  <img src="https://img.shields.io/badge/os-macOS-green.svg?style=flat" />
  <img src="https://img.shields.io/badge/os-tuxOS-green.svg?style=flat" />
  <img src="https://img.shields.io/badge/works%20on-Raspberry%20Pi-CA0B3D.svg?style=flat" />
  <img src="https://travis-ci.org/modswift/mod_swift.svg?branch=master" />
</p>

<b>mod_swift</b> allows you to write native modules
for the
<a href="https://httpd.apache.org/">Apache Web Server</a>
in the 
<a href="http://swift.org/" target="swift">Swift</a>
programming language.

<center><em>Server Side Swift the right way</em>.</center>

We recommend using it with the Homebrew Apache 2.4 on macOS:

<center><code>brew install httpd --with-mpm-event --with-http2</code></center>

or the same on Linux (tested with Ubuntu 16.04).

### Shows us some code!

We provide a few examples:
[a 'raw' Apache Module](https://github.com/AlwaysRightInstitute/mod_swift/tree/master/mods_baredemo),
[a demo for the bundled Express](https://github.com/AlwaysRightInstitute/mod_swift/tree/master/mods_expressdemo),
as well as a
[TodoMVC backend](https://github.com/AlwaysRightInstitute/mod_swift/tree/master/mods_todomvc").
But here you go, the "standard" Node example, a
HelloWorld webpage:

```swift
func expressMain() {
  apache.onRequest { req, res in
    res.writeHead(200, [ "Content-Type": "text/html" ])
    try res.end("&lt;h1&gt;Hello World&lt;/h1&gt;")
  }
}
```

Middleware using Express features like Mustache templates or
JSON support:

```swift
let app = apache.express(bodyParser.urlencoded(), 
                         cookieParser(), session())
  
app.get("/express/cookies") { req, res, _ in
  try res.json(req.cookies)  // returns all cookies as JSON
}

app.get("/express/") { req, res, _ in
  let tagline = arc4random_uniform(taglines.count)

  let values : [ String : Any ] = [
    "tagline"     : taglines[tagline],
    "viewCount"   : req.session["viewCount"] ?? 0,
    "cowOfTheDay" : cows.vaca()
  ]
  try res.render("index", values)
}
```

Access a SQL database using Apache 
[mod_dbd](https://httpd.apache.org/docs/2.4/mod/mod_dbd.html):

```swift
guard let con = req.dbdAcquire()                 else { return ... }
guard let res = con.select("SELECT * FROM pets") else { return ... }

while let row = res.next() {
  req.puts("&lt;li&gt;\(row[0])&lt;/li&gt;")
}
```

The documentation can be found here:
<a href="http://docs.mod-swift.org/">docs.mod-swift.org</a>.

<hr />

<div class="posts">
  {% for post in site.posts %}
    <article class="post">

      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>

      <div class="entry">
        {{ post.excerpt }}
      </div>
      
      <div class="date">
        <table border="0" width="100%"> <!-- old skool -->
          <tr>
            <td>{{ post.date | date: "%B %e, %Y" }}</td>
            <td style="text-align:right;"><a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a></td>
          </tr>
        </table>
      </div>
    </article>
  {% endfor %}
</div>
