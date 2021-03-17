# Troubleshooting Common Problems

***What should I do if the Python interpreter can’t find Pillar?***    

While you have your blender cloud virtualenv activated, do a pip install -e . from the pillar directory. That should ensure that Python finds it. You can also run poetry run pip install -e .    
Worst case scenario, you can [convert](https://gist.github.com/adamghill/8a96311b5cf1790c72a0b825ffe4f6d2) the pyproject.toml to setup.py and try to install dependencies manually.    

***How do I know if the Blender Cloud installation has been successful?***    

Visit cloud.local:5000/ - you should see a “welcome redirection” message. When you visit cloud.local:5000/welcome you should see the Blender Cloud homepage.    

***What are some things I can try if things are not working?***    
- Review the Docker files for Blender Cloud and Blender ID    
- In config_local.py     
  * Make sure that all ports are changed to “5000” and not “5001”    
  * Make sure you set the MAIN_PROJECT_ID    
  * Make sure that you are using “HTTP” and not “HTTPS”    
- Make sure the Secret Key matches between the Blender ID and Blender Cloud configuration files    
  * Config_local.py    
  * settings.py    

***What should I do if Blender Cloud can’t find mongodb?***    

Make sure that you’ve edited `/etc/hosts` to include the IP address for mongoDB.    
You should also make sure that your Docker containers are running: `docker ps -a`    

***What should I do if I have errors logging into the local Blender ID?***    

Make sure that you have confirmed your email. See some of the MySQL commands below.    

***What should I do if I have a “misdirected URL” error in Blender ID?***    

Make sure that you have the right name for the Project ID. You can double check this in `config_local.py` and the MySQL database.    
Also check the following tables and possibly set to these values:    
```
UPDATE bid_main_oauth2application SET redirect_uris="http://cloud.local:5000/oauth/blender-id/authorized";
UPDATE bid_api_webhook SET url="http://cloud.local:5000/api/webhooks/user-modified";
UPDATE oauth2_provider_grant SET redirect_uri="http://cloud.local:5001/oauth/blender-id/authorized";
```

***What should I do if I encounter a JWT error?***
- Confirm that you have generated the appropriate keys.
- Confirm that you have set the variables to the correct location.
- Confirm that the variable names are correct (see Note in installation guide)

***What should I try if I see a 404 'Not found' page?***
- Make sure that you have run `./gulp all` in the Blender-Cloud directory.
- You can also `gulp` in the sub-directories (like Pillar). 
- Make sure that you're connecting to a separated domain name like `cloud.local` - some browsers handle cookies differently so using the IP address or `localhost` will result in failure    


# MySQL Commands    

For troubleshooting, it can be helpful to inspect the MySQL DB for Blender ID. Here are some useful commands:    
```
use blender_id;    
show tables;
select * from bid_main_user;    
UPDATE bid_main_user SET confirmed_email_at=2022-01-01 23:30:00.699372;    
```

If you need to remove MySQL, you can do that with:    
```
sudo apt-get remove -y mysql-*    
sudo apt-get purge -y mysql-*    
```

You can update the root password:       
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new-password';`    
Then reboot:    
```
sudo service mysql stop    
sudo service mysql start    
```
*Note: the new password must be reflected in the `settings.py` file under DATABASES*    
