---
layout: post
title:  "local_assigns in Rails partials"
date:   2016-07-08 11:43:00 -0400
categories: ruby stuff
---

If you're working with a partial and wondering which local variables are defined,
it can be somewhat tricky to find every `render` in the code base that points
to that partial.

Calling `local_assigns` in a partial provides a hash with each available local
variable as a key, along with its current value. No blind poking around with
global search required!

If you would also like to know where this partial was called, try calling
`controller` or `__method__`.

`controller` will give you the controller associated with the current partial.

`__method__` with indicate where this partial was called (probably a view)
Note that this does not provide an exhaustive list of every place in the code
base that a given partial is called.
