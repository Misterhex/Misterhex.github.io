---
tags: software-engineering nodejs
---

Recently, at work, i was tasked with providing a training session to [NodeJS](https://nodejs.org/en/) to help adopt `node` to our organization.

This post is a recap on what i covered on:

### What is NodeJS?
![cname file content]({{ site.url }}/assets/img/nodejs.png) 

- Javascript running on the server.
- Run on Chrome V8 javascript engine.
- Single-threaded event loop, Non-blocking I/O.
- [NPM](https://www.npmjs.com) for package management.
- Currently version 8.4.0

### Architecture
![cname file content]({{ site.url }}/assets/img/nodejs_architecture.png) 

`NodeJS` is powered by 2 major componenent, [V8](https://github.com/v8/v8) and [libuv](https://github.com/libuv/libuv).

`libuv` brings `asynchronous I/O` to `node`. It should also be noted that the [kestrel](https://github.com/aspnet/KestrelHttpServer) ( aspnet core http server ) is also leveraging `libuv`.

`v8` is the javascript engine in `node`.

### Why Non-Blocking I/O?
![cname file content]({{ site.url }}/assets/img/nodejs_event_loop.png) 

NodeJs is designed with non-blocking evented I/O, all I/O calls are returned on `callbacks`. This is very different from traditional frameworks in the sense when an I/O operation is happening, no application threads is blocked.

The nature of most modern web servers are mostly I/O bound by nature. Blocking I/O is not scalable because each I/O would hold up a thread. For example, if we have 1000 I/O calls happening, then there are at least 1000 OS threads created, consuming 1000 * 10 MB of memory, context-switching in the cpu would also be really high. This is an inefficient concurrency model. Most languages/frameworks are not designed with this in mind.

- Non-blocking I/O supports tens of thousands of concurrent connections.
- Non-blocking IO single-threaded concurrency model.
- Can scale to high number of concurrent requests.
- Blocking I/O handle concurrency by having more threads. 
- More threads = more memory and cpu context-switching.
- But Non-blocking IO without language support make code hard to read/write.

### Non-Blocking I/O frameworks:
- NodeJS
- Go [https://golang.org](https://golang.org)
- C# [https://www.microsoft.com/net/core#macos](https://www.microsoft.com/net/core#macos)
- Play framework. [https://playframework.com](https://playframework.com)
- Java NIO [https://docs.oracle.com/javase/7/docs/api/java/nio/package-summary.html](https://docs.oracle.com/javase/7/docs/api/java/nio/package-summary.html)

### Language support for concurrency
Language support is important to prevent *spaghetti* code when writing concurrent code. The [callback hell](http://callbackhell.com) in nodejs early days is a good example.

With the new addition of `async` and `await` in nodejs 8, concurrency support is awesome. [https://blog.risingstack.com/mastering-async-await-in-nodejs/](https://blog.risingstack.com/mastering-async-await-in-nodejs/)

IMO, `Golang` & `C#` are by far the best languages with concurrency support baked into the language.

### NPM
- Package manager for javascript.
- Used to install node program/modules.
- Specify dependencies in a file call package.json.
- Modules get installed into `node_modules` folder.
- NPM version 5 generate `package.lock.json` to guarantee similar dependencies are always restored.
- Alternative to npm: [Yarn](http://yarnpkg.com)

### Conclusion
NodeJs is a pretty decent choice for building apps that require high I/O scalability.
