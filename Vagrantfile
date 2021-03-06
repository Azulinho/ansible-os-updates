# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  machines = [
    { 'name' => 'centos7-os-updates',
      'url' => 'https://github.com/holms/vagrant-centos7-box/releases/download/7.1.1503.001/CentOS-7.1.1503-x86_64-netboot.box',
      'ip' =>  '192.168.33.96' },
    { 'name' => 'suse12-os-updates',
      'url' => 'https://atlas.hashicorp.com/elastic/boxes/sles-12-x86_64/versions/0.1.1/providers/virtualbox.box',
      'ip' =>  '192.168.33.97' }]

  machines.each do |item|
      config.vm.define item['name'] do |machine|
        machine.vm.box = item['name']
        machine.vm.box_url = item['url']
        machine.vm.hostname = item['name']
        machine.vm.network "private_network", ip: item['ip']
        #machine.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: "venv"

        machine.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
          # https://github.com/hashicorp/otto/issues/423#issuecomment-186076403
          vb.linked_clone = true if Vagrant::VERSION =~ /^1.8/
          vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
          vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
          vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
          vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
          vb.customize ["storagectl",  :id, "--name", "IDE Controller", "--hostiocache", "on"]
        end
      end
  end

  config.vm.provision :shell, inline: 'rm -f /etc/udev/rules.d/70-persistent-net.rules'

  config.vm.provision :ansible do |ansible|
    ansible_raw_arguments = ''
    ansible.playbook = "site.yml"
    if defined? ENV['LIMIT']
      ansible.limit= ENV['LIMIT']
    else
      ansible.limit = "all"
    end
    ansible.sudo = true
    ansible.raw_arguments = ["-f", "5"]

    if defined? ENV['TAGS']
      ansible.tags = ENV['TAGS']
    end
    if defined? ENV['START_AT_TASK']
      ansible.start_at_task = ENV['START_AT_TASK']
    end

    ansible.raw_ssh_args= ["-o ControlMaster=no", "-o ControlPersist=no", "-o ControlPath=/dev/null"]
    ansible.inventory_path = "./inventory/vagrant"
    ansible.groups = {
      "centos7" => ["192.168.33.96"],
      "suse12" => ["192.168.33.97"]
    }
  end
end
