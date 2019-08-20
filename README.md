Jasmin Web Panel

Jasmin SMS Web Interface and REST API for Jasmin SMS Gateway
Pre-requirements

You must install Jasmin SMS Gateway via official document
Installation

Download and Extract folder We recommend installing in a virtualenv

Install dependencies:

$ pip install -r requirements.pip

cd to JasminWebPanel and run:

$ ./manage.py migrate 
$ ./manage.py createsuperuser 
$ ./manage.py collectstatic

The last is only needed if you are running the production server (see below) rather than the Django dev server. It should be run again on any upgrade that changes static files. If in doubt, run it. You can also override the default settings for the telnet connection in local_settings.py. These settings with their defaults are:

TELNET_HOST = '127.0.0.1'
TELNET_PORT = 8990
TELNET_USERNAME = 'jcliadmin'
TELNET_PW = 'jclipwd'

Running

To run for testing and development:

$ ./manage.py runserver

This is slower, requires DEBUG=True, and is much less secure
Deployment

To run on production:

$ sudo apt-get install apache2 libapache2-mod-wsgi
$ cp 000-default.conf /etc/apache2/sites-available/000-default.conf
$ sudo a2enmod wsgi
$ a2ensite 000-default.conf
$ service apache2 restart

This requires that you run the collectstatic command (see above) and you should have DEBUG=False.
Docker usage

Environment variables

DB_ENGINE='postgres' (default: sqlite)
DB_NAME= (default: jasmin-smpp)
DB_HOST= (No Default)
DB_USER= (No Default)
DB_PASSWORD= (No Default)

JASMIN_HOST= (default: jasmin)
JASMIN_PORT= (default: 8990
JASMIN_USERNAME= (default: jcliadmin)
JASMIN_PASSWORD= (default: jclipwd)

Build and Run docker

docker build --rm -t local/jasminwebpanel:master ./

docker run -p 8000:8000 -e DB_ENGINE=postgres -e DB_HOST=postgres \
	 -e DB_USER=airflow -e DB_PASSWORD=airflow \
	 -e JASMIN_HOST=jasmin -e JASMIN_PORT=8990 -e JASMIN_USERNAME=jcliadmin -e JASMIN_PASSWORD=jclipwd \
	 -it local/jasminwebpanel:master
