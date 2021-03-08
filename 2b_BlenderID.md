# Blender ID - Part B


## Step 1: Configure Blender Cloud to work with Blender ID

At this point, the Blender Cloud `config_local.py` and `.blender-id/bid_main/fixtures/blender_cloud_devserver.json` need to correlate `client ID`.
Blender Cloud is an OAuth Application and is also called a "client". Blender ID must be aware of which client applications should have access to its information.     

Client Secret has to correspond in three places:    
- `/data/git/blender-id/blenderid/settings.py` --> `SECRET_KEY`    
- `/data/git/blender-id/bid_main/fixtures/blender_cloud_devserver.json` --> `client_secret`    
- `http://id.local:8000/admin/` --> OAuth Application `client secret`  

You need to go into Blender Cloud and make sure your user is authorized to connect Flamenco:
- Log in to Blender Cloud
- Create a user
- Open the terminal and in the `blender-id` folder run `poetry run ./manage.py makesuperuser <youremail>`
- Refresh your Blender Cloud
- Click `Whoosh`
- Create an OAuth Token, giving the token a name that you want and making sure that it lasts a long time (i.e. set date `2999-01-01`)
- Change "Redirect URI" from `...cloud.local:5001...` to `...cloud.local:5000...`
- Go into the User settings and give your user badges like `cloud-subscriber` so they have access.
- Go to `id.local:8000/admin`


## Step 2: Complete Linking Flamenco Manager

Follow the Flamenco instructions. You should now be able to complete linking with your Flamenco Manager.

If you have further issues, please see the Troubleshooting guide or post on the [Flamenco channel](https://blender.chat/channel/flamenco)
