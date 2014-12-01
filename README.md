# Dockito Vagrant box

* Start by cloning this repository inside a `Development` folder where you keep all your projects;
* Then go inside this new folder: `cd Development\devbox`;
* Start the vagrant box: `vagrant up`;
* Log in into your new box: `vagrant ssh`.

It will sync its parent folder (`Development`), making all your projects available inside the VM at the `/vagrant` folder.

You will have a [CoreOS](http://coreos.com/) install with [Fig](http://fig.sh/) and an [HTTP Proxy](https://github.com/dockito/proxy). For more information on how the image is built, check https://github.com/dockito/devbox-packer.
