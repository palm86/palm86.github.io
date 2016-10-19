---
layout: post
title: Your first Phoenix app <span>#1</span>
published: false
---

Most Djangonauts probably started learning Django by building, at least in part,
the polling app that is developed in the [Your first Django app](https://docs.djangoproject.com/en/1.10/intro/tutorial01/)
tutorial series. I therefore thought that it might be a useful Phoenix learning
experience to port the Django polling app to Phoenix in a series of posts. We won't go for the end
result immediately, but will rather follow some of the same detours as is
needed to illustrate some principles or features.

Let's learn by example.

The application will consist of two parts:

* A public site that lets users view polls and vote
* An admin site that lets you create, update, and delete polls

We assume you have [Phoenix and friends installed](https://palm86.github.io/2016/09/09/installing_phoenix/). The tutorial is written with the Django 1.10 tutorial in mind for Phoenix 1.2.1.

The code for this tutorial is hosted on [GitHub](https://github.com/palm86/Your-first-Phoenix-app-tutorial) with tags for the major milestones.

## Creating a project

To create a new Phoenix project, run the following command in your projects folder and answer yes to any prompts:

{% highlight bash %}
$ mix phoenix.new mysite
{% endhighlight %}

The command generates a file structure like this:

```
mysite
├── brunch-config.js
├── _build
├── config
├── deps
├── lib
├── mix.exs
├── mix.lock
├── package.json
├── priv
├── README.md
├── test
└── web
```
We will deal with the function of each of these files and folders in due time. In short:

*   **mysite**: the root that contains your project. It can be named whatever you want.
*   **config**: analogous to the settings.py file or settings module of any Django project.
*   **deps**: all dependencies are downloaded and stored here. It plays the role that a Python virtual environment would play for a Django project.
*   **lib**: a place for utility modules that are not directly involved in serving responses.
*   **mix.exs**: similar to manage.py in Django, but more general.
*   **mix.lock**: plays the role that a requirements.txt file would play in Python.
*   **package.json**: used by npm to install assets and node packages for asset management.
*   **priv**: your static assets and database seeds/fixtures and migrations go here.
*   **test**: your tests go here.
*   **web**: your app logic goes here.

Note that you will also have gotten a nicely prepopulated `.gitignore` file tailored to Phoenix's needs.

The project's Elixir dependencies as well as its static assets will have been installed as well. These tasks can be performed manually in the future by running `mix deps.get` and `brunch build`.

## Serving the project

Before we can serve the app, we have to run `mix ecto.create` to create a database for the project according to the settings in the config. It is not technically necessary for a Phoenix app to have a database. You can add the `--no-ecto` option when calling `mix phoenix.new` to skip database creation.

Now we can simply change to the "mysite" directory with `cd mysite` and run `mix phoenix.server`, which is the equivalent of `python manage.py runserver` in Django.

Answer yes to any prompts asking you to install a local copy of "rebar".

If you now open [localhost:4000](localhost:4000) in your browser, you should be greeted by the standard Phoenix home page.

We are now at the commit tagged `part1-default-application` in the GitHub repo. Note that you might have to run `npm install`, `brunch build`, and `mix deps.get`. If you haven't followed the steps in the tutorial so far.

## Creating the Poll app

Our use of the term "app" so far has probably led to some confusion for Django developers. Phoenix does not distinguish between projects and apps in the same way that Django does. It is typical for a Django project to consists of tons of apps (which should really have been called plugins), some inside the same project structure, some as external dependencies. The difference should become clearer as we progress with the rest of the tutorial.
