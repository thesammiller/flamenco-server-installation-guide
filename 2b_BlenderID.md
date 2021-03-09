# Blender ID - Part B


## Step 1: Configure Blender Cloud to work with Blender ID

You need to go into Blender Cloud and make sure your user is authorized to connect Flamenco:
- Log in to Blender Cloud
- Create a new user
- Open the terminal and in the `blender-id` folder run `poetry run ./manage.py makesuperuser <youremail>`
- Go to `id.local:8000/admin/` and log in.
- Click `Whoosh`
- Click on OAuth Application and confirm that "Redirect URI" is `...cloud.local:5000...` 
- While in OAuth Application, make sure that your `client secret` is correct.
- Go into the User settings and give your user badges `cloud-subscriber` and `cloud-has-subscription` so they have access.
- Go to Blender Cloud at `cloud.local:5000`
- You should now be able to log in as your user.


## Step 2: Complete Linking Flamenco Manager

Follow the Flamenco instructions. You should now be able to complete linking with your Flamenco Manager.

If you have further issues, please see the Troubleshooting guide or post on the [Flamenco channel](https://blender.chat/channel/flamenco)
