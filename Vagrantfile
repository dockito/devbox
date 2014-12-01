# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "dockito/devbox"

  if (/linux/ =~ RUBY_PLATFORM) != nil
    # Allows chown operations in the shared folders
    # Fixes `npm install` execution in containers with volumes
    # Side-effects: node_modules will be created to root users in the host machine
    config.vm.synced_folder "../", "/vagrant", nfs: true, linux__nfs_options: ["no_root_squash", "rw", "no_subtree_check"]
  else
    config.vm.synced_folder "../", "/vagrant", nfs: true
  end
end
