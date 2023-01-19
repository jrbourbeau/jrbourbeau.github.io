---
title: Triaging Bug Reports
date: 2023-01-19
---

Triaging bug reports is an important part of maintaining a library. This post lists some of my
thoughts on how to triage effectively.

## Be welcoming

Bug reports are a valuable source of information about pain points users are facing and highlight areas
where your project could be improved. We build software to be used and we should listen to those who
actually use it.

Writing a good bug report can take a significant amount of time. They're also voluntary -- users
who file bug reports often are going above and beyond whatever their normal job is.

I've found it both feels good socially and helps increase the level of engagement if you're welcoming
on an issue tracker. This doesn't take much effort on the maintainer's part, often a single sentence
that starts with "Thanks" will do.

For example, most of [my initial comments on bug reports](https://github.com/dask/dask/issues/9830#issuecomment-1382314008)
start off along the lines of:

> Thanks for the detailed issue and reproducer @github-username

This helps, in some way, acknowledge that the user took the effort to write a bug report and also
sets a friendly tone for the rest of the engagement on the issue.

## Reproduce the issue

This tends to be where most of the work in bug report triaging is. If the user doesn't provide
something, usually a code snippet, that reproduces the issue my usual first step is to ask for such
a reproducer. Having a reproducer that you can run on your local machine to drastically increases the
chance that the bug will actually be resolved.

There are several aspects that make up a good minimal reproducer.
[This blog post on craft minimal bug reports](https://blog.dask.org/2018/02/28/minimal-bug-reports) has
a nice description of what makes a good reproducer -- I will often point users to that blog post.

Put some effort into this step. Sometimes users, especially new users, need support to extract a
minimal example from their real-life use case.

## Confirm that it's actually a bug

Sometimes it's not obvious where the underlying issue is when a user encounters a bug. Confirm that the
issue is actually with your project and not, for example, some other library that your project uses or
integrated with in some way. 

Additionally, make sure that the issue isn't just user error. If it is a user error, let the user know
and give them a suggestion about what they should be doing instead. Also consider if documentation could
be updated to avoid this type of user error in the future.

## Set up future work

Once you've confirmed the issue is actually a bug and are able to reproduce it in some way,
it's good to provide a sense for what reasonable next steps are. In some cases you'll know
exactly what the problem is and can point to the line of code that needs to change.
In other cases, it's a more educated guess. For example:

> I don't know exactly what the problem is, but my guess is it has to do with module X.
> We made some changes there recently in PR#1234 that could be related.

Even just pointing to the relevant part of the codebase is valuable for whoever ends up working on the issue,
whether it's a random community member, another maintainer, or you 6 months from now.

## Don't solve everything

It's easy to get sucked into a ultra responsive mode of working where you try to fix every single bug that gets filed.
Try your best to avoid this. Every project has limited maintainer bandwidth and choosing to work on a bunch
of small edge cases takes time away from other efforts that could have a broader impact and push your project
forward in a more serious way.

I'm not saying don't fix bugs -- I'm saying avoid the pressure to fix every little thing. Your time is
valuable, put it towards things that have a broad impact.

## Extend an invitation to join

I've found it useful to ask the user who opens an issue if they're interested in fixing it.
Something simple like:

> Is this something you're interested in working on? (no obligation though)

is totally sufficient. Though I do try to convey that they shouldn't feel a responsibility to fix the issue
(i.e. the "no obligation though" part). It's nice to give folks an easy way out, though you might be
surprised how often [people](https://github.com/dask/dask/issues/8297)
[say](https://github.com/dask/dask/issues/8082)
["yes"](https://github.com/dask/dask/issues/8543).

This helps get more people familiar with the internals of your project, sometimes results in
repeat contributions, and also reduces the number of bug reports you end up fixing yourself.
This can also be important culturally for community-driven open source projects.