# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "dockito/devbox"
  
  # CoreOS replaces the authorised_keys on every boot based on its cloud-config file
  # But Vagrant change its default behavior on 1.7 to to update the authorized_keys with a secure one on the first boot
  # To allow password-less login, we need this disabled
  # making feature https://github.com/mitchellh/vagrant/issues/2608#issuecomment-67446310 useless
  config.ssh.insert_key = false

  if (/linux/ =~ RUBY_PLATFORM) != nil
    # Allows chown operations in the shared folders
    # Fixes `npm install` execution in containers with volumes
    # Side-effects: node_modules will be created to root users in the host machine
    config.vm.synced_folder "../", "/vagrant", nfs: true, linux__nfs_options: ["no_root_squash", "rw", "no_subtree_check"]
  else
    config.vm.synced_folder "../", "/vagrant", nfs: true
  end
end
