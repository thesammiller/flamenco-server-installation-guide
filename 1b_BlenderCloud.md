#Blender Cloud Installation - Part B

##Step 1: Install Git Repos
Reference: [Blender-Cloud readme](https://developer.blender.org/diffusion/BC/)    

If you haven't already, copy the Git repositories.     
*Please note: There are some Git repositiories for Flamenco on GitHub that are out of date. Use the links below.*    

```
git clone git://git.blender.org/pillar-python-sdk.git
git clone git://git.blender.org/pillar.git
git clone git://git.blender.org/attract.git
git clone git://git.blender.org/flamenco.git
git clone git://git.blender.org/pillar-svnman.git
git clone git://git.blender.org/blender-cloud.git
```

##Step 2: Poetry     
Reference: [docker/3_buildwheels/build.sh](https://developer.blender.org/diffusion/BC/browse/master/docker/3_buildwheels/build.sh)    

A Python interpreter must be set up with the appropriate dependencies. In order to avoid conflicts with other dependency versions, Python uses Poetry, which is a wrapper for a virtual environment. Virtual environments are ways to isolate Python distributions on a machine. While they can be very helpful, they can also be convoluted. If you run into trouble with a module that you expect to be installed, it’s likely a problem with virtual environments. It can sometimes be a good idea to uninstall then reinstall Poetry and/or virtualenv in order to get the appropriate compiler working.     

```
pip3 install wheel poetry==1.0
cd blender-cloud
poetry install --no-dev
```

The Python `cryptography` module has been updated and now requires a `rust` installation. As of this writing (2-22-21), this can be by-passed with the following environment variable:    
`export CRYPTOGRAPHY_DONT_BUILD_RUST=1`    

##Step 3: Wheelhouse Configuration 
Reference: [docker/3_buildwheels/build.sh](https://developer.blender.org/diffusion/BC/browse/master/docker/3_buildwheels/build.sh)    

Blender-Cloud uses two different “wheel” programs. Python3 or Pip3 wheel is a system that installs dependencies. “Wheelhouse” is a content management system. In this next step we use Pip’s “wheel” to install “wheelhouse."    

Poetry expects `setup.py` files and will not work without them. To get around this, we need to create a `requirements.txt`. To do this, run the following command:
`poetry run pip3 freeze | grep -v '\(pillar\)\|\(^-[ef] \)' > requirements.txt`

When you've created a requirements file, you can run:
`pip3 wheel --wheel-dir=./data/wheelhouse -r requirements.txt`

After that, we can set up some environment variables.
```
export WHEELHOUSE=/data/wheelhouse
export PIP_WHEEL_DIR=/data/wheelhouse
export PIP_FIND_LINKS=/data/wheelhouse
mkdir -p $WHEELHOUSE
```
