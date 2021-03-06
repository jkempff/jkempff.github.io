---
layout: post
title: Create promise chains from arrays
category: javascript,promises,es2015
---

Yo. It's been a while since my last post. And this is nothing special, but something I quite often use and find very useful.

When working with promises, you sometimes need to wait for more than one promise to resolve. Most time [Promise.all][promise.all] will solve this for you.

```js
Promise.all(arrayOfPromises)
  .then(results => {})
  .catch(err => {}));
```

This works just fine, but there is one caveat: the promises in the array already have been created and are doing their stuff. But, if you have a chain of functions, and *rely on results from previous promise*, you can't use `Promise.all`.

## Reduce that promise

To fix this, you simply can use `Array.prototype.reduce`:

```js
const reducePromises = fns => fns.reduce(
  (chain, link) => chain.then(
    result => new Promise(link(result))
  ), Promise.resolve()
);
```

And use it as

```js
// This array usually is somehow generated, otherwise
// there'd be no need to use a reducer.
const chain = [
  () => (resolve, reject) => {
    return retrieveSomeInfo(resolve);
  },
  someInfo => (resolve, reject) => {
    const detail = someInfo.detail;
    return doSomethingWithDetail(detail).then(resolve);
  },
  ...
];

reducePromises(chain)
  .then(chainResult => {
    // Hoorraaay!
  })
  .catch(err => {
    // Oh no..
  })
```

Of course the example is pretty dumb, as you could just write it down as a normal promise chain. The good thing about `reducePromises` is, that you can easily *compose chains of logic*.

For those of you that are not yet familiar with ES2015 style JS, here is the same reducer in good'ol ES5:

```js
function reducePromises(fns) {
  return fns.reduce(function (chain, link) {
    return chain.then(function (result) {
      return new Promise(link(result));
    });
  }, Promise.resolve());
}

var chain = [
  function () {
    return function (resolve, reject) {
      ..
    }
  },
  ..
];

reducePromises(chain).then(..).catch(..);
```

And that's it. Promises ftw.

[promise.all]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise/all
