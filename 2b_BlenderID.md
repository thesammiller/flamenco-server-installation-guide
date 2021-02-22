# Blender ID - Part B

## Step 1: Final Configurations

The CLIENT SECRET in `id.local:8000/admin/` must match the CLIENT SECRET in `.blender-id/bid_main/fixtures/blender_cloud_devserver.json`.

If you run into a CSRF Token error, edit `/data/git/flamenco/flamenco/templates/flamenco/managers/linking/choose_manager.html` to include:    
`<input type="hidden" name="csrf_token" value="{{ csrf_token() }}" />`

## Step 2: Complete Linking Flamenco Manager

Follow the Flamenco instructions. You should now be able to complete linking with your Flamenco Manager.

