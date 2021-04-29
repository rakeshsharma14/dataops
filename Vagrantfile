
IMAGE_NAME = "hashicorp/bionic64"
N = 3

Vagrant.configure("2") do |config|

	config.vm.provider "virtualbox" do |vb|
		vb.memory = 1024
		vb.cpus = 2
	end

	config.vm.define "master" do |mstr|
		mstr.vm.box = IMAGE_NAME
		mstr.vm.hostname = "master"
		mstr.vm.network "private_network", ip: "192.168.2.200"
		
		# example provisioning
		mstr.vm.provision "shell", type: "shell", preserve_order: true, inline: "echo 'hello master'"
		mstr.vm.provision "B", type: "shell",  after: "shell", inline:<<-SHELL
			echo \" start of installations \"
			sudo apt-get update -y
			sudo apt-get install python3-pip -y
			pip3 install pandas -v
			echo \" end of installations \"
		SHELL
	end

	# setting the 3 clients
	(1..N).each do |i|
		config.vm.define "client-#{i}" do |clnt|
			clnt.vm.box = IMAGE_NAME
			clnt.vm.hostname = "client-#{i}"
			clnt.vm.network "private_network",  ip: "192.168.2.#{200 + i}"
			clnt.vm.provision "shell", type: "shell", preserve_order: true,  inline: "echo 'hello client-#{i}'"
		end
	end
end
		
