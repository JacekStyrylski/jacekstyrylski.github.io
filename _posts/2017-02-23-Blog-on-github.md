---
layout: post
title: Blog on GitHub
date: 2017-02-23 21:41
---
Recently I've decided to set up a new blog, so I needed to decide where to host it so it will fits me best.

I've been using [wordpress.com](http://wordpress.com) for some time, however I wasn't happy with it
mainly due to imperfect editor, and missing code formating for Typescript.

I've learned about the possibility to host entire blog on [GitHub Pages](https://guides.github.com/features/pages/)
what seems to be pretty interesting taking into how simple it is to write new posts using famous GitHub Flavored Markdown.

## Typescript example

```typescript
@suite class DefaultDeleteTest {
    private static server: BetterVotingServer;

    public static before(){
        DefaultDeleteTest.server = srv;
        chai.use(chaiHttp);
    }
}
```

## GitHub Pages tutorial

There is pretty good tutorial together with nice boilerplate for Blog hosted on GitHub Pages:

- [Tutorial](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)
- [Boilerplate](https://github.com/barryclark/jekyll-now)