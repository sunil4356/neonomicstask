Vagrant.configure("2") do |config|
  
  config.vm.define "neonomics-master" do |master|
	master.vm.box = "centos/7"
	master.vm.hostname = "neonomics-master"
	master.vm.network "private_network", ip: "172.28.128.100"
	master.vm.provider :virtualbox do |vb|
		vb.name = "neonomics-master"
		vb.memory = 2048
		vb.cpus = 2
		end
	end
  
  NodeCount = 1

  # neonomics-client Node
  (1..NodeCount).each do |i|
    config.vm.define "neonomics-client#{i}" do |node|
      node.vm.box = "centos/7"
      node.vm.hostname = "neonomics-client#{i}"
      node.vm.network "private_network", ip: "172.28.128.10#{i}"
      node.vm.provider "virtualbox" do |v|
        v.name = "neonomics-client#{i}"
        v.memory = 1024
        v.cpus = 1
        # Prevent VirtualBox from interfering with host audio stack
        v.customize ["modifyvm", :id, "--audio", "none"]
      end
    end
  end
  config.vm.provision "shell", inline: <<-SHELL
		yum install -y yum-utils java
		yum -y install epel-release
		yum -y install ansible
		ansible --version
		yum install -y wget git vim maven
		sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
		sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
		yum install jenkins -y
		sudo systemctl start jenkins
		yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
		yum install docker-ce docker-ce-cli containerd.io -y
		sudo systemctl start docker
  SHELL
end
