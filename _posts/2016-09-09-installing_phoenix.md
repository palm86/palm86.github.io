---
layout: post
title: Installing Phoenix the tl;dr way
---

In the rest of the posts, I will assume you have Erlang, Elixir, Hex, Phoenix, Node.js and PostgreSQL up and running. So let's get that out of the way. See the [Phoenix installation docs](http://www.phoenixframework.org/docs/installation) if you are not running Ubuntu, otherwise, these commands will get you sorted:

### tl;dr

{% highlight bash %}
# Erlang/Elixir
$ wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
$ sudo dpkg -i erlang-solutions_1.0_all.deb
$ sudo apt-get update
$ sudo apt-get install esl-erlang elixir

# Hex
$ mix local.hex

# Phoenix
$ mix archive.install https://github.com/phoenixframework/archives/raw/master/phoenix_new.ez

# Node.js
$ sudo apt-get install nodejs nodejs-legacy npm

# PostgreSQL
$ sudo apt-get install postgresql postgresql-contrib
{% endhighlight %}


### Erlang and Elixir

The Erlang and Elixir versions packaged with Ubuntu are slightly outdated, so we grab them from a PPA.

{% highlight bash %}
$ wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
$ sudo dpkg -i erlang-solutions_1.0_all.deb
$ sudo apt-get update
$ sudo apt-get install esl-erlang elixir
{% endhighlight %}

You can test that the installation was successful by running:

{% highlight bash %}
$ elixir -v
{% endhighlight %}

Or try the interactive shell (terminate with Ctrl-C):

{% highlight bash %}
$ iex
{% endhighlight %}


### Hex

[Hex](https://hex.pm) is the Erlang ecosystem package manager. It is to Erlang/Elixir what [pip](https://pip.pypa.io/) is to Python. Or close enough. Install it with:

{% highlight bash %}
$ mix local.hex
{% endhighlight %}

[Mix](http://elixir-lang.org/docs/stable/mix/Mix.html) is a build tool for Elixir projects that comes installed with Elixir.

### Phoenix

We'll install Phoenix itself from a Mix archive (an application together with its BEAM files). It is not currently clear to me what the Python counterpart of a Mix archive is. Install it with:

{% highlight bash %}
$ mix archive.install https://github.com/phoenixframework/archives/raw/master/phoenix_new.ez
{% endhighlight %}

See the [Phoenix installation docs](http://www.phoenixframework.org/docs/installation) for alternatives.

### Node.js

Phoenix uses [Brunch](brunch.io) for asset management. Brunch requires [npm](https://www.npmjs.com/) and [Node.js](https://nodejs.org/en/). On Ubuntu, these can be installed with:

{% highlight bash %}
$ sudo apt-get install nodejs nodejs-legacy npm
{% endhighlight %}

If your version of Ubuntu does not provide version 5 or later of Node.js, you may want to follow the instructions on the links above.

### PostgreSQL

We will use PostgreSQL as our database. Install it with:

{% highlight bash %}
$ sudo apt-get install postgresql postgresql-contrib
{% endhighlight %}

pgAdmin III is a handy GUI client for PostgreSQL. You can install it with:

{% highlight bash %}
$ sudo apt-get install pgadmin3
{% endhighlight %}

Any additional dependencies we might need will be installed through the Mix tool. We are now ready for our first Phoenix project.
