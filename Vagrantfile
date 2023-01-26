Vagrant.configure("2") do |config|
    servers=[
        {
          :hostname => "Main",
          :box => "bento/ubuntu-20.04",
          :ip => "172.16.1.50",
          :ssh_port => '2200'
        },
        {
          :hostname => "Node1",
          :box => "bento/ubuntu-20.04",
          :ip => "172.16.1.51",
          :ssh_port => '2201'
        },
        {
         :hostname => "Node2",
         :box => "bento/ubuntu-20.04",
         :ip => "172.16.1.52",
         :ssh_port => '2202'
        },
        {
          :hostname => "Minikub",
          :box => "bento/ubuntu-20.04",
          :ip => "172.16.1.53",
          :ssh_port => '2203'
        }
      ]

    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            if machine[:hostname] == 'Main' 
    		        node.vm.hostname = 'Main'
    		    else
    		        node.vm.hostname = machine[:hostname]
    		    end
            
            node.vm.network :private_network, ip: machine[:ip]
            node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"
            node.vm.synced_folder "./data", "/home/vagrant/data"
            # node.vm.synced_folder "./html", "/var"
            node.vm.provision "file", source: "./copiedfile.txt", destination: "/home/vagrant/copiedfile.txt"

            node.vm.provider :virtualbox do |vb|
            	if machine[:hostname] == 'Minikub' 
    			        vb.customize ["modifyvm", :id, "--memory", 3072]
                    	vb.customize ["modifyvm", :id, "--cpus", 2]
    			    else
    			        vb.customize ["modifyvm", :id, "--memory", 512]
                    	vb.customize ["modifyvm", :id, "--cpus", 1]
    			    end
            end
        end
    end
end
