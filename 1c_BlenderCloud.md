# Blender Cloud - Part C

## Step 1: Apache Dependencies

We need to make sure that Apache is properly configured. This will interact with the `mod-wsgi` installation from Part B.    

`sudo apt-get install -y git apache2 libapache2-mod-xsendfile libjpeg8 libtiff5 ffmpeg rsyslog logrotate curl`    
```
export APACHE_RUN_USER=www-data
export APACHE_RUN_GROUP=www-data
export APACHE_LOG_DIR=/var/log/apache2
export APACHE_PID_FILE=/var/run/apache2.pid
export APACHE_RUN_DIR=/var/run/apache2
export APACHE_LOCK_DIR=/var/lock/apache2
mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR
```
```
sudo cp docker/4_run/apache/remoteip.conf /etc/apache2/mods-available/
sudo cp docker/4_run/apache/wsgi-py36.* /etc/apache2/mods-available/
sudo su - 
a2enmod remoteip && a2enmod rewrite && a2enmod wsgi-py36
exit
sudo cp docker/4_run/apache/apache2.conf /etc/apache2/apache2.conf
sudo cp docker/4_run/apache/000-default.conf /etc/apache2/000-default.conf
sudo cp docker/4_run/apache/logrotate.conf /etc/logrotate.d/apache2
sudo cp docker/4_run/*.sh /
```
`service cron start`    


## Step 2: Gulp
References: Pillar & Blender Cloud Installation
A system like this requires a number of dependencies. Pillar is built on top of JavaScript, but is provided to Blender-Cloud as a Python SDK. As such, we have to download dependencies for JavaScript in addition to Python.    

```
sudo apt-get install nodejs npm
```

If you have issues installing node: 
```
sudo curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -       
sudo apt -y install nodejs make gcc g++
```
First, `gulp` in the `blender-cloud` directiory:    
```
./gulp all
```

Next, `gulp` in the `pillar`, `flamenco` and `pillar-svnman` directories:    
```
./gulp
```


## Step 3: Pillar Dependencies
Make sure that you have the virtualenvwrapper installed.
```
sudo apt-get install virtualenvwrapper
```

The installation files are missing a `requirements.txt` for Pillar. There may be some issues with dependencies. Since the project is a few years old, it's important to make sure that you have the correct version of the dependencies. Check the appendix for developer dependency versions. You can install by using the command:    
`pip install module==####`    
...where `module` is the module name and `####` is the version number.    

For a list of modules from the development environment, please see the end of this file.    

There may also be an error for a missing module `email_validator`. You should be able to `pip` install:    
`pip3 install email_validator`


