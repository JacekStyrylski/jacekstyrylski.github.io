---
layout: post
title: Write post in Jekyll
date: 2017-02-23 22:54
---

In order to write a new post in Jekyll we need to do following:

1. Create new file in _posts folder having following name:

    `[YYYY-MM-DD-title].md`
2. At beginning of the file there has to be Yaml [Front Matter](https://jekyllrb.com/docs/frontmatter/):

    ```sass
    layout: post                                                    
    title: "New post"
    date: 2016-11-18 13:13:13 +0100
    categories: general
    ```

3. There post itself, which can be written in Markdown, should be placed directly after second `---`.

Please be aware that if you'll select date from day in the future, post will not be rendered until that time.

Further informations might be find here:

[https://jekyllrb.com/docs/posts/](https://jekyllrb.com/docs/posts/)