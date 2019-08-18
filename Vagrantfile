VAGRANTFILE_API_VERSION = "2"
VAGRANT_DISABLE_VBOXSYMLINKCREATE = "1"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
# Use same SSH key for each machine
config.ssh.insert_key = false
config.vm.box_check_update = true
config.vm.define "node1" do |node1|
  node1.vm.box = "centos/7"
  node1.vm.box_version = "1809.01"
#  node1.vm.hostname = "node1.test.example.com"
  node1.vm.network "private_network", ip: "192.168.55.61"
  node1.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
  node1.vm.provision :shell, :inline => "sudo yum install -y epel-release; sudo yum -y install sshpass python2 python-devel gcc; sudo curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py; python get-pip.py; sudo pip install -U pip;", run: "always"
  node1.vm.provision :shell, :inline => "pip install 'ansible==2.7.2.0'", run: "always"
  node1.vm.provider "virtualbox" do |node1|
    node1.memory = "1024"
  end

end

config.vm.define "node2" do |node2|
  node2.vm.box = "centos/7"
  node2.vm.box_version = "1809.01"
#  node2.vm.hostname = "node2.test.example.com"
node2.vm.network "private_network", ip: "192.168.55.62"
node2.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
node2.vm.provision :shell, :inline => "sudo yum install -y epel-release; sudo yum -y install sshpass python2 python-devel gcc; sudo curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py; python get-pip.py; sudo pip install -U pip;", run: "always"
node2.vm.provision :shell, :inline => "pip install 'ansible==2.7.2.0'", run: "always"
  node2.vm.provider "virtualbox" do |node2|
    node2.memory = "1024"
  end
end

#config.vm.define "repo" do |repo|
#  repo.vm.box = "centos/7"
##  repo.vm.hostname = "repo.test.example.com"
#  repo.vm.network "private_network", ip: "192.168.55.59"
#  repo.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
#  repo.vm.provision :shell, :inline => "sudo yum install -y epel-release; sudo yum -y install sshpass python2 python-devel gcc; sudo curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py; python get-pip.py; sudo pip install -U pip;", run: "always"
#  repo.vm.provision :shell, :inline => "pip install 'ansible==2.7.2.0'", run: "always"
#  repo.vm.synced_folder ".", "/vagrant"
#  repo.vm.provider "virtualbox" do |repo|
#    repo.memory = "1024"
#  end
#end

config.vm.define "control" do |control|
  control.vm.box = "centos/7"
  control.vm.box_version = "1809.01"
  control.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
  control.vm.provision :shell, :inline => "sudo yum install -y epel-release; sudo yum -y install python-devel vim gcc python-cryptography; sudo curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py; python get-pip.py; sudo pip install -U pip; yum install -y sshpass", run: "always"
  control.vm.provision :shell, :inline => "pip install --upgrade setuptools ;pip install pexpect ;pip install 'ansible==2.7.2.0'", run: "always"
#  control.vm.hostname = "control.test.example.com"
  control.vm.network "private_network", ip: "192.168.55.60"
  control.vm.provider :virtualbox do |control|
    control.customize ['modifyvm', :id,'--memory', '2048']
  end
  control.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: ".git/"
  control.vm.provision :ansible_local do |ansible|
    ansible.playbook = '/vagrant/playbooks/envplaybooks/master.yml'
    ansible.install = false
    ansible.compatibility_mode = "2.0"
    ansible.inventory_path = "/vagrant/inventory"
    ansible.config_file = "/vagrant/ansible.cfg"
    ansible.limit = "all"
end
end
end