---
layout: post
title: "Django Fun"
date: 2013-01-24 22:29
comments: true
categories: "Python"
---

Here's how to install python on your web server :)

###Log in using SSH access
This process may change from different kind of hosts to different machines so find out first.

###Install Python 2.7.2

```
mkdir ~/python
cd ~/python
wget http://www.python.org/ftp/python/2.7.2/Python-2.7.2.tgz
tar zxfv Python-2.7.2.tgz
rm -rf Python-2.7.2.tgz
find ~/python -type d | xargs chmod 0755
cd Python-2.7.2
./configure --prefix=$HOME/python
make
make install
cd ..
rm -rf Python-2.7.2
```

###Modify ~/.bashrc

```
vim ~/.bashrc # Press i and enter the following:
export PATH=$HOME/python/bin:$PATH # press <escape>:wq<enter>
source ~/.bashrc # Whenever you want to work using Python 2.7 in the
                 # console you will need to enter this
```

Test your Python install

```
python -V # This should output "Python 2.7.2"
```

###Install setuptools

```
wget http://pypi.python.org/packages/source/s/setuptools/setuptools-0.6c11.tar.gz
tar xzvf setuptools-0.6c11.tar.gz
rm setuptools-0.6c11.tar.gz
cd setuptools-0.6c11
python setup.py install
cd ..
rm -rf setuptools-0.6c11
```

###Install pip

```
wget http://pypi.python.org/packages/source/p/pip/pip-1.2.1.tar.gz
tar xzvf pip-1.2.1.tar.gz
rm pip-1.2.1.tar.gz
cd pip-1.2.1
python setup.py install
cd ..
rm -rf pip-1.2.1
```

###Install Django, flup and MySQL Python Module

Now that pip is installed this is easy part.

```
pip install Django
pip install flup
pip install MySQL-python
```

###Create .htaccess

```
cd ~/public_html/mysite # where mysite is the root folder of your site
vim .htaccess # Press i then enter the following:
AddHandler fcgid-script .fcgi
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)$ mysite.fcgi/$1 [QSA,L] # Press wq
```

###Configure Your Website

```
mkdir ~/django-projects
cd ~/django-projects
django-admin.py startproject myproject # I use the same name as mysite
                                       # without any periods
cd ~/public_html/mysite
vim mysite.fcgi # Press i then enter the following:
#!/homeX/your_username/python/bin/python
import sys, os

# Where /home/your_username is the path to your home directory
sys.path.insert(0, "/home/your_username/python")
sys.path.insert(13, "/home/your_username/django-projects/myproject")

os.environ['DJANGO_SETTINGS_MODULE'] = 'myproject.settings'
from django.core.servers.fastcgi import runfastcgi
runfastcgi(method="threaded", daemonize="false") # Press <escape>wq<enter>
chmod 0755 mysite.fcgi
``` 

###Test your configuration

```
python mysite.fcgi


# Fix any errors and run the command again. When you have an HTML page
# returned your Django install is complete. You should be able to go to your
# sites URL and view the Django welcome page.
``` 

That’s everything

Now you can get to work and create some real Django pages.

###A Few Tips

Remember to install every python and django package using pip – that way they will be available to your django app.
Keep in mind that every Django command like syncdb, collectstatic  etc. should be run using the correct version of Python. This means using source ~/.bashrc every time you log into your SSH client e.g.
source ~/.bashrc
python manage.py syncdb
python manage.py shell
etc.
