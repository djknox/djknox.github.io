---
layout: post
title: Starting A New Django Project
permalink: "/starting-a-new-django-project/"
date: 2019-09-28 18:27 -0400
---
[Django](https://www.djangoproject.com/) is a hugely popular framework for creating web applications. Written in Python, the framework is a common choice for new startups, but it is also used by many large-scale applications like Instagram and Eventbrite.  

This post will list some commands for setting up your own Django project using [Git](https://git-scm.com/) for version control, [pipenv](https://pipenv-fork.readthedocs.io/en/latest/) for managing dependencies and a virtual environment, and a terminal such as [Terminal](https://support.apple.com/guide/terminal/welcome/mac) or [iTerm2](https://www.iterm2.com/) for entering the commands.  

---

Here's what you can do to start a new Django application:  

First, make a new directory for the project and ```cd``` into that new directory:
```
mkdir my-new-django-project
cd my-new-django-project
```

It's always a good idea to keep track of your project with Git, so start a new repository in your project directory:
```
git init
```

Now that an empty Git repo has been initialized, use ```pipenv``` to install Django:
```
pipenv install django
```
This will create a virtual environment and a ```Pipfile``` to track your project's dependencies. Django will be added to the ```Pipfile``` in the ```[packages]``` section.

At this point, your project directory will contain these two files:
```
Pipfile		Pipfile.lock
```


Now that you have a virtual environment, enter it with the ```shell``` command to start working on your project:
```
pipenv shell
```

Within the virtual environment, which has Django installed, you can start a new project:
```
django-admin startproject <project_name> .
```
> In the above command, ```<project_name>``` is just a placeholder; this is where you will provide the name of your Django project.  

For example, create a new project called "my_new_django_project" with:
```
django-admin startproject my_new_django_project .
```

> Be sure to remember the trailing ```.``` at the end of the command -- this will place all the necessary files within the current directory. Without the ```.``` at the end, everything will be put into a different, new, directory and may not be what you want.

Your project directory will now have the following structure:
```
Pipfile		Pipfile.lock	manage.py	my_new_django_project
```

> ```manage.py``` is Django's command-line utility that sits at the root of your project. You'll use it to do all sorts of tasks like making/running migrations and starting your development server.  

> A project package is also created that will have the name you provided in the ```startproject``` command. This directory contains all the Django application files and is where you will develop your project.

Before staging and committing your new changes, it's not a bad idea to create a ```.gitignore``` file so you don't put any unneeded or sensitive files in your version control. You can generate one for new Django projects with [gitignore.io](https://gitignore.io/api/django).

Stage and commit your new project:
```
git add .
git commit -m 'install django and start new project'
```

Use the ```runserver``` command to start the development server and see your new app in the browser at ```http://127.0.0.1:8000/```:
```
python3 manage.py runserver
```

Now you have a fresh Django project to start building your web app with! 