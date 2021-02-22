# Blender Cloud - Part D

## Step 1: Install Docker 

In order to run Pillar, we need to make sure that we have some services available. MongoDB is a database. Redis is a caching system. RabbitMQ is a messenger. Fortunately, the default Docker images for both of these will result in a working system.  Those interested in Docker should read the Docker Compose YAML file.

Follow the instructions here:
https://docs.docker.com/engine/install/ubuntu/
https://docs.docker.com/engine/install/linux-postinstall/


## Step 2: Run Docker Images
```
sudo mkdir -p /data/storage
sudo chown you:yourgroup /data/storage
```

```
docker run -d -v /data/db:/data/db -p 27017:27017 --name mongo mongo
docker run -d -p 6379:6379 --name redis redis
docker run -d -p 5672:5672 --name rabbit rabbitmq
```

Add the following to `/etc/hosts`:
`127.0.0.1       localhost id.local cloud.local elastic kibana mongodb mongo redis`

# Step 3: Configure Blender_Cloud
Reference: [Blender-Cloud](https://developer.blender.org/diffusion/BC/) and [Pillar Configuration](https://pillarframework.org/development/install/)
At this point in the installation process, we are ready to configure the application and run it. The first step involves copying an example configuration file. The following step sets up the server. 

Copy the example `config_local` and edit the file as neccessary.
`cp config_local.example.py config_local.py`

- Change secret key (per `Pillar` installation tips)
- Change port on cloud.local to 5000


Run the initial setup, which will create the database. *Note: MongoDB Docker must be running.*    
`poetry run ./manage.py setup setup_db <email>`

When the output is finished, copy the value of `<project_id>` and assign it as value for `MAIN_PROJECT_ID` in `config_local.py`.    

You can now run the server. From the `blender-cloud` directory, issue the command:    
`poetry run ./manage.py runserver`

To test the server:
`curl cloud.local:5000`

At this point you should get a welcome redirection.

You can also install `py.test` and run that.
`pip3 install pytest`
`pip3 install pytest-cov`
