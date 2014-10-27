# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|


  # The vbguest and omnibus plugins should make other boxes possible.
  config.vm.box = "utopic64"

  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/utopic/current/utopic-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network :public_network
  config.vm.network :forwarded_port, guest: 3000, host: 3000

  config.berkshelf.enabled = true

  config.omnibus.chef_version = :latest

  # This VM config option is required in order to be able to create the mount --bind symlink to the sync folder
  config.vm.provider "virtualbox" do |v|
    v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/vagrant-root", "1"]
    v.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.provision "chef_solo", run: "always" do |chef|

    #chef.log_level = :debug

    chef.add_recipe "meteor_windows"

    chef.json = {
      :nodejs => {
        :install_method => "package",
        :npm => "1.3.23"
      },
      :meteor_windows => {
        :apps => [
          "orikami",
          "mozartadmin",
          "smell"
        ]
      }
    }
  end
end