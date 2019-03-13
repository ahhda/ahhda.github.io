---
layout: post
title: Using Cookiecutter-django with Google Cloud Storage
date: 2019-03-12
author: anuj
categories: [cloud, gce, django]
image: assets/images/django-gcp.png
featured: true
hidden: true
---

Have you ever wanted to automate all these boring things that you have to do while setting up new django project? Like writing proper settings, setting up whole folder structure, adding docs, readmes, setup.py etc? There is an app that can do these mundane actions for you: [Cookiecutter Django](https://github.com/pydanny/cookiecutter-django).

Cookiecutter Django is a framework for jumpstarting production-ready Django projects quickly. It has some great features as it is pre-loaded with the basic requirements needed in order to quickly get a project up and running.

![github_meme]({{ site.baseurl }}/assets/images/dev_environment.jpg)

But it uses Amazon S3 for media storage which might not be the case with everyone. Today we will see the steps to add support for Google Cloud Storage for storing user uploaded and other media files.

### 1. Go to [Google Cloud Console](https://console.cloud.google.com/apis/credentials) and create a service account

![github_meme]({{ site.baseurl }}/assets/images/cloud_credential.png)

Choose the Service type as JSON and download the newly created json file under the Django Project directory > config folder. Lets name it `bucket.json`.

### 2. Go to [Cloud Storage](https://console.cloud.google.com/storage/browser) and create a bucket

Copy the bucket name, we will use it later on.

### 3. Add Credentials to the env file

Edit the `.envs/.production/.django` file and add the following credentials (you can also add this to the local env file for testing):

```
# Google
# ------------------------------------------------------------------------------
GOOGLE_APPLICATION_CREDENTIALS=/app/config/bucket.json
```

### 4. Add the django-storages library

This is the library that we would be using to handle all our calls. Edit the `requirements/production.txt` file and replace `django-storages[boto3]==1.7.1` with `django-storages[google]==1.7.1`.

### 5. Edit the configuration file to use Google Cloud Storage

Add the following lines to the file `config/settings/production.py`:

```
DEFAULT_FILE_STORAGE = 'storages.backends.gcloud.GoogleCloudStorage'
GS_BUCKET_NAME = #YOUR BUCKET NAME
GS_DEFAULT_ACL = 'publicRead'
MEDIA_URL = 'https://storage.googleapis.com/{}/'.format(GS_BUCKET_NAME)
MEDIA_ROOT = 'https://storage.googleapis.com/{}/'.format(GS_BUCKET_NAME)
```

### 6. Build and Run!

Thats it. You are now ready to test the serving of the media files from Google Cloud Storage buckets.
