# Django Heroku Bootstrap (DHB)

Get your Django app running on Heroku in less than 5 minutes. Really.

## What's in the box

Check out requirements.txt for details on the dependencies. DHB assumes you are using S3 for your static files when running on Heroku.
When running locally, all statics are served from static/ folder in the root of the project

All the settings are spread accross 3 files in the settings/ directory. 
* common.py has all your standard Django jazz that is identical for dev and production environments
* dev.py contains development specific settings 
* prod.py contains production specific settings and overrides

## Bootstrapping Your awesome app

1. Being a smart developer you are using a virtual environment for your project. Activate it.
2. Run pip install -r requirements.txt. Grab a cup of tea/coffee. Come back to find all packages successfully installed.
3. Head to settings/prod.py and update your S3 credentials

### Static files

```python
#Your Amazon Web Services access key, as a string.
AWS_ACCESS_KEY_ID = ""

#Your Amazon Web Services secret access key, as a string.
AWS_SECRET_ACCESS_KEY = ""

#Your Amazon Web Services storage bucket name, as a string.
AWS_STORAGE_BUCKET_NAME = ""
```   

### Database please

Of course. You first need to get Postrgres on your Heroku 

```
heroku addons:add heroku-postgresql:dev
```

Now let's figure out the url for your database. To do this let's run the following command that comes wiht DHB

```
fab what_is_my_database_url
```

The output will look something like this

```
HEROKU_POSTGRESQL_<COLOR>_URL: postgres://<user>:<password>@<host>:5432/blabla
```

Use the first portion of the string (before ':') and  head to settings/prod.py to update your database url. Mine looks like this, for example

```python
DATABASES = {'default': dj_database_url.config(default=os.environ["HEROKU_POSTGRESQL_ROSE_URL"])}
```

4. Make sure you are logged in to Heroku
```
heroku login
```
5. Get a new git repository initialized
```
git init
```   
6. Create a new heroku project
```
heroku create
```
7. Run your site locally
```
fab run
```
8. Move all your static goodness to S3
```
fab collectstatic
``` 
9. Deploy to Heroku
```
git add .
```
```
git commit -a -m "first commit"
```
```
git push heroku master
```
10. Sync your data with Heroku
```
fab remote_syncdb
```


