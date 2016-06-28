---
layout: post
title:  "Keep on Moving - ES5 -> ES6 Tips"
date:   2016-06-20 10:00:00 +0100
author: Jonny Wildey
---

A few months ago we decided to fully commit to the new [**ES6**](https://github.com/lukehoban/es6features). Although the transition has been relatively smooth I thought I'd share some of the problems we found and some of the solutions we had to them.

## lodash

Probably the biggest reason migration has been so easy is that we tend to use [lodash functions](https://lodash.com/) whenever possible. A completely unintended consequence of this is that the majority of our code is very close to being in ES6 format. The majority of our refactoring is removing _self_ and adding lambdas when useful:

**ES5**

```javascript
var self = this;
self.exaggerate = function(str) {
  return str + '!'
};
var statements = ['cool', 'no worries', 'sure'];
var exaggeratedSentences = _.map(statements, function(st) {
    return self.exaggerate(st);
});

```

**ES6**

```javascript
this.exaggerate = str => str + '!';
const statements = ['cool', 'no worries', 'sure'];
const exaggeratedSentences = _.map(statements, st => this.exaggerate(st) );

```

## Put a lid on it

By the time we decided to move to [Node 6](https://github.com/lukehoban/es6features) we already had a substantial codebase, so migrating all of our code to the new norms was not going to be possible. We adopted an approach that allowed us to move towards the full implementation without having to do it right now. How do we do this?

To a certain extent we use linting, but in a repo with 20 files using ES6 and 160 not, this tends to flood your terminal with warnings rather than direct you to the problems that should be solved.

We decided to create a set of tests called _lids_. These run through our source files, looking for ES6 files. All functions and files using ES6 syntax should have a
`'use strict';` so this is pretty easy to implement. Every month or so we set a limit on how many non ES6 files there should be in the repo. All new commits must pass these tests. So if I try and push a commit where my files are full of _var_* I'll see:


![Delicious error](/images/keep-moving/error.png)


*Is the plural of var _vars_ or _var_?


# Worthwhile?

This was quick to implement, the majority of the work being on file navigation. It's been surprisingly effective, with all of the developers being caught out every now and then. And in terms of ES6 adoption, view this unnecessary graph!

![Downward sloping graph](/images/keep-moving/useful-graph.png)


## When do I refactor?

We originally made a rule that if you touched a file, you were responsible for refactoring it. Unfortunately this tended to create some HUGE pull requests and be incredibly unfair for anyone who had a task that made minor changes to multiple files. Now we tend to refactor one or two files per pull request. It doesn't muddy up the original intent of the PR and is still progress.

## 🍲 Takeaways

If you wanted our approach to this summed up in 3 lines.

1. 🚄 When writing code, future-proofing should be a concern.
2. 🕗 Refactor slowly, but surely.
3. 🚨 Create tests to force yourself to adhere to new standards.

Got any comments or know a better way of doing this? [Hit us up on our new Twitter](https://twitter.com/TrussleTech)
