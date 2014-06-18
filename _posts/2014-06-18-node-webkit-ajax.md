---
layout: post
title: node-webkit ajax request
tags: [node-webkit]
fullview: true
---

I'm really implressed by how Notrous.IO used [node-webkit](https://github.com/rogerwang/node-webkit) to create their desktop app. So recently I was playing around with [node-webkit](https://github.com/rogerwang/node-webkit). Have to say it's awesome!

But there is no examples on how to do simple AJAX request using it! Naive `$.ajax()` doesn't work =(

## Solution
After hour of googling I finally understood that I have to figure out how to get `jQuery.ajax()` working in `node.js` environment (since `node-webkit` based on it).
First you have to install `jQuery` and `xmlhttprequest`: `npm i xmlhttprequest jquery --save`.
Then you must configure `jquery`: 
```
var $ = require('jQuery');
var XMLHttpRequest = require('xmlhttprequest').XMLHttpRequest;

$.support.cors = true;
$.ajaxSettings.xhr = function() {
    return new XMLHttpRequest();
};
```

Now you can use `$.ajax()` as usual!