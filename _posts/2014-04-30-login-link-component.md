---
layout: post
title: Login link component
tags: [emberjs]
fullview: true
---

This post describes how to create emberjs component `login-link`. This component acts like normal link or opens a pop-up depending on situation.

In [stepic](http://stepic.org) we do support anonymous users — you can go through steps, solve quizzes and progress will be saved (as long as cookie alive). But anyway we gently nudge user to register or login. We have "please login" links spread over application. To make login process seamless and not bothering we redirect user back after login.

Problem is that stepic may also be embedded into iframe on third-party website. In this case we don't want to redirect user to login page (many reasons..) but open a pop-up window with login page.

Decision we came up with is to implement `login-link` component and use it instead of `{{"{{link-to ..."}}}}`.

Here is source code:

<script src="https://gist.github.com/H1D/edc5aa74f4ef8c35e43c.js"></script>

And here is how we use it:
```
To save your progress please {{"{{#login-link"}}}}log in{{"{{/login-link"}}}} first.
```

###How it works
`login()` method returns promise which resolving only when `App` really has `user` object loaded.

`LoginLinkComponent` acts like normal link when `App` is not embedded (see `click()`). But when `App` is embedded we trying to open pop-up with login page then just calling `login()` on `ApplicationController` every 600ms. When login succeeded `App.reset()` do the rest for us.

To access `ApplicationController` from `LoginLinkComponent` we use emberjs injections.

Depending on auth implementation in your app you may need to access pop-up window (to get unique token for example). In that case instead of calling `login()` you can check hidden input inside pop-up window like so: 
```
popup = @get('login_popup')
token = popup.document.getElementById('auth_token')	
if token 
  ...
```