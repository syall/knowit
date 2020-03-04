---
title: "The Browser as a Program"
date: 2020-03-03T18:36:15-05:00
draft: false
---

> The internet has connected us in previously unimaginable ways, blah blah, blah blah blah, insert cliché.
>
> -- Someone on the Internet

The internet has been evolving over decades now, leading to a complex web of legacy applications coexisting with cutting edge innovations.

However, for end users today, the internet is a black box that can be accessed with just a power button and a click on a browser icon.

*Wait.*

Have you ever taken a moment to think about what a browser actually is?

**A browser is just a computer program.**

The thought never passes our minds since the browser is so integral, even equivalent, to computer usage for most people: the browser just is.

People download programs and apps, but a browser always seems to be in its own special category.

In this article, we will be explored in three perspectives that showcase why browsers are such a technically interesting class of programs.

## A Programming Languages Perspective

If I go to [`www.example.com`](https://www.example.com), what does the browser receive?

By opening Chrome Developer Tools and looking at the `Sources` tab, there is only one file being sent: `index.html`.

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

When I visit that website, that `index.html` file is visualized in my browser.

What does this mean technically?

It means the browser is a program that can render an HTML file to the screen.

Not only this, when we take a look at the `index.html` file, there is also embedded CSS.

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

So, the browser can not only render HTML but also render CSS.

From a user perspective, the browser seems to have a magic box where HTML and CSS just appear.

From a technical perspective, the feat is quite impressive as the browser needs to interface with the native graphics engine of the operating system accurately.

Beside HTML and CSS, there is the notorious beast of the internet: JavaScript (JS).

Riddled with history due to evolving standards coupled with legacy support, the language of the web is extremely complex!

Because of this, browser support for JS is a very nuanced topic, but the big picture is that each browser has a toolchain with compilers, optimizers, etc. completely built-in for JS alone.

However, the complexity compounds as a system because the JS needs to be able to interact with the HTML and CSS rendering seemlessly.

## An Operating Systems Perspective

Most Browsers simulate or implement a file system in which content is loaded by URLs.

[Way to Go](https://way-to-go.syall.work/) is a simple static game with these main resources when requested:

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

How does the browser know to get these specific files from the server?

Based on a standard set by history for a document-based internet, requesting the URL `https://way-to-go.syall.work` will cause the server to send an `index.html` file first unless otherwise specified.

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

While parsing that HTML file, the browser will load the dependencies by definition of `src` in the `script` tags and `href` in the `link` tags, sending more requests to the server until the browser has all the resources needed to fully render the page.

Since the browser requests these files so freely during the parsing process, one might wonder:

*Does this mean that I can request any public resource with the correct URL?*

Yes! Like we've been doing, these resources can be accessed via URLs.

But let's take a step back: What is a URL?

By definition, a URL stands for Uniform Resource Locator, a location of a resource.

All of these URLs we have been using are just specifying a server domain and a resource defined at that path.

The path on the server-side could be implemented with a complex framework or simply point to a static file but either way, the path specifies a request to a location on the server that the server can respond to!

For example, the game is published from the contents of the `master` branch, meaning the [whole entire repository](https://github.com/syall/way-to-go) is requestable (except `CNAME` which is used only in deployment).

[`https://way-to-go.syall.work/README.md`](https://way-to-go.syall.work/README.md) specifies the `README.md` file and consequently returns the `README.md` when requested.

If you try to locate a resource that [`/does-not-exist`](https://way-to-go.syall.work/does-not-exist), the server will respond with a `404` page unless otherwise routed.

Now for one final question:

*If the URL is just a path that specifies a location, can the browser display files from the local computer the browser is running on?*

Yes! At least in the major browsers that implement that functionality.

In Chrome and most Chromium based browsers, you can just type `/` and it will show the `/` directory of your computer with the respective links to the subdirectories and files.

Specifying a local file or directory via URL is also simple: `file://path/to/resource`.

Essentially, the browser is a file explorer for the local computer, able to navigate through directories and render files (though mostly as plain text).

## A Networking Perspective

URLs specify paths and locations, but how does the computer actually send the request to another machine and how does that machine understand what is sent?

At the fundamental level of internet (short for interconnected network), data needs to be sent from device to device.

As defined by abstractions denoted in standards such as the [OSI model](https://en.wikipedia.org/wiki/OSI_model), anything from physical wire, wireless communication, and inbetween can be used to route data from one source to another.

All of these messages between different devices are standardized as communication protocols so devices that use the same protocol can understand each other.

Browsers are also programs that implement these different types of communication protocols, meaning they are able to communicate with different types of devices!

The main protocols Chrome implements are: `HTTP`, `HTTPS`, `FILE`, and `FTP`.

The majority of the internet uses `HTTP` and `HTTPS` protocols to receive website resources such as the `index.html`.

The `FILE` protocol allows the browser to access local files that we saw in the [Operating Systems](#an-operating-systems-perspective) section.

`FTP` stands for File Transfer Protocol and is another protocol that sends files between a client and a server.

Having protocols implemented mean that the browser is not just a standalone program but also a capable networking application.

## Complexity

Browsers as programs contain code that span over a majority of fields in both Computer Science and Software Engineering, a class of programs that are unique in the span of functionality.

From a programming languages perspective, major browsers have to implement at least three evolving languages (HTML, CSS, and JavaScript) as well as the interactivity between them, balancing heaviness in complexity with speed for user experience.

From an operating systems perspective, the browser can act as an operating system interface, especially in the situation where all work is done through web applications or built-in tools (possible in Chrome DevTools `Filesystem` tab under `Sources`).

From a networking perspective, the browser implements complex communication protocols so it can network with other devices and the internet.

With the onset of [WebAssembly](https://webassembly.org/) and the existence of [ChromeOS](https://www.chromium.org/chromium-os), the importance of the browser will only increase as apps are ported from native to web applications, reaching a wider array of users in a familiar environment: the browser.
