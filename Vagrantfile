# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = 'bento/ubuntu-16.04'
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |vb|
    vb.memory = 256
  end

  config.vm.define :bash do |app|
    app.vm.network :forwarded_port, guest: 80, host: 8081
    app.vm.provision :shell, path: 'provisioners/bash/install_apache2.sh'
  end

  config.vm.define :ansible do |app|
    app.vm.network :forwarded_port, guest: 80, host: 8082

    app.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioners/ansible/playbook.yml"
    end
  end

  config.vm.define :chef do |app|
    app.vm.network :forwarded_port, guest: 80, host: 8083
    app.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = 'provisioners/chef/cookbooks'
      chef.add_recipe 'apache2'
    end
  end

  config.vm.define :puppet do |app|
    app.vm.network :forwarded_port, guest: 80, host: 8084

    app.vm.provision :shell, :inline => <<-SHELL
      apt-get update
      apt-get install -y puppet
    SHELL

    app.vm.provision :puppet do |puppet|
      puppet.manifests_path = 'provisioners/puppet/manifests'
      puppet.manifest_file = 'apache2.pp'
    end
  end
end
