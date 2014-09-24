require 'json'

cluster = JSON.parse(IO.read(File.join("virtual_machines.json")))

Vagrant.configure("2") do |config|

  cluster.each do |hostname, info|

    config.vm.define hostname do |cfg|

      cfg.vm.provider :virtualbox do |vb, override|
        override.vm.box = "trusty64"
        override.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
        override.vm.network :private_network, ip: "#{info["ip"]}"
        override.vm.hostname = hostname
        vb.name = hostname
        vb.customize ["modifyvm", :id, 
                      "--memory", info["config"]["mem"],
                      "--cpus", info["config"]["cpus"], 
                      "--hwvirtex", "on" ] 
      end

      cfg.vm.provision :ansible do |ansible|
        ansible.verbose = "v"
        ansible.inventory_path = "ansible/inventory"
        ansible.playbook = "ansible/#{info["playbook"]}"
      end
    end
  end
end
