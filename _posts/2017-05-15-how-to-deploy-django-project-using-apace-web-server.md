---
layout: post
title: How To Deploy Django Project Using Apache Web Server
description: 
feature-img:
date: 2017-05-15 12:15:00 +08:00
---
### Requirements

- Ubuntu 16.04 Server running on VirtualBox 5.1.20
- Apache Web Server 2.4.18
- A Dinamic IP Address for your server that can be accessed by your OS Host
- Pip 8.1.1 Python(2.7)
- Make sure that your Django Project can run on your server

### Install and Enable mod_wsgi

The mod_wsgi package implements a simple to use Apache module which can host any Python web application which supports the Python WSGI specification.
Run the folllowing command below to install mod_wsgi:

`sudo apt-get install libapache2-mod-wsgi python-dev`

Next, you will need to enable `mod_wsgi` Apache module. To do so, run the following command:

`sudo a2enmod wsgi`

### Configure New VirtualHost for your Django Project

Run the following command, to make new VirtualHost configuration:

`sudo nano /etc/apache2/sites-available/kotakide.conf`

Add the following content to the file:

![](https://farm5.staticflickr.com/4160/34256170025_096f4a1ce3_o_d.png)

Enable the virtual host with the following command:

`sudo a2ensite kotakide.conf`

### Add wsgi file to your project

Create the `wsgi` file by running the following command:

`sudo nano /var/www/djang-kotakide/kotakide.wsgi`

Your content in that file should be similiar to the following content:

```
import os
import sys

sys.path.append('/var/www/django-kotakide/env/lib/python2.7/site-packages')
sys.path.append('/var/www/django-kotakide')

os.environ['DJANGO_SETTINGS_MODULE'] = 'KotakIdeBulukumba.settings'

from django.core.wsgi import get_wsgi_application
application=get_wsgi_application()
```

if you're using environment variable to set up your project, e.g. Database setting. Add the set environment command in your `wsgi` file instead of export it via `cli`. Example:
```
os.environ['DB_NAME'] = 'your_db_name'
os.environ['DB_PASSWORD'] = 'your_db_password'
```

Save the file and restart Apache to apply the changes:

`sudo apachectl restart`

### Static Files

You need to set your static files directory and generate the static files. Add new configuration in `settings.py` file like the following below :

```
STATIC_ROOT = '/var/www/djang-kotakide/static/'
STATIC_URL = '/static/'
```

Then run this following coommand:

`python manage.py collectstatic`

Restart Apache to apply the changes

### Add the New Host on Your OS Host

If you want your Django Project ca be accessed on your OS Host, add that host on `/etc/hosts` by the following format:

`IP-Address-Ubuntu-Server ServerName`

example:
`192.168.43.187 django-kotakide.com`

### Testing

To test your Django Project, open your web browser on your OS Host and type the URL/ServerName (http://yourdomain.com) that you entered in your virtual host configuration.

#### Several Result

**SignUp Page**

![](https://farm3.staticflickr.com/2811/34216387266_02b67d4ac8_o_d.png)

**SignIn Page**

![](https://farm5.staticflickr.com/4161/34099596372_33307acdb0_o_d.png)

**Home Page**

![](https://farm5.staticflickr.com/4171/34216387326_a4390892bc_o_d.png)
