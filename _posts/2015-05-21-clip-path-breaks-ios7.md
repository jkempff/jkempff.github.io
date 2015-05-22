---
layout: post
title: CSS that will break any iOS 7 Browser
category: CSS
---

_UPDATE:_

this only happens, if you have an error on the `polygon` or `circle` or what else you've got in the clip-path definition:

`polygon(50% 50%, 10% 10%, 0%, 0%)`

Note the last, missplaced comma!

Nevertheless, this should not bring an entire browser down..

Check the update: [Demo][1]


The following code will cause any iOS 7 browser (they are all the same: safari webkit, even chrome) to crash.
(example code has been updated)

{% highlight css %}
@-webkit-keyframes {
    0% {
        -webkit-clip-path: polygon(15px 99px, 30px 87px)
    }
    100% {
        -webkit-clip-path: polygon(12px 99px, 10px, 87px)
    }
}
{% endhighlight %}


<span style="text-decoration: line-through">It doesn't matter what polygon you are trying to create, just `polygon` will break it.</span> The wrong placed comma at `100%` actually does it.

<span style="text-decoration: line-through">The good news is, afaik it only crashes when using `polygon()`, not with e.g. `circle()`.</span> Also wrong.

The bad news is obvious.. This was an hour of testing and it was not worth it.

[1]: /examples/clip-path-breaks-ios7/