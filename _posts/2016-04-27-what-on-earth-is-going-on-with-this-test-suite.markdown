---
layout: post
title:  "What On Earth Is Going on With This Test Suite?"
date:   2016-04-27 11:43:00 -0400
categories: ruby stuff
---


### Going down the rabbit hole:

This started with an attempt to write a unit test for a module. Most of the existing unit
tests subclassed `ActiveSupport::TestCase`, but this didn't work for me. Comparing
the existing tests to documentation online only raised more questions, so I started digging:

---

### Backtracking:

### Rspec, Minitest, and Test::Unit -- what is the difference?

Test::Unit was the default testing framework before Minitest. It is currently available
as a standalone gem, test-unit

Minitest is the current default testing framework, and ships with versions of Ruby
1.9 and more recent.

Rspec is another popular testing framework, and perhaps the easiest to differentiate.

[A helpful video about testing under rspec, Minitest, and Test::Unit][test-video]

---

### Shoulda:

This is a set of gems put out by Thoughtbot-- it includes shoulda-matchers and
shoulda-context. Looking over their documentation, it is somewhat ambiguous which
parts of shoulda apply to rspec, Minitest, and Test::Unit.

---

### ActiveSupport::TestCase

Okay, so, tests which subclass Test::Unit::TestCase are using Test::Unit,
and tests which subclass MiniTest::Unit::TestCase or Minitest::Test are using Minitest.
What are tests which subclass ActiveSupport::TestCase?

...trick question! There is an adapter for ActiveSupport::TestCase built into the
test-unit gem, but it's also in Minitest

---

### What is the difference between Test::Unit and Minitest?

This post helps to cover some of the confusing points. tl;dr? It's a damn mess.

[https://mattbrictson.com/minitest-and-rails][minitest-vs-test-unit]

---

### So, how do you tell which testing framework(s?) is/are in use?

There is a awful lot of syntactic sugar available which makes Test::Unit look like
MiniTest, and Minitest look like Test::Unit. (Oh, and does it look like I made a typo
  in writing "MiniTest" in one place and "Minitest" in another? That is on purpose:
  older versions of Minitest were written as "MiniTest," while more recently, it
  shows up as "Minitest." Feel free to flip a table now.)

My best guess is to run `bundle show` to see which gems are in use with your
current app. Since Minitest and Test::Unit can be included without showing up
in the Gemfile, this explicitly states if these gems are or are not present.


[test-video]: http://rubyoffrails.com/videos/12-testing-3-ways-rspec-minitest-test-unit
[minitest-vs-test-unit]: https://mattbrictson.com/minitest-and-rails
