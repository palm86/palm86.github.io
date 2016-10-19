---
layout: post
title: Your first Phoenix app <span>#1</span>
published: false
---

Most Djangonauts probably started learning Django by building, at least in part,
the polling app that is developed in the [Your first Django app](https://docs.djangoproject.com/en/1.10/intro/tutorial01/)
tutorial series. It may therefore be a useful Phoenix learning
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

`mix phoenix.new mysite` is the equivalent of `django-admin startproject mysite` and generates a file structure like this:

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

Now we can simply change to the "mysite" directory with `cd mysite` and run `mix phoenix.server`, which is the counterpart of `python manage.py runserver` in Django.

Answer yes to any prompts asking you to install a local copy of "rebar".

If you now open [http://localhost:4000](http://localhost:4000) in your browser, you should be greeted by the standard Phoenix home page.

We are now at the commit tagged `part1-default-application` in the GitHub repo. Note that you might have to run `npm install`, `brunch build`, and `mix deps.get` if you only grabbed the code and haven't followed the steps in the tutorial.

## Creating the Poll app

Our use of the term "app" so far has probably led to some confusion for Django developers. Phoenix does not distinguish between projects and apps in the same way that Django does. It is typical for a Django project to consists of tons of apps (which should really have been called plugins), some inside the same project structure, some as external dependencies. The difference should become clearer as we progress with the rest of the tutorial. What we've called an "app" up to now is a web application, not a plugin.

So in Django your next step would have been `python manage.py startapp polls`, which would have created default modules for you models, views, tests, etc. There isn't really an equivalent for this command in Phoenix, because we already have the necessary modules in the web folder:

```
web
├── channels
├── controllers
├── gettext.ex
├── models
├── router.ex
├── static
├── templates
├── views
└── web.ex
```

The folders are mostly self-explanatory and more detail is to follow. But a brief note on the Model-View-Controller (MVC) pattern is warranted here. Django and Phoenix differ in their implementation (or naming) of the MVC pattern components. The mapping is as follows:

<table>
<thead>
<tr>
<th>MVC</th>
<th>Django</th>
<th>Phoenix</th>
</tr>
</thead>
<tbody>
<tr><td>model</td><td>model</td><td>model</td></tr>
<tr><td>controller</td><td>view</td><td>controller</td></tr>
<tr><td>view</td><td>template</td><td>view + template</td></tr>
</tbody>
</table>

## Write your first controller

Open `mysite/web/controllers/page_controller.ex` and consider its contents:

```
defmodule Mysite.PageController do
  use Mysite.Web, :controller

  def index(conn, _params) do
    render conn, "index.html"
  end
end
```

It defines the `index` function (or action) that will be called when you visit the root URL. The first argument `conn` contains information about the connection, whereas the second contains any parameters contained in the request body. When called, it renders the `index.html` template. We are not going to change it for now.

In Django the equivalent view (yes it's a view in Django) could look like this:

```
from django.shortcuts import render

def index(request):
    return render(request, 'index.html')
```

Or without the shortcuts:

```
from django.http import HttpResponse
from django.template import loader

def index(request):
    t = loader.get_template('index.html')
    c = {}
    return HttpResponse(t.render(c, request))
```
In other words, in Django you can render text directly from within a view:

```
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

Similar shortcuts can be used in Phoenix, but let us go the more conventional route and let the view, not the controller, produce the response. Open `mysite/web/views/page_view.ex` and change its contents to:

```
defmodule Mysite.PageView do
  use Mysite.Web, :view

  def render("index.html", _assigns) do
      "Hello, world. You're at the polls index."
  end
end
```

Now serve the app again and visit the root URL. You should see the text "Hello, world. You're at the polls index." You will also notice that the Phoenix logo is still there. This is because in Phoenix all views are actually rendered within a master layout view. You can find the template for the layout view at `mysite/web/templates/layout/app.html.eex`.

If you browse around, you will also find the index template for `PageView` at `mysite/web/templates/page/index.html.eex`. It contains what used to be displayed when we first visited the root URL (apart from the Phoenix logo). So how come this isn't displayed anymore? We overrode it when we defined the `def render("index.html", _assigns)` function in `PageView`. How is it possible to override templates by defining functions? Well, templates are compiled into functions on their corresponding view modules. This means that `PageView` had a `def render("index.html", _assigns)` function even before we added one, but that the one we added has precedence (matches before the existing function). See [Phoenix Templates Are Just Functions](http://www.jeramysingleton.com/phoenix-templates-are-just-functions/) for more details.

So there is our Hello World example, but how did Phoenix know which controller action to call when we visited the root URL? The answer can be found in the following code snippet in `mysite/web/router.ex`:

```
scope "/", Mysite do
  pipe_through :browser # Use the default browser stack

  get "/", PageController, :index
end
```

It tells Phoenix that `GET` requests to "/" (the root) are to be handled by the `index` action of `PageController`. At this point in time you may or may not still be getting used to the Elixir syntax. The above can also be written as follows, which somehow felt less daunting to me initially:

```
scope("/", Mysite) do
  pipe_through(:browser) # Use the default browser stack

  get("/", PageController, :index)
end
```

`scope`, `pipe_through`, and `get` are Elixir macros defined by Phoenix that will expand during compilation into function definitions. Don't worry too much about it now.

Note that URL routing in Django is done solely by matching on a URL regex and not by the HTTP method as well as in Phoenix. In Phoenix on the other hand, URLs are not routed to controller actions by custom regex patterns as in Django. The Phoenix way is perhaps less scary to newcomers who don't know regex, but leaves it up to you to validate any parameters such as id's that you get from a URL.

If you which to grab to code so far, check it out with `git checkout part1-hello-world`.

In the next post, we will put the database to work and set up the admin interface.
