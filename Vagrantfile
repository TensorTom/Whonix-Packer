config.vm.box = "digital_ocean"
config.ssh.private_key_path = "~/.ssh/id_rsa"
config.vm.provider :digital_ocean do |provider|
    provider.client_id = "vagrant"
    provider.api_key = "c1df45a7933d9728c216c1e6c8e49955aeeb173fac4b5492060a01ca6bdfa81e"
    provider.image = "Ubuntu 12.10 x64"
    provider.region = "New York 2"
end

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    config.vm.define "gateway" do |gw|
      gw.vm.box = 'whonix-gateway'
      gw.ssh.username = 'user'
      gw.vm.provider 'virtualbox' do |vb|
        vb.customize ['modifyvm', :id, '--memory', 196]
        vb.gui = false
      end
  
      gw.vm.network "private_network", auto_config: false,
        virtualbox__intnet: "Whonix", adapter: 2
  
      gw.vm.network "forwarded_port", guest: 22, host: 2200, 
        id: "ssh", disabled: true
      gw.vm.network "forwarded_port", guest: 22, host: 2299
    end
  
    config.vm.define "workstation" do |ws|
      ws.vm.box = 'whonix-workstation'
      ws.ssh.username = 'user'
      ws.ssh.host = '10.152.152.11'
      ws.ssh.proxy_command = 'ssh user@127.0.0.1 -p 2299 -o '\
        'StrictHostKeyChecking=no -i ' + File.join(Dir.pwd(), 
          ".vagrant/machines/gateway/virtualbox/private_key") + 
         ' nc %h %p'
      ws.vm.provider 'virtualbox' do |vb|
        vb.gui = false
      end
      ws.vm.boot_timeout = 60
  
      ws.vm.network "private_network", ip: "10.152.152.11", 
        virtualbox__intnet: "Whonix", adapter: 1
      ws.vm.network "forwarded_port", guest: 22, host: 2200,
        id: "ssh", disabled: true
  
      ws.ssh.forward_x11 = true
      ws.ssh.forward_env = ["DISPLAY"]
    end
  
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vbguest.auto_update = false
  end