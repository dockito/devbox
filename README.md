# Dockito Vagrant box

* Start by cloning this repository inside a `Development` folder where you keep all your projects;
* Then go inside this new folder: `cd Development\devbox`;
* Start the vagrant box: `vagrant up`;
* Log in into your new box: `vagrant ssh`.

It will sync its parent folder (`Development`), making all your projects available inside the VM at the `/vagrant` folder.

You will have a [CoreOS](http://coreos.com/) install with [Fig](http://fig.sh/) and an [HTTP Proxy](https://github.com/dockito/proxy). For more information on how the image is built, check https://github.com/dockito/devbox-packer.


## Custom provision

Since each developer has their own preferences for what should be automatically configured in the VM (e.g. initial access folder and custom alias). Including a **custom-provision.sh** inside devbox directory it will be provisioned by Vagrant.

**Basic example:**

```bash
# bash_profile on CoreOS is read-only on file system
# due that we clone the original one and uses this new copy
unlink /home/vagrant/.bash_profile
cp /usr/share/skel/.bash_profile /home/vagrant/.bash_profile

# configures bash profile to access the vagrant folder
# after log into the VM through ssh
echo "
cd /vagrant
" >> /home/vagrant/.bash_profile
```
