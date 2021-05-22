# Deploy Django Web Application on Heroku
We will start from a fresh Django project then by time we will add resources to this app while deployment in many phase.

## What is Heroku
Before Deploying/take a django site to live server we will learn what is `Heroku`? Heroku is a website hosting platform that supports several languages including Python (any python application including Django). This platform is different from traditional hosting service (like cloud service, dedicated service). So that, it is somethiing called PaaS (Platform as a Service). There are other PaaS available also like (`Netlify`). The main benefit of a PassS is: it makes much easier your deployment process. You need not maintain or configure any web server, configaration files, databases, etc. Just some simple commands will deploy your site successfully. Let's start this Django Tutorial-1.

## Starting a Project
1. Go to the directory where you want to start the project
2. Check if python and virtualenv is isntalled by typing (`$ python -V` and `$ virtualenv --version`) - Here in this app we are using Python version 3.9.2 and virtualenv version 20.4.3
3. Type `$ virtualenv venv` to create a python virtual environment in the current directory
4. Type `$ source venv/bin/activate` (on Linux) and `$ venv/Scripts/activate` (on Windows) to activate the virtual environment
5. Type `$ pip install django==3.2.3` to install django 3.2.3 (The current latest version) - always use this on future for this project
6. Type `$ django-admin startproject deploy` to create a django project called deploy.
7. `$ cd deploy` to move to django project folder
8. `$ python manage.py migrate` to migrate the database file
9. `$ python manage.py createsuperuser` to create admin user. (user: admin, pass: 1234)
10. Now type `$ python manage.py runserver` to start our app locally
11. To test open the web browser and type in address bar: http://127.0.0.1:8000/

## Deploying the project on Heroku
1. First make sure `Git` and `Heroku Toolbelt` - Basically Heroku CLI is installed o your computer. (`$ heroku --version` and `$ git --version`)
2. Make sure you are in the django project's root folder (where manage.py resides)
3. Type `$ heroku login` to login into heroku. To login first login to your heroku account from web browser. And `$ heroku login` will open the browser to authenticate
4. After login, `$ git init` to create a git version
5. `$ git add .` and the git `$ git commit -m 'first commit'`
6. Now we will create a Heroku app from commandline `$ heroku create deploy10mp` (Here deploy10mp is app name)
7. Now add our local git project to heroku remote by `$ heroku git:remote -a deploy10mp`
8. Now install the Gunicorn application server by typing `$ pip install gunicorn`
9. Now we can start the gunicorn server on our local machine by `$ gunicorn deploy.wsgi`
10. Create a new file in the projec directory called `Procfile`. It is actually a Heroku file that works as a web workers
11. Add the line in Procfile `web: gunicorn deploy.wsgi` (the same as we started server using gunicorn at our local machine).here `web:` specifys the web worker
12. Run `$ pip freeze > requirements.txt` to the requirements.txt file
13. We added and changed some files so we have to track and commit all the files again
14. Run `$ git add .` and then `$ git commit -m "added Procfile and installed gunicorn"`
15. Now push the project on Heroku app we just created: `$ git push heroku master`
16. In this phase, you will get an error (Specifically, staticfile error).
17. To fix this, go to `deploy/settings.py` and come at the end of the file, under `STATIC_URL = '/static/'` add this line `STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')`. Make sure `os` module is imported.
18. Now save the file and again `$ git add .` then `$ git commit -m "added staticfiles's root"`
19. Finally `$ git push heroku master` to push the project to `deploy10mp` app.
20. Now the app is successfully deployed to Heroku. If you browse the app link, `https://deploy10mp.herokuapp.com` either you will get `Application Error` from Heroku or `Disallowed host error` from Django.
21. To fix this, again open `deploy/settings.py` and find the line `ALOWED_HOST = []` and in this line add the localhost address `127.0.0.1` and the app link `deploy10mp.herokuapp.com`. Save the file and exit.
22. Again `$ git add .` then `$ git commit -m 'added to allowed host '`.
23. Finally `$ git push heroku master` to push the updated project.
24. Now the app should work.


For more course and tutorial browse: https://www.10minuteprogram.org