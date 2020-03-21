---
title: "Generic Pre Commit"
date: 2020-03-21T19:34:35+01:00
tags:
- pre-commit
- hugo
- automate
---

I have created https://github.com/txomon/pre-commit-hooks as a generic way to have random code be specified directly in my git repos without having to create custom hooks. The inner working are basics, and you can actually check them there.


`pre-commit` relies on you adding a hooks repository and the hook id within it, which will be then linked to whatever scripts you have set in that hooks repository.

My solution is to create a generic hook that uses `system` kind of hook, and just run `bash -c` with it. The arguments you put are then passed to `bash -c`.

Such as in this repo:

```
repos:
  - repo: https://github.com/txomon/pre-commit-hooks
    rev: b7aeed3b9a00db8bcec16adcf07d237251b45947
    hooks:
      - id: text-command-always
        name: Generate site with hugo
        args:
          - hugo && git add categories/ css/ index.* sitemap.xml tags/ post/
```

Where I make it so that hugo will build the site every time.
