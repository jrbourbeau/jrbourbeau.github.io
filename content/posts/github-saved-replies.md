---
title: Better living through saved replies on GitHub
date: 2023-01-20
summary: "Using saved replies on GitHub can improve your developer life"
---

## Problem

I often find myself writing the same comment or snippet several times when working on
GitHub. This can feel pretty tedious.

### Example: New contributors

When a first-time contributor has a PR merged to a project I work on,
I'll usually leave a comment like:

> Also, I noticed this is your first code contribution to this repository. Welcome!

### Example: Details

Another thing I find myself typing often are `<details>` blocks (which get rendered
on GitHub as collapsable sections of text):

~~~
<details>
<summary>Details:</summary>

```
```

</details>
~~~

This is nice, for example, when including a long traceback in a comment. The downside is
there's quite a bit of boilerplate you need to remember and type.

## Solution

GitHub has a feature for [creating](https://docs.github.com/en/get-started/writing-on-github/working-with-saved-replies/creating-a-saved-reply)
and [using](https://docs.github.com/en/get-started/writing-on-github/working-with-saved-replies/using-saved-replies)
saved replies. Using saved replies for these types of comments has definitely resulted
in a quality of life improvement for me as a developer. You should try them out if you find
yourself in a similar situation. 
