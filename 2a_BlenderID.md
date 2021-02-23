# Blender ID - Part A

## Step 0: Install Flamenco Manager

At this point, you should have a running Blender Cloud. This is not necessary, but it may be helpful to run Flamenco Manager and understand how the pieces are coordinated. [Download](https://www.flamenco.io/download/) the Flamenco Manager (and the Worker while you're there). If these terms are confusing, take a look at the official [terminology](https://www.flamenco.io/docs/user_manual/terminology/).    

Unzip the files and from the `flamenco-manager` directory, run `./flamenco-manager -setup`    

You should find a URL listed towards the end of the server output. Go to that address in a browser. You should see the linking page for Flamenco.

You should enter your local Blender Cloud URL in the form:     
`http://cloud.local:5000`

You will be asked to login. When you do so, you'll get an error. 

Flamenco Managers must be authorized by the Flamenco Server. The Flamenco Server is currently authorized through Blender Cloud by Blender ID. The next step in the installation process is setting up the Blender ID server.    

## Step 1: Git and VirtualEnv
References: [Blender ID Docker](https://developer.blender.org/diffusion/BID/browse/master/docker/)
*Note: Most of these commands are from the Blender ID installation notes. However, there are further details.*

`git clone ##blenderid##`

- Copy `blenderid/__settings.py` to `blenderid/settings.py` and adjust for your needs.
- Make sure that you have uncommented “ALLOWED_HOSTS” in blenderid/settings.py
- Run `git submodule init` and `git submodule update`
- Create a virtual environment (with Python 3.6) by running `pipenv install --dev`
  (if you get an error when installing the `mysqlclient` package on macOS,
  [change your mysql_config](https://github.com/PyMySQL/mysqlclient-python#note-about-bug-of-mysql-connectorc-on-macos)).
  Note that from now on we assume you run from a `pipenv shell` or prefix commands with
  `pipenv run`.


# Step 2: Install MySQL
You must install `MySQL`:    
`sudo apt-get install mysql-server mysql-client libmysqlclient-dev`

If you run into errors, try to install in verbose mode. 
There might be a stall on installing `keyring`. To get around that, set the following environment variable:    
`export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring`

Configure `MySQL`:
`mysql_secure_installation`
Set the password for `root` to `root`.
When you are done configuring, you should be able to run the following command:    
`sudo mysql -u root -p`

*Note: If you cannot run the previous command with the password `root`, you will get an error.*

# Step 3: Continue BlenderID Installation

- Create the database with `mysqladmin create blender_id --default-character-set=utf8`.
- Run `./manage.py migrate` to migrate your database to the latest version.
- Run `./manage.py createcachetable` to create the cache table in the database.
- Run `mkdir media` to create the directory that'll hold uploaded files
  (such as images for the badges).
- In production, set up a cron job that calls the
  [cleartokens](https://django-oauth-toolkit.readthedocs.io/en/latest/management_commands.html#cleartokens)
  management command regularly.
- In production, set up a cron job that calls the `flush_webhooks --flush -v 0` management command
  regularly.
- Run `./manage.py createsuperuser` to create super user
- Load any fixtures you want to use.
   - list fixtures  `ls */fixtures/*`
   - `./manage.py createcachetable`
   - `./manage.py loaddata default_site`
   - `./manage.py loaddata default_roles`
   - `./manage.py loaddata blender_cloud_devserver`
   - `./manage.py loaddata blender_cloud_dev_webhook`
   - `./manage.py loaddata flatpages`
- Run `./manage.py collectmedia` to collect media from fixtures and place into the media directory.
- Add to /etc/hosts  127.0.0.1 id.local

*Note: You must confirm your email address. If you do not receive an email, you can do this through MySQL.*

**Warning from the Developers: Please do *not* use the devserver settings as-is. This will make your install insecure as everybody has access to that secret. You can use it as a basis, but change the secret so that it's totally new and random. The best way to do this is to create a new OAuth Application in the Blender ID admin and use the devserver settings as a template. Then delete the devserver OAuth Application.**    

