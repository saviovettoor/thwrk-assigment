$install_docker_on_nodes = <<SCRIPT
yum install -y yum-utils device-mapper-persistent-data lvm2 ;
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo ;
yum update -y ;
yum install docker-ce -y ;
systemctl enable docker && systemctl start docker ;
usermod -aG docker vagrant;
curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
SCRIPT

Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
	config.vm.box_check_update = true
	config.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__auto: true, mount_options: ["dmode=777,fmode=777"],
    rsync__exclude: ".git/"
	
	config.vm.define "swarmnode1" do |master|
		master.vm.hostname = "swarmmasternode"
		master.vm.network "private_network", ip: "10.0.8.20"
		master.vm.provision "shell", inline: $install_docker_on_nodes
		master.vm.provider :virtualbox do |ps|
		  ps.memory=2048
		end
		master.vm.provision "shell", inline: "docker swarm init --advertise-addr 10.0.8.20 --listen-addr 10.0.8.20:2377"
        master.vm.provision "shell", inline: "docker swarm join-token -q worker > /vagrant/token"
		master.vm.provision "file", source: "docker-compose.yaml", destination: "/opt/docker-compose.yaml"
	end
	
	config.vm.define "swarmnode2" do |worker|
		worker.vm.hostname = "swarmworkernode"
		worker.vm.network "private_network", ip: "10.0.8.21"
		worker.vm.provision "shell", inline: $install_docker_on_nodes
		worker.vm.provider :virtualbox do |ps|
		  ps.memory=2048
		end
		worker.vm.provision "shell", inline: "docker swarm join --advertise-addr 10.0.8.20 --listen-addr 10.0.8.20:2377 --token `cat /vagrant/token` 10.0.8.20:2377"
		worker.vm.provision "file", source: "docker-compose.yaml", destination: "/opt/docker-compose.yaml"
		worker.vm.provision "shell", inline: "/usr/local/bin/docker-compose up &"
		
	end
end
