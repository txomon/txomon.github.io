---
title: "From Jekyll to Hugo"
date: 2020-03-21T17:38:31+01:00
tags:
- Documentation
- Hugo
- Blog
- Jekyll
---

I'm taking this time to clean up the stuff I have, and I have decided to put in a bit better shape my blog.

I'm quite a bit tired of jekyll, the part of using ruby never convinced me, and documentation sucked, so I decided to move to Hugo.

Just in case you are wondering why documentation sucked, it's because content is all mixed, pro tip:

{{< youtube t4vKPhjcMZg >}}

Anyway, the process has been kind-a terrible, because hugo's docs are,... well, they hold more information but they are also completely mixed, getting started has a full reference of the documentation, it doesn't hold you by the hand on how to build a site... There are tons of things that I have been looking at examples just to be able to pinpoint them.

Few things that I need to remember for the future:

* New posts can be created with for example: `hugo new post/2020/03/21/from-jekyll-to-hugo.md`

* When creating posts with images, because output will be a folder, they need to already be a folder by themselves: `hugo new post/2020/03/21/from-jekyll-to-hugo/index.md` for this one for example

* Not all themes support everything. Hugo seems like it has been extended in several ways through themes that do crazy stuff

Now I am trying to create a pre-commit hook to update the website whenever needed
