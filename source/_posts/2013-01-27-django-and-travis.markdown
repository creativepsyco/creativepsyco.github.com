---
layout: post
title: "Django and Travis"
date: 2013-01-27 03:42
comments: true
categories: [Django]
---

A sample `.travis` file to allow you to run and test on the [Travis CI][travis].

```yaml

language: python
python:
  # - "2.6"
  - "2.7"
services: mysql
env:
  # - DJANGO=1.2.7
  # - DJANGO=1.3.1
  - DJANGO=1.4.3 DJANGO_SETTINGS_MODULE="mysite.travis_settings"
install:
  - pip install -q Django==$DJANGO --use-mirrors
  - pip install pep8 --use-mirrors
  - pip install mysql-python --use-mirrors
  # - pip install https://github.com/dcramer/pyflakes/tarball/master
  # - pip install -q -e . --use-mirrors
before_script:
#   - "pep8 --exclude=migrations --ignore=E501,E225 src"
#   - pyflakes -x W src
  - mysql -e 'create database mysite_db;'
  - python manage.py syncdb --noinput
script:
  - python manage.py test

```

Enjoy :)

[travis]: http://travis-ci.org