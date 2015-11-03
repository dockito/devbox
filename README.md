# Dockito Vagrant box

* Start by cloning this repository inside a `Development` folder where you keep all your projects;
* Then go inside this new folder: `cd Development\devbox`;
* Start the vagrant box: `vagrant up`;
* Log in into your new box: `vagrant ssh`.

It will sync its parent folder (`Development`), making all your projects available inside the VM at the `/vagrant` folder.

You will have a [CoreOS](http://coreos.com/) install with [Compose (former Fig)](https://github.com/docker/compose) and an [HTTP Proxy](https://github.com/dockito/proxy).

## Adding Disk Space to Your CoreOS Machine

https://coreos.com/docs/cluster-management/scaling/adding-disk-space/

## Troubleshooting First Vagrant Up

In case occur some weird problem in the first vagrant up the following steps have usually solved the it:

```
# runs the provision manually
vagrant provision
# reloads the vm again
vagrant reload
```

## Custom provision

Since each developer has their own preferences for what should be automatically configured in the VM (e.g. initial access folder and custom alias). Including a **custom-provision.sh** inside devbox directory it will be provisioned by Vagrant.

**Basic example:**

```bash
# bash_profile on CoreOS is read-only on file system
# due that we clone the original one and uses this new copy
unlink /home/core/.bash_profile
cp /usr/share/skel/.bash_profile /home/core/.bash_profile


# configures bash profile to access the vagrant folder
# after log into the vm through ssh
echo "
cd /vagrant/bravi
" >> /home/core/.bash_profile
```
