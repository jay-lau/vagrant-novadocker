Vagrant.require_version ">= 1.4.3"
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    numNodes = 3
    r = numNodes..1
    (r.first).downto(r.last).each do |i|
        config.vm.define "osnode#{i}" do |node|
            node.vm.box = "ubuntu"
            node.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"
            node.vm.provider "virtualbox" do |v|
                v.name = "osnode#{i}"
                v.customize ["modifyvm", :id, "--memory", "1536"]
                if i == 1
                    v.customize ["modifyvm", :id, "--memory", "2048"]
                end
            end
            if i < 10
                node.vm.network :private_network, ip: "10.211.65.10#{i}"
            else
                node.vm.network :private_network, ip: "10.211.65.1#{i}"
            end
            node.vm.hostname = "osnode#{i}"
            node.vm.provision "shell", path: "scripts/setup-ubuntu.sh"
            node.vm.provision "shell", path: "scripts/setup-ubuntu-ntp.sh"
			node.vm.provision "shell" do |s|
                s.path = "scripts/setup-ubuntu-hosts.sh"
                s.args = "-t #{numNodes}"
            end
            if i == 1
                node.vm.provision "shell", path: "scripts/setup-controller.sh"
            end
            if i != 1
                node.vm.provision "shell" do |s|
                    s.path = "scripts/setup-compute.sh"
                    s.args = "-m 10.211.65.101"
                end
			end
        end
	end
end
