---
layout: post
title: Deploying a Django App to Heroku
permalink: "/deploying-a-django-app-to-heroku/"
---
[Heroku](https://www.heroku.com/platform) is a cloud-based platform as a service (PaaS) that lets developers easily deploy their apps to the web. The service includes a free tier, so it can be great for sharing hobby projects or messing around with cloud deployment. If you've already [started your own Django app](/starting-a-new-django-project/) and are ready to make it live, then this article will show you how to get it on Heroku.

---

## Files/Packages to Add to Your Django Project

### Django-Heroku Package
The [Django-Heroku Package](https://github.com/heroku/django-heroku) will take care of settings configuration for deploying a Django app to Heroku. Add this package to your project with ```pipenv```:  
```
pipenv install django-heroku
```

At the bottom of your ```settings.py``` file, add this code so your Heroku deployment will have environment variables like ```DATABASE_URL```, ```ALLOWED_HOSTS```, etc. automatically configured:  
```
# Configure Django App for Heroku.
import django_heroku
django_heroku.settings(locals())
```

### Green Unicorn
[Gunicorn](https://gunicorn.org/) (or 'Green Unicorn') is a WSGI (Web Server Gateway Interface) Server that ensures that your Django app can communicate with web servers.  

Add Gunicorn to your project with ```pipenv```:  
```
pipenv install gunicorn
```

### Procfile
Heroku apps use a [Procfile](https://devcenter.heroku.com/articles/procfile) for declaring processes that will run when the app is started up.  

In your project's root, create a new file named ```Procfile``` (notice there is no file extension). Within that file, declare a ```web``` process that invokes Gunicorn:
```
web: gunicorn <project_directory>.wsgi
```
> In the above ```web``` process command, ```<project_directory>``` is just a placeholder; this is where you will provide the name of your project directory (the directory containing ```settings.py```).  

For example, if your project has this structure:  
```
Pipfile		Pipfile.lock    Procfile	manage.py	my_django_project
```
then your ```Procfile``` will contain:
```
web: gunicorn my_django_project.wsgi
```

Be sure to commit these changes to your project's Git repository.

## Deploying to Heroku
Now that you have your project configured for Heroku, you can actually deploy it on the platform.

Before anything, you'll need to create a Heroku account and then [create a new app](https://dashboard.heroku.com/new-app) for your project from the dashboard.  

You'll also need to [install the Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) to interact with your Heroku app on the command line. Using Homebrew:
```
brew tap heroku/brew && brew install heroku
```

Heroku gives you a few deployment methods, and this article will cover two of them: Heroku Git and GitHub. You can find all the deployment methods, with information about each of them, under the 'Deploy' tab of the Heroku Dashboard.

### Deployment with Heroku Git
This method involves adding Heroku as a remote repository and pushing the application to it, which will then deploy to the platform.

You'll need to log in:
```
heroku login
```

Add the remote repository:
```
heroku git:remote -a <the-name-of-your-heroku-app>
```

Push to the remote repository to deploy your app to Heroku:
```
git push heroku master
```

### Deployment with GitHub
If you'd prefer not to have to manage a separate remote repository for Heroku deployments, you can connect your project's GitHub repository and deploy from there.  

Under the 'Deploy' tab of the Heroku Dashboard, select 'GitHub' in the 'Deployment method' section and then find your project's repository in the 'Connect to GitHub' section. You can either deploy your app with the 'Deploy Branch' button under the 'Manual deploy' section, or enable automatic deployments in the 'Automatic deploys' section.

With automatic deployments enabled, any time you commit to the ```master``` branch, your app will also deploy to Heroku.  

### Additional Heroku Configuration
Regardless of the deployment method you choose, you will have to set up some things on your Heroku app.

Be sure to log in:
```
heroku login
```

Run migrations on the Heroku app:
```
heroku run python3 manage.py migrate
```

Create a new ```superuser``` for logging in to the Django admin dashboard:
```
heroku run python3 manage.py createsuperuser
```

You can set environment variables with the ```config:set``` command. For example:
```
heroku config:set DEBUG=False
```

After going through these deployment steps successfully, you should be able to see your Django app on the web by adding ```.herokuapp.com``` to the end of the name you gave to your Heroku app.