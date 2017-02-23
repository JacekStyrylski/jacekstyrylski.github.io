---
layout: post
title: Run Jekyll website locally
date: 2017-02-23 22:40
---

In order to check your website locally you need to set up local Jekyll environment using following steps:

1. [Install Ruby](https://www.ruby-lang.org/en/documentation/installation/), be careful when using package managers since
they not always have latest version what might lead to problems with certificates eventually stopping the installation.
2. `gem install github-pages` be sure that you've restarted console since environment variables needs to be reloaded.
3. From given repository call `jekyll serve --watch`

App Should be available under following link:

[http://127.0.0.1:4000](http://127.0.0.1:4000)
