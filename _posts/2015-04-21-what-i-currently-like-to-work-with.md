---
layout: post
title: What I currently like to work with
category: frontend
---

Of course, this is constantly changing and may differ any time you ask me about it. And no: the following does not represent the newest and latest shit that may be around. I'm just writing about the tools I currently enjoy working with.

### [webpack][webpack]

I recently switched from [require.js][requirejs] to [webpack][webpack]. Why? The one thing I don't like about requirejs is it's configuration. I always had problems setting this %&*§ up... My guess is, I just did it wrong, or missed some parts in the documentation, but when testing webpack, it just worked.

Also: You can use amd **or** commonjs style modules, which also says: bower has become irrelevant. Now I only need npm for all my frontend dependencies.

Beside that, it is much more powerful! For example you can use a `jsx-loader`, that will automagically turn your fancy react.jsx code into plain javascript.

You can even require .sass or .less files if you wish to and webpack will load them, if needed.

And no: this will not blow up your `main.js` file, since it automatically creates so called chunk files, which are only loaded, if the user requires that module. Of course, you'll need to require stuff asynchronously, if you want to use this.

### [ReactJs][reactjs]

Not yet having built something with it that's in production, I am a bloody beginner. But there is something in the making.. so be prepared!

There are many things about React that I like. One of them is, that you can use the exact same component on the client, as on the server. If a user hits the page, you can serve him rendered markup. Therefore he sees something instantly, even without javascript being enabled. The same goes for search engine bots. Then, react.js does it's magic and wraps itself around the markup. Now, any action a user does, e.g. navigating, is handled and rendered by the client.

And if you set your app up, following some guidelines, even a form submission - that normally is completely asynchronous, shows a fancy loader and shakes itself when something goes wrong - can work for people that have disabled javascript. Without the shaking or being asynchronous, of course.. But thats a trade I'd take.

React also tells you, if you've fucked up and your server rendered something differently than the browser. Its very useful warnings in the `console` are something I've never seen before, anywhere.

Check out this talk by Ryan Florence, showing many demos and nice features:

<iframe width="100%" height="315" src="https://www.youtube-nocookie.com/embed/z5e7kWSHWTg" frameborder="0" allowfullscreen></iframe>

### [Silex][silex]

Silex is "The PHP micro-framework based on the Symfony2 Components" by sensiolabs.  
I am only using it for a prototype yet, but the setup was so simple, I think of using it for any kind of prototype that requires more than one page.

If you are about to build a webservice or a website without the need of a backend, that needs to be lightweight, you should give it a try:
At DrupalCon Amsterdam 2014, Crell did a talk about how they used Silex in combination with ElasticSearch, to build a super fast RESTApi.

Besides [many other goodies][silexgoodies] from symfony2, you get [Twig][twig] as a powerful templating engine.

The thing that got me into testing it, was their hello world example:

{% highlight php %}
<?php
require_once __DIR__.'/../vendor/autoload.php';

$app = new Silex\Application();

$app->get('/hello/{name}', function($name) use($app) {
    return 'Hello '.$app->escape($name);
});

$app->run();
{% endhighlight %}

Yes, I know.. this doesn't show anything and even if my job was building "hello world" apps, I'd use something else than php.. But this just looked much more pretty than any php code I've written so far. So I went for it.

### ..and thats it for now!

Nope, it's not. I also enjoy using the [Silverstripe][silverstripe] Framework and CMS (not to be confused with Microsofts Silverlight!).

My project [Reflektor M][reflektorm] is based on this framework, but this is going to be another article.


[webpack]: http://webpack.github.io/
[requirejs]: http://requirejs.org/
[reactjs]: https://facebook.github.io/react/
[silex]: http://silex.sensiolabs.org/
[silexgoodies]: http://silex.sensiolabs.org/documentation
[twig]: http://twig.sensiolabs.org/
[silverstripe]: http://www.silverstripe.org/
[reflektorm]: http://reflektor-m.de/