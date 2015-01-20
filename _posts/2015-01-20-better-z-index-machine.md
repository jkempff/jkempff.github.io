---
layout: post
title: Better Z-Index Management
category: SASS
---

This will make one tiny problem go away!

<p class="bg-light-gray small pam">
Before posting, I noticed smashingmagazine.com <a href="http://www.smashingmagazine.com/2014/06/12/sassy-z-index-management-for-complex-layouts/">wrote about this</a> some months ago. I decided to put it up anyways, because my approach uses a default index list, which therefore is slightly more practical.
</p>

#### The old way

Some years ago, I read somewhere (can't remember where and when) that `z-index: 99999;` is "a webdevelopers desperate cry for help".

The article was about z-index management and she/he/they suggested to use variables, to get rid of those z-index massacres:

{% highlight scss %}
$z-index-header: 1;
$z-index-main:   2;
$z-index-modal:  3;
// ..and so on
{% endhighlight %}

..so I started using this approach. And it worked out pretty good!

Imagine a new feature "Feedback Overlay" that has to be above everything *except* the header and maybe some warning modals that might pop up: Having multiple, fixed elements overlapping each other, spread all over the project, this could end in a desaster.

But with those `$z-index-variables`, all existing indexes are in one place.

----

Then I worked on a project, managing about 20 different indexes.

When I had to add a new index at the very bottom, I had to rewrite the entire list..  
Nobody likes this! So it made me think of an even better way to manage z-indexes:

#### The new way

Now I use a *list of named z-index* and a tiny scss helper function:

{% highlight scss %}
$default-indexes: 
  main,
  header,
  footer,
  navigation,
  modal;

@function zindex($name, $list: $default-indexes) {
    @return index($list, $name);
}
{% endhighlight %}

Global usage:

{% highlight scss %}
.header {
    z-index: zindex(header); // 2
}

.main {
    z-index: zindex(main); // 1
}
{% endhighlight %}

Within a css module, you can define a separate list and use that instead:

{% highlight scss %}

$module-z-indexes: head, body, footer;

.module__footer {
    z-index: zindex(footer, $module-z-indexes); // 3
}

{% endhighlight %}

If you prefer, a mixin can help too:

{% highlight scss %}
@mixin zindex($name, $list: $default-indexes) {
    z-index: zindex($name, $list);
}
{% endhighlight %}

Now you only have to maintain your index lists and never have to care about the actual indexes.

It's simple and it won't save the world, but it made me happy.