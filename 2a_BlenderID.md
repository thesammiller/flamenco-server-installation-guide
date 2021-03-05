# Blender ID - Part A

## Step 0: Install Flamenco Manager

At this point, you should have a running Blender Cloud. The following step is not necessary, but it may be helpful to run Flamenco Manager and understand how the pieces are coordinated. [Download](https://www.flamenco.io/download/) the Flamenco Manager (and the Worker while you're there). If these terms are confusing, take a look at the official [terminology](https://www.flamenco.io/docs/user_manual/terminology/).    

Unzip the files and from the `flamenco-manager` directory, run `./flamenco-manager -setup`    

You should find a URL listed towards the end of the server output. Go to that address in a browser. You should see the linking page for Flamenco.

You should enter your local Blender Cloud URL in the form:     
`http://cloud.local:5000`

You will be asked to login. When you do so, you'll get an error. 

Flamenco Managers must be authorized by the Flamenco Server. The Flamenco Server is currently authorized through Blender Cloud by Blender ID. The next step in the installation process is setting up the Blender ID server.    

## Step 1: Git and VirtualEnv
References: [Blender ID Docker](https://developer.blender.org/diffusion/BID/browse/master/docker/)
*Note: Most of these commands are from the Blender ID installation notes. However, there are further details.*

`git clone git://git.blender.org/blender-id.git`

*Note: I am not sure if all of the following adjustments are needed, I am still testing.*    

- Copy `blenderid/__settings.py` to `blenderid/settings.py` and adjust for your needs.    
 * Set BLENDER_ID_ADDON_CLIENT_ID to the ID in config_local.py for the Cloud    
 * Change DEFAULT_FROM_EMAIL to the email you use to establish Blender Cloud    
 * Create a Unique Secret Key    
 * Remove 'https' from OAUTH2_PROVIDER if you're not usinfg https (or something like stunnel)    
 * Add "id.local" to INTERNAL_IPS (especially if you are not running id.local on localhost)    
 * Make sure that you have uncommented “ALLOWED_HOSTS” in blenderid/settings.py (for testing only)    

- Run `git submodule init` and `git submodule update`    

# Step 2: Install MySQL
You must install `MySQL`:    
`sudo apt-get install mysql-server mysql-client libmysqlclient-dev`

If you run into errors, try to install in verbose mode. 
There might be a stall on installing `keyring`. To get around that, set the following environment variable:    
`export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring`

(if you get an error when installing the `mysqlclient` package on macOS,
  [change your mysql_config](https://github.com/PyMySQL/mysqlclient-python#note-about-bug-of-mysql-connectorc-on-macos)).


Configure `MySQL`:
`mysql_secure_installation`
Set the password for `root` to `root`.
When you are done configuring, you should be able to run the following command:    
`sudo mysql -u root -p`

*Note: If you cannot run the previous command with the password `root`, you will get an error when trying to run Blender ID if you do not change the password in the `DATABASES` dictionary in `settings.py`.*

# Step 3: Coordinate settings between Blender Cloud and BlenderID:

*Note: The following assumes you are install Blender Cloud and Blender ID on the same computer. This will be most use \
cases.*

- Make sure that the Client ID matches in two locations:
  *  `config_local.py` -> blender-id['id'] 
  *  `bid_main/fixtures/blender_cloud_devserver.json` -> fields[client_id]
- From inside the `blender-id` directory, run `poetry install` to install the necessary dependencies
- Create the database with `mysqladmin create blender_id --default-character-set=utf8`.

*Note: The following python commands should be run with a `poetry run` prefix. e.g. `poetry run ./manage.py migrate`*

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
  * Enter your email address
  * Enter your password
  * Run the following command in MySQL -> `UPDATE bid_main_user SET confirmed_email_at="021-02-21 16:31:43.343342";`
  * You must confirm your email before you can use the superuser.
- Load any fixtures you want to use.
   - list fixtures  `ls */fixtures/*`
   - `./manage.py createcachetable`
   - `./manage.py loaddata default_site`
   - `./manage.py loaddata default_roles`

Instead of `./manage.py createsuperuser`, you can also create a user on Blender Cloud:
- Make sure that your Blender Cloud instance is running (which also requires the Dockers running).
  * `/data/git/blender-cloud/poetry run ./manage.py runserver`
- Run the Blender ID site (leave the terminal open):
  * '/data/git/blender-id/poetry run ./manage.py runserver`
- Using a web browser, navigate to `cloud.local:5000/welcome`.
- Click `Login`.
- Underneath the login, sign up for an account. (do not click Sign-Up from the homepage as this will redirect you to the Blender Store)
- After you have entered your information, go to the Blender ID terminal.
- You will see the email that was sent to confirm the password.
- Click the link and go to the webpage.
- Enter a password and click submit.

*Note: You must confirm your email address. If you closed the terminal for Blender ID server, you can do this through MySQL. (see command above)*
*Note: If you do not create a super user with `./manage.py createsuperuser` you will have to set the appropriate flags in MySQL*

Once you have created an account, you can continue with the rest of the management setup:
   - `./manage.py loaddata blender_cloud_devserver`
   - `./manage.py loaddata blender_cloud_dev_webhook`
   - `./manage.py loaddata flatpages`
- Run `./manage.py collectmedia` to collect media from fixtures and place into the media directory.
  * If you are prompted to overwrite files, say yes.
- Add to `/etc/hosts`:  `127.0.0.1 id.local` (this is not necessary if you followed Blender Cloud instructions)

**Warning from the Developers: Please do *not* use the devserver settings as-is. This will make your install insecure as everybody has access to that secret. You can use it as a basis, but change the secret so that it's totally new and random. The best way to do this is to create a new OAuth Application in the Blender ID admin and use the devserver settings as a template. Then delete the devserver OAuth Application.**    

