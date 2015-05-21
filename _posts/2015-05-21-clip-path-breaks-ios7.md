---
layout: post
title: CSS that will break any iOS 7 Browser
category: CSS
---

The following code will cause any iOS 7 browser (they are all the same: safari webkit, even chrome) to crash.

{% highlight css %}
@-webkit-keyframes {
    0% {
        -webkit-clip-path: polygon(15px 99px, 30px 87px)
    }
    100% {
        -webkit-clip-path: polygon(12px 99px, 10px 87px)
    }
}
{% endhighlight %}

It doesn't matter what polygon you are trying to create, just `polygon` will break it.

The good news is, afaik it only crashes when using `polygon()`, not with e.g. `circle()`.

The bad news is obvious.. This was an hour of testing and it was not worth it.