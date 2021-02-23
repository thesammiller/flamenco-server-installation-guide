# Blender Cloud - Part C

## Step 1: Apache Dependencies
**I am not an expert on Apache so I do not fully understand this step**    
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
`service cron start`a     


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
cd /data/git/blender-cloud
./gulp all
npm install
```

Next, `gulp` in the `pillar` directory:    
```
cd /data/git/pillar
./gulp
npm install
```


## Step 3: Pillar Dependencies
Make sure that you have the virtualenvwrapper installed.
```
sudo apt-get install virtualenvwrapper
```

The installation files are missing a `requirements.txt` for Pillar. There may be some issues with dependencies. Since the project is a few years old, it's important to make sure that you have the correct version of the dependencies. Below are a few of them along with their version. You can install by using the command:    
`pip install module==####`    
...where `module` is the module name and `####` is the version number.    

For a list of modules from the development environment, please see the end of this file.    

There may also be an error for a missing module `email_validator`. You should be able to `pip` install:    
`pip3 install email_validator`



### Developer Pillar Module Versions       
```
algoliasearch            1.20.0   
amqp                     2.5.0    
asn1crypto               0.24.0   
atomicwrites             1.3.0    
attract                  1.1.dev0 /home/sybren/workspace/cloud/attract          
attrs                    19.1.0   
Babel                    2.7.0    
bcrypt                   3.1.6    
billiard                 3.6.0.0  
bleach                   3.1.0    
blinker                  1.4      
celery                   4.3.0    
Cerberus                 1.3.1    
certifi                  2019.3.9 
cffi                     1.12.3   
chardet                  3.0.4    
Click                    7.0      
commonmark               0.9.0    
coverage                 4.5.3    
cryptography             2.7      
elasticsearch            6.1.1    
elasticsearch-dsl        6.1.0    
Eve                      0.9.1    
Events                   0.3      
fasteners                0.15     
flamenco                 2.3.dev0 /home/sybren/workspace/cloud/flamenco         
Flask                    1.0.3    
Flask-Babel              0.12.2   
Flask-Caching            1.7.2    
Flask-DebugToolbar       0.10.1   
Flask-Login              0.4.1    
Flask-Script             2.0.6    
Flask-WTF                0.14.2   
future                   0.17.1   
gax-google-logging-v2    0.8.3    
gax-google-pubsub-v1     0.8.3    
gcloud                   0.18.3   
google-apitools          0.5.28   
google-gax               0.12.5   
googleapis-common-protos 1.6.0    
grpc-google-logging-v2   0.8.1    
grpc-google-pubsub-v1    0.8.1    
grpcio                   1.21.1   
httplib2                 0.12.3   
idna                     2.8      
importlib-metadata       0.17     
ipaddress                1.0.22   
IPy                      1.0      
itsdangerous             1.1.0    
Jinja2                   2.10.1   
kombu                    4.6.0    
MarkupSafe               1.1.1    
monotonic                1.5      
more-itertools           7.0.0    
mypy                     0.501    
ndg-httpsclient          0.5.1    
nose                     1.3.7    
oauth2client             4.1.3    
pillar                   2.0      /home/sybren/workspace/cloud/pillar           
pillar-devdeps           1.0      /home/sybren/workspace/cloud/pillar/devdeps   
pillar-svnman            0.1.dev0 
pillarsdk                1.10     /home/sybren/workspace/cloud/pillar-python-sdk
Pillow                   6.0.0    
pip                      18.1     
pluggy                   0.12.0   
ply                      3.8      
protobuf                 3.8.0    
py                       1.8.0    
pyasn1                   0.4.5    
pyasn1-modules           0.2.5    
pycparser                2.19     
PyJWT                    1.7.1    
pymongo                  3.8.0    
pyOpenSSL                19.0.0   
pytest                   4.4.2    
pytest-cov               2.7.1    
python-dateutil          2.8.0    
pytz                     2019.1   
rauth                    0.7.3    
raven                    6.10.0   
redis                    3.2.1    
requests                 2.22.0   
responses                0.10.6   
rsa                      4.0      
setuptools               40.6.2   
shortcodes               2.5.0    
simplejson               3.16.0   
six                      1.12.0   
svn                      0.3.46   
svnman                   1.1.dev0 /home/sybren/workspace/cloud/pillar-svnman    
typed-ast                1.0.4    
urllib3                  1.22     
vine                     1.3.0    
webencodings             0.5.1    
Werkzeug                 0.15.4   
WTForms                  2.2.1    
zencoder                 0.6.5    
zipp                     0.5.1    
```

