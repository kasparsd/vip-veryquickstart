# Vagrantfile API/syntax version.
VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version '>= 1.5.0'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/trusty32"
  config.vm.hostname = 'vip.local'
  config.vm.network 'private_network', ip: '10.86.73.73'

  # Virtualbox overrides
  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpus", "1"]
    v.customize ["modifyvm", :id, "--memory", "1024"]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.synced_folder ".", "/srv", owner: 'www-data', group: 'www-data', mount_options: ["dmode=775", "fmode=775"]

  # Provision everything we need with Puppet
  config.vm.provision :puppet do |puppet|
    puppet.module_path = "puppet/modules"
    puppet.manifests_path = "puppet/manifests"
    puppet.manifest_file = "init.pp"
    puppet.options = ['--templatedir', '/srv/puppet/files']
    puppet.facter = {
      "quickstart_domain" => 'vip.local',
    }
  end

end
