---
layout: post
title:  "Using pre-commit hooks in git"
date:   2016-05-03 11:43:00 -0400
categories: ruby stuff
---

Every now and then a merge conflict evades notice and the offending code gets
committed. It seems like that should be a solved problem, though, right? ...at
least, in the case of `<<<<<< HEAD` strings remaining in place-- that would just
be a simple string search.

Fortunately, git has hooks available, which can be modified to include a variety
of behaviors.

In your git repository, check the `.git/hooks` directory. There should be a set
of sample files. For my purposes, I'm looking at the `pre-commit.sample` file.

To start using this script, rename it to `pre-commit`.
<<<<<< HEAD
======

this is a test
