---
title: Trigger & Skipping CI services
date: 2019-11-26
---

A couple of notes about interacting with continuous integration (CI) services that I find useful when working on open source projects. 

## Using an empty commit to trigger CI

Sometimes it's useful to open a pull request to simply trigger any CI services for a project. Until recently I had done this by making some small change (e.g. adding a blank line to a `README`). Then I found out `git commit` has an [`--allow-empty` option](https://git-scm.com/docs/git-commit) which lets you make an empty commit:

```console
git commit --allow-empty -m "Test CI"
```

## Saving CI resources when possible

When making a small change like fixing documentation typos, it's not always necessary to run a full CI system, which can be limited for projects using free CI services. Luckily, most CI services like [Travis CI](https://docs.travis-ci.com/user/customizing-the-build/#skipping-a-build), [CircleCI](https://circleci.com/docs/2.0/skip-build/#skipping-a-build), and [Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops&tabs=yaml#skipping-ci-for-individual-commits) allow you to skip CI build for a particular commit if you include `"[skip ci]"` in the commit message.

```console
git commit -m "Don't test CI [skip ci]"
```

And if, like me, you often forget to include that `"[skip ci]"`, you can use

```console
git commit --amend
```

to modify the commit message for the most recent commit.

It's worth noting that some projects may prefer to have CI run for all commits, no matter how small the changes. So make sure to take a look at contributing guides.
