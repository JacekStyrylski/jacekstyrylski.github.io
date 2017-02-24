---
layout: post
title: Write post in Jekyll
date: 2017-02-23 22:54
---

In order to write a new post in Jekyll we need to do following:

1. Create new file in _posts folder having following name:

    `[YYYY-MM-DD-title].md`
2. In beginning of the file there has to be Yaml [Front Matter](https://jekyllrb.com/docs/frontmatter/):

```yaml
layout: post
title: "New post"
date: 2016-11-18 13:13:13 +0100
categories: general
```

``` html
<div class="highlight highlight-source-yaml">
    <pre>
        <span class="pl-ent">layout</span>:
        <span class="pl-s">post</span>
        <span class="pl-ent">title</span>:
        <span class="pl-s">
            <span class="pl-pds">
                "
            </span>
            New post
            <span class="pl-pds">
                "
            </span>
        </span>
        <span class="pl-ent">
            date
        </span>
        :
        <span class="pl-s">
            2016-11-18 13:13:13 +0100
        </span>
        <span class="pl-ent">
            categories
        </span>
        :
        <span class="pl-s">
            general
        </span>
    </pre>
</div>
```

``` html
<div class="language-yaml highlighter-rouge">
    <pre class="highlight">
        <code>
            <span class="s">layout</span>
            <span class="pi">:</span>
            <span class="s">post</span>
            <span class="s">title</span>
            <span class="pi">:</span>
            <span class="s2">"</span>
            <span class="s">New</span>
            <span class="nv"> </span>
            <span class="s">post"</span>
            <span class="s">date</span>
            <span class="pi">:</span>
            <span class="s">2016-11-18 13:13:13 +0100</span>
            <span class="s">categories</span>
            <span class="pi">:</span>
            <span class="s">general</span>
        </code>
    </pre>
</div>
```

3. There post itself, which can be written in Markdown, should be placed directly after second `---`.

Please be aware that if you'll select date from future, post will not be rendered until that time.

Further informations might be find here:

[https://jekyllrb.com/docs/posts/](https://jekyllrb.com/docs/posts/)