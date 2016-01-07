# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

VM_MEMORY = "1536"

CLOUD_CONFIG_PATH = File.join(File.dirname(__FILE__), "user-data")
$update_channel = "stable"
$image_version = "current"
$shared_folders = {
  "../" => "/vagrant",
  "~/.ssh/" => "/vault/.ssh"
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "coreos-%s" % $update_channel
  if $image_version != "current"
      config.vm.box_version = $image_version
  end
  config.vm.box_url = "http://%s.release.core-os.net/amd64-usr/%s/coreos_production_vagrant.json" % [$update_channel, $image_version]

  ["vmware_fusion", "vmware_workstation"].each do |vmware|
    config.vm.provider vmware do |v, override|
      override.vm.box_url = "http://%s.release.core-os.net/amd64-usr/%s/coreos_production_vagrant_vmware_fusion.json" % [$update_channel, $image_version]
    end
  end

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", VM_MEMORY]
  end

  config.vm.provider :vmware_fusion do |v|
    v.vmx["memsize"] = VM_MEMORY
  end

  config.vm.network :private_network, ip: "10.10.10.10", netmask: "255.255.255.0"

  # CoreOS replaces the authorised_keys on every boot based on its cloud-config file
  # But Vagrant change its default behavior on 1.7 to to update the authorized_keys with a secure one on the first boot
  # To allow password-less login, we need this disabled
  # making feature https://github.com/mitchellh/vagrant/issues/2608#issuecomment-67446310 useless
  config.ssh.insert_key = false

  # mount_options actimeo is used to reduce the delay on watch tasks (like runnig tests on file changes)
  # see: http://stackoverflow.com/questions/27035702/grunt-watch-detects-file-changes-only-after-5-seconds-with-vagrant-and-nfs/28118716#28118716
  $shared_folders.each_with_index do |(host_folder, guest_folder), index|
    if (/linux/ =~ RUBY_PLATFORM) != nil
      # Allows chown operations in the shared folders
      # Fixes `npm install` execution in containers with volumes
      # Side-effects: node_modules will be created to root users in the host machine
      config.vm.synced_folder host_folder.to_s, guest_folder.to_s, nfs: true, linux__nfs_options: ["no_root_squash", "rw", "no_subtree_check"], mount_options: ['actimeo=1']
    else
      config.vm.synced_folder host_folder.to_s, guest_folder.to_s, nfs: true, mount_options: ['actimeo=1']
    end
  end

  install_docker_compose = "
    mkdir -p /opt/bin && \
    curl --progress-bar -o /opt/bin/docker-compose -L https://github.com/docker/compose/releases/download/1.5.2/docker-compose-`uname -s`-`uname -m` && \
    chmod +x /opt/bin/docker-compose
  "

  enable_rpcd = "
    mkdir -p /etc/conf.d && touch /etc/conf.d/nfs && \
    systemctl enable rpc-statd.service && \
    systemctl start rpc-statd.service
  "

  config.vm.provision :file, :source => "#{CLOUD_CONFIG_PATH}", :destination => "/tmp/vagrantfile-user-data"
  config.vm.provision :shell, :inline => "mv /tmp/vagrantfile-user-data /var/lib/coreos-vagrant/", :privileged => true
  config.vm.provision :shell, :inline => "coreos-cloudinit --from-file /var/lib/coreos-vagrant/vagrantfile-user-data", :privileged => true
  config.vm.provision :shell, :inline => install_docker_compose, :privileged => true
  config.vm.provision :shell, :inline => enable_rpcd, :privileged => true

  # Allows custom provision since each developer has their own preferences
  # for what should be automatically configured in the vm
  # (e.g. initial access folder and custom alias)
  if File.file?("custom-provision.sh")
    config.vm.provision "shell", path: "custom-provision.sh"
  end
end
