STAGE ONE: INSTALLING BLENDER CLOUD

*“Welcome to open source, where you’re not the only one running your website.”* - Dr. Sybren

Step 1: Install Ubuntu
The first step is to install a clean Ubuntu machine. Virtual machines are easy ways to create fresh installations. The following steps were completed using VirtualBox and Vagrant. 

Step 2: Install basic dependencies on Ubuntu (from `../blender-cloud/docker/1_base/Dockerfile`)
`apt-get install -y tzdata openssl ca-certificates locales`

To install Blender-Cloud, we need a Python interpreter. The Blender-Cloud Docker builds from source.

Step 3: Install Python (from `../blender-cloud/docker/2_buildpy/buildpy.docker`)

```
sudo sed -i 's/^# deb-src/deb-src/' /etc/apt/sources.list
sudo apt-get update
sudo apt-get install -y build-essential apache2-dev checkinstall curl
mkdir python
curl -O https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tar.xz
tar xf Python-3.6.6.tar.xz
rm -v Python-3.6.6.tar.xz`
mkdir /opt/python
export PATH="/opt/python/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
cp python /opt/python
Cd ~/python/Python-3.6.6
./configure \
    --prefix=/opt/python \
    --enable-ipv6 \
    --enable-shared \
    --with-ensurepip=upgrade
chown -R  $UID:$GID /opt/python
make -j8 install
```

If you run into Zlib error -> sudo apt-get install zlib1g-dev

Ldconfig (?)
/opt/python/bin/python3 -m pip install -U pip
If you run into SSL errors: rerun .configure with --with-ssl

From 2_buildpy/ (files)
Mod-WSGI is a simple internet server. It is part of the Apache system.


Step 3: Install mod-wsgi from source
mkdir -p /dpkg && cd /dpkg
apt-get source libapache2-mod-wsgi-py3

cd /dpkg/mod-wsgi-*
./configure --with-python=/opt/python/bin/python3
make -j8 install
mkdir -p /opt/python/mod-wsgi
cp /usr/lib/apache2/modules/mod_wsgi.so /opt/python/mod-wsgi

/opt/python/bin/python3 setup.py install

Step 4: Post Python/mod-wsgi install
echo /opt/python/lib > /etc/ld.so.conf.d/python.conf
Workaround: sudo vi create the file

Cd /opt/python/bin
Sudo ln -s python3 python

