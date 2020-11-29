---
title: Migrating to Gatsby
date: "2020-11-29T07:26:03.284Z"
description: "Moving a blog from Jekyll to Gatsby"
categories: [code]
comments: true
---

# Why?

I can hear you already, "Didn't you **JUST** make your blog in Jekyll? Why are you switching?". Well, I realized that while Jekyll is nice and low-effort I actually wanted my blog to be cool. Gatsby has support for a ton of modern features which are harder to find in Jekyll. Gatsby uses React which makes for some nice practice. Even though I said I didn't want to code and just write content, I realized I was wrong and I actually like writing code :)

Gatsby still fills the other requirements I set like markdown content, easily deployable. It has the added benefit that it uses Javascript tooling which I am far more experienced with than Ruby tooling.

# Starting off

I started by picking out a [nice theme](https://www.gatsbyjs.com/starters/renyuanz/leonids) and promptly started tearing it apart and putting different pieces in to create something custom. With React this was a lot easier for me than it was in Jekyll.

I played around with some styling, adding some buttons for social info. For now this is fine, later I plan to add some more advanced stuff like search, rss feed, ... We'll see when that happens though 😅

# CI/CD

The flow of deploying and previews can stay the same, Vercel is pretty smart when it comes to figuring out how to build a project. However, since we switched to a more code-heavy approach we now also have access to tools like Prettier and ESLint (built in with Gatsby). Let's add this to our CI process.

```yml
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12.x, 14.x]

    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm install
      - run: npm run build
```

Let's also make sure anything we commit is properly formatted. To do this automatically, we will use [Husky](https://www.npmjs.com/package/husky). Configuration is dead-simple because we already have a NPM script that runs Prettier.

I added the following snippet to package.json

```json
  "husky": {
    "hooks": {
      "pre-commit": "npm run format"
    }
  }
```

# Finishing

At this point, all that's left is deleting some unused assets and debug code lines. Looking back, I'm very happy I made the switch from Jekyll to Gatsby. Gatsby will allow me to really customize my blog if I want.
