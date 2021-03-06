---
title: "Browsers"
date: 2020-03-03T18:36:15-05:00
draft: false
---

> The internet has connected us in previously unimaginable ways, blah blah, blah blah blah, insert cliché.
>
> -- Someone on the Internet

The internet has been evolving for decades now, leading to a complex web of legacy applications coexisting with cutting edge innovations.

However, for end users today, the internet is a black box that can be accessed with just a push of a power button and a click on a browser icon.

*Wait. What is a browser, actually?*

**A browser is just a computer program.**

The thought never passed my mind since the browser is so integral, even equivalent, to computer usage for most people.

We download programs and apps, but browsers seem to be in a unique position in our mind.

In this article, we will explore how interesting and capable the browser is from three technical perspectives.

## A Programming Languages Perspective

Let's first assume that I have no idea what a browser does.

If I navigate to [`www.example.com`](https://www.example.com), how does the browser respond?

By opening Chrome Developer Tools and looking at the `Sources` tab, there is one file that is received: `index.html`.

```html
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;

    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 2em;
        background-color: #fdfdff;
        border-radius: 0.5em;
        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);
    }
    a:link, a:visited {
        color: #38488f;
        text-decoration: none;
    }
    @media (max-width: 700px) {
        div {
            margin: 0 auto;
            width: auto;
        }
    }
    </style>
</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is for use in illustrative examples in documents. You may use this
    domain in literature without prior coordination or asking for permission.</p>
    <p><a href="https://www.iana.org/domains/example">More information...</a></p>
</div>
</body>
</html>
```

But the browser doesn't just receive this file: the browser also visualizes the `index.html` file on my screen.

When we take a deeper look in the `index.html` file, there is also embedded CSS in the `style` tag.

```html
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    /* ... CSS Here! ... */
    </style>
</head>
<!-- ... -->
```

The browser supports both HTML as a mark-up language and CSS as a styling language!

From a technical perspective, the browser is a program that processes HTML and CSS code to interface with the native graphics engine of the operating system.

Besides HTML and CSS, there is the notorious beast of the internet: JavaScript (JS).

Riddled with history due to evolving standards coupled with legacy support, the standard for the language is extremely complex and has fragmented implementation between browsers.

Despite the divides, the big picture is that each browser has its own dedicated JS toolchain (parsers, compilers, optimizers, etc.) that can interpret all of the nuances the standard allows.

As a architectural unit, the complexity compounds as the implementations of these living programming languages have to not only interoperate at initial load time but also respond dynamically to user activity.

## An Operating Systems Perspective

Most browsers implement or at least simulate a file system for content loaded in by URLs, storing the data as sources.

For example, [`Way to Go`](https://way-to-go.syall.work/) is a simple static game that responds with these relevant resources:

```text
way-to-go
├── css
│   └── index.css
├── index.html
└── src
    ├── animation.js
    ├── game.js
    ├── index.js
    └── utils.js
```

How does the browser know to get these specific files from the URL?

Based on a historic standard (for a document-based internet), requesting the URL [`https://way-to-go.syall.work`](https://way-to-go.syall.work) will cause the server to send an `index.html` file unless otherwise specified.

While parsing `index.html`, the browser will load the dependencies by definition of `src` in the `script` tags and `href` in the `link` tags, sending more requests to the server until the browser has all the resources needed to fully render the page.

```html
<!-- index.html -->
<!-- ... -->
<head>
    <!-- ... -->
    <link rel="stylesheet" type="text/css" href="/css/index.css">
    <!-- ... -->
</head>
<!-- ... -->
<body>
    <!-- ... -->
    <script src="/src/utils.js"></script>
    <script src="/src/animation.js"></script>
    <script src="/src/game.js"></script>
    <script src="/src/index.js"></script>
</body>
<!-- ... -->
```

Since the browser requests these files during the parsing process so freely, one might wonder:

*Can any public resource be requested with the correct URL?*

The answer is a resounding yes.

But wait a minute: *What exactly is a URL?*

URL stands for Uniform Resource Locator, a standard to define a location of a resource.

On the user's side, all of the URLs we use specify a domain (the abstraction for a computer) and define a location at that domain.

On the server's side, the response can range in implementation (sending a simple file, computing business logic in a web framework, etc.) but usually ends with some data sent to the client.

The game `Way to Go` has its content published with a simple GitHub pages static file server, meaning the [whole entire repository](https://github.com/syall/way-to-go) is requestable (except `CNAME` which only used in deployment).

For example, the URL [`https://way-to-go.syall.work/README.md`](https://way-to-go.syall.work/README.md) specifies the `README.md` file at the `way-to-go.syall.work` domain.

On the other hand, trying to locate a resource that [`/does-not-exist`](https://way-to-go.syall.work/does-not-exist) will respond with a `404` page unless otherwise routed.

Now for one final question:

*If the URL is just a path that specifies a location, can the browser display files from the local computer the browser is running on?*

Yes! At least in the browsers that implement that functionality.

In [Chromium](https://www.chromium.org/Home)-based browsers, you can just type `/` in the URL bar and the `/` directory of your local computer will appear in the browser.

Specifying a local file or directory via URL is also simple: `file://path/to/resource`.

Essentially, the browser is a file explorer for the local computer, able to navigate through directories and render files (though mostly as plain text).

There are also other browser APIs that can interface with native operating system functionality (wifi, bluetooth, etc.), but the file system is the most used.

There is also complex concurrency management in browsers, most visibly shown in tabs and service workers, but I will not dive into it due to lack of domain knowledge.

## A Networking Perspective

URLs specify paths and locations for a request, but how does the computer actually send the request to another machine, and how does that machine understand what was sent?

As defined by abstractions denoted in standards such as the [OSI model](https://en.wikipedia.org/wiki/OSI_model), anything from physical wire, wireless communication, and inbetween can be used to route data from one source to another in a network.

All of these messages between different devices are standardized as communication protocols so devices that use the same protocol can understand each other.

Browsers are programs that implement a subset of these communication protocols, meaning browsers are able to communicate with different types of devices that also implement the respective protocols.

The most commonly used protocols used in Chromium-based browsers are: [`HTTP`](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol), [`HTTPS`](https://en.wikipedia.org/wiki/HTTPS), [`FILE`](https://en.wikipedia.org/wiki/File_URI_scheme), and [`FTP`](https://en.wikipedia.org/wiki/File_Transfer_Protocol).

The majority of the internet uses `HTTP` and `HTTPS` protocols to receive website resources, such as `index.html`.

The `FILE` protocol allows the browser to access local files, as shown in the [Operating Systems](#an-operating-systems-perspective) section.

`FTP` stands for File Transfer Protocol which exists to transfer files between a client and a server, very aptly named.

The browser is not only a standalone program but also a capable networking application due to the implementations of these different protocols.

## Complexity

Spanning over the most technically challenging fields in both Computer Science and Software Engineering, browsers are a class of programs that are unique in both functionality and complexity.

From a programming languages perspective, major browsers have to implement at least three evolving languages (HTML, CSS, and JS) as well as the interactivity between them, balancing heaviness in complexity with speed for user experience.

From an operating systems perspective, the browser can act as an operating system interface by implementing a file system and scheduling concurrent processes and threads.

From a networking perspective, the browser implements complex communication protocols so it can both connect to and understand other devices in a network.

With the onset of [WebAssembly](https://webassembly.org/) porting native apps to the web and [ChromeOS](https://www.chromium.org/chromium-os) pushing for a  cloud-based operating system built on web technologies, the capability and complexity of the browser can only trend upward over time.
