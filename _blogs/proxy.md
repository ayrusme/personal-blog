---
title:  "Proxy - Javascript"
date:   2020-11-29
permalink: proxy-js
layout: blog_post
categories: ["javascript"]
---
# Javascript Proxy

Javascript built-ins has a great feature called "Proxy", which is literally its namesake - a substitute. Proxies can be used in the place of the actual objects to achieve a `man-in-the-middle`. This proxy object will help us manipulate how the actual object itself behaves.

For example, let's take a look at this

```javascript
const someObj = {
    key: "value"
};
someObj.key = "hello world";
console.log(someObj.key) // hello world
```
Even though the object is a `const`, it will still store thew value `hello world` to the key. 

A proxy will be able to prevent this behaviour by overriding the object's getters and setters. Here is a sample snippet where the object's setter is overriden.

```javascript
let handler = {
  set: function(object, property, value) {
    // returning false indicates failure to set
    return false;
  }
};

const someObj = new Proxy({
    key: "value"
}, handler);

someObj.key = "hello world";
console.log(someObj.key) // value

```

The proxy prevents any modifications to the object because of the handler function. This can be extended to say prevent any modifications to specific keys inside the object, or even to track changes to the object. 


## Track changes to objects

A common usecase in cron jobs is to understand if something got updated. Suppose the previous state is persisted and the new state is fetched from some data source, writing `if-else` conditions to understand these changes is soo old school! Check out this working code which pushes the moodified keys into an array. 

<script src="https://gist.github.com/ayrusme/e57add78329fdae2c06940e409efdb8e.js"></script>

Elegant, isn't it? This can be extended to validate the keys when the values are getting set. It doesn't just stop there. Proxies can implement their own methods on top of overriding existing methods. They can add named lookup to arrays `arr.name` instead of `arr[0]` and so on. 

Proxies are powerful and they can help in certain niche situations where some client library is being exposed to the world but you don't want anyone to override the library's methods aka `monkey patching`. All these can be handled through proxies. 

More awesome examples can be seen [here](https://github.com/mikaelbr/awesome-es2015-proxy)

If you have any queries about this page, or you want to tell me about something you've built, [hit me up](mailto:hello@suryaraman.dev)

[MDN Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)