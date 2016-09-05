---
layout: post
title: Phoenix for Djangonauts
---

I've recently joined a project in which we are building a RESTful
API that facilitates data acquisition, transformation, and presentation. Without
going into details, the problem lends itself well to functional thinking in the sense that state
is passed form function to function, transforming the data along the way.
Therefore, although any language/framework pair would in principle do, the team chose
Elixir/Phoenix with the belief that the language in which you solve the problem should be as close as possible to the domain of the problem.

Of course, one of the biggest selling points of functional languages is their
performance and concurrency, while one of the biggest drawbacks is arguably their
syntax and poor productivity. In many respects these pro's and con's are complementary
to what languages such as Python has to offer. Python/Django has turned out to
be a highly productive pair and while Python is not generally known for its
speed, with the right tooling, Django scales pretty well (see
[High Performance Django](https://www.djangoproject.com/)).

[Elixir](http://elixir-lang.org/), released in 2012 by José Valim, is a language that has taken my opinion of functional on an unorthodox dance to
*Minor Swing*. While it is certainly something to get
used to, both in terms of paradigm and syntax, it is fun to work with and pleasing to the eye. It runs on the rock solid Erlang virtual machine, but has the feel of something fresh and rejuvenated.

In Elixir, request handling is part of the cocktail. The concept of [Plugs](https://hexdocs.pm/plug/Plug.Conn.html) blurs the request-middleware-response roundtrip into a series of transformations on a `struct` representing a client connection. While the Plug framework is [all you need](https://codewords.recurse.com/issues/five/building-a-web-framework-from-scratch-in-elixir) to build a web application or API, [Phoenix](http://www.phoenixframework.org/) is a fantastic framework built around Plugs that makes developing every bit as productive as in Django.

Well, in theory that is. In the following posts I will write about just how productive Phoenix really is in the eyes of a Djangonaut. I will write about what delights me, what surprizes me, and what annoys me. Usually I will simply write to organize and archive what I've learned. I will assume you are familiar with Django and have at least read through the Elixir [learning resources](http://elixir-lang.org/learning.html) and glanced through the Phoenix [guides](http://www.phoenixframework.org/docs/overview).

First off, compared to Django, Phoenix can do with more and better documentation. This blog aims to contribute towards that goal in a small way.
