+++
author = "Eric Marsh"
comments = true
date = "2016-09-29T13:29:20-05:00"
draft = false
image = "images/async.png"
menu = ""
share = true
slug = "dont-use-doc-write-for-script-tags"
tags = ["javascript", "performance"]
title = "Don't use Document.Write() for Script Tags"

+++

I saw [this article](http://blog.dareboost.com/en/2016/09/avoid-using-document-write-scripts-injection/) linked on [Hacker News](https://news.ycombinator.com/item?id=12604544) about reasons not to use document.write() to insert script tags. The primary one being that Chrome is [not going to support it.](https://developers.google.com/web/updates/2016/08/removing-document-write)

### What should you do?

Ok, great. Now you know about it, what are you going to do? 

1. The code change is pretty straight forward. As a user points out in the [comments](https://news.ycombinator.com/item?id=12604742), you can just use DOM manipulation rather than using write.

```javascript
var _script = document.createElement('script');
_script.src = '//blablabla.bla/bla.js';

_script.async = ''
(document.body || document.getElementsByTagName('body')[0])
      .appendChild(_script);
```

2. Search your header, and your footer partials for any `document.write` commands.
3. If you are using Google Tag Manager you will want to go through any custom Javascript tags to see if there are any `document.write()` commands.

### Should you add async?

Yes, definitely. According to [caniuse.com](http://caniuse.com/#feat=script-async), roughly 92% of browsers support the attribute. There is really no reason not to.

### Sounds great, but what does async do?

You can think of he async attribute telling the JavaScript file to load in background and not block rendering of the rest of the file (if possible). This will allow the file to run only when it has been downloaded and will make the page appear to load faster.

If you want a bit more detailed read about async, take a look at [css-tricks.com article on the subject.](https://css-tricks.com/thinking-async/)