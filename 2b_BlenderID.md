# Blender ID - Part B


# Step 1: Configure Blender Cloud to work with Blender ID

At this point, the Blender Cloud `config_local.py` and `.blender-id/bid_main/fixtures/blender_cloud_devserver.json` need to correlate `client ID`.
Blender Cloud is an OAuth Application and is also called a "client". Blender ID must be aware of which client applications should have access to its information.    

If you have already run the above commands, you can adjust with MySQL commands.    
To check the `client ID` expected by Blender ID, run the following command in `MySQL`:    
`SELECT * FROM bid_main_oauth2application;`    

**I'm not sure that I needed to run these commands **    
Some other commands to run:    
```
CREATE USER 'blender_id'@'%' IDENTIFIED BY 'jemoeder';
GRANT ALL ON blender_id.* to 'blender_id'@'%';
FLUSH PRIVILEGES;
SHOW GRANTS FOR 'blender_id'@'%';
```
If you run into a `misdirected URI` error, you may want to try some of the following adjustments:
```
UPDATE bid_main_oauth2application SET redirect_uris="http://cloud.local:5000/oauth/blender-id/authorized";
UPDATE bid_api_webhook SET url="http://cloud.local:5000/api/webhooks/user-modified";
UPDATE oauth2_provider_grant SET redirect_uri="http://cloud.local:5001/oauth/blender-id/authorized";
```

At this point, you should be able to log-in to Blender Cloud with your email as the Blender ID.

The CLIENT SECRET in `id.local:8000/admin/` must match the CLIENT SECRET in `.blender-id/bid_main/fixtures/blender_cloud_devserver.json`.

If you run into a CSRF Token error, edit `/data/git/flamenco/flamenco/templates/flamenco/managers/linking/choose_manager.html` to include:    
`<input type="hidden" name="csrf_token" value="{{ csrf_token() }}" />`

You should also assign yourself a role such as "cloud-subscriber". You can do this in the console.

## Step 2: Complete Linking Flamenco Manager

Follow the Flamenco instructions. You should now be able to complete linking with your Flamenco Manager.

