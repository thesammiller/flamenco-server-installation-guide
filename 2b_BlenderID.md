# Blender ID - Part B


# Step 1: Configure Blender Cloud to work with Blender ID

At this point, the Blender Cloud `config_local.py` and `.blender-id/bid_main/fixtures/blender_cloud_devserver.json` need to correlate `client ID`.
Blender Cloud is an OAuth Application and is also called a "client". Blender ID must be aware of which client applications should have access to its information.    


At this point, you should be able to log-in to Blender Cloud with your email as the Blender ID.

The CLIENT SECRET in `id.local:8000/admin/` must match the CLIENT SECRET in `.blender-id/bid_main/fixtures/blender_cloud_devserver.json`.

If you run into a CSRF Token error, edit `/data/git/flamenco/flamenco/templates/flamenco/managers/linking/choose_manager.html` to include:    
`<input type="hidden" name="csrf_token" value="{{ csrf_token() }}" />`

You should also assign yourself a role such as "cloud-subscriber". You can do this in the console.

## Step 2: Complete Linking Flamenco Manager

Follow the Flamenco instructions. You should now be able to complete linking with your Flamenco Manager.

