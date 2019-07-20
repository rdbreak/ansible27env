VAGRANTFILE_API_VERSION = "2"
VAGRANT_DISABLE_VBOXSYMLINKCREATE = "1"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
# Use same SSH key for each machine
config.ssh.insert_key = false
config.vm.box_check_update = true
config.vm.define "control" do |control|
  control.vm.box = "Rendanic/oraclelinux-7.5"
  control.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
  control.vm.provision :shell, :inline => "sudo yum install -y epel-release; sudo yum -y install python2; sudo curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py; python get-pip.py; sudo pip install -U pip; pip install pexpect;", run: "always"
  control.vm.hostname = "control.node.example.com"
  control.vm.network "private_network", ip: "192.168.55.80"
  control.vm.provider :virtualbox do |control|
    control.customize ['modifyvm', :id,'--memory', '2048']
  end
end
  
config.vm.define "node1" do |node1|
  node1.vm.box = "Rendanic/oraclelinux-7.5"
  node1.vm.hostname = "ans1.node.example.com"
  node1.vm.network "private_network", ip: "192.168.55.81"
  node1.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
  node1.vm.provision :shell, :inline => "sudo yum install -y epel-release; sudo yum -y install python2; sudo curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py; python get-pip.py; sudo pip install -U pip;", run: "always"

  node1.vm.provider "virtualbox" do |node1|
    node1.memory = "1024"
  end

end

config.vm.define "node2" do |node2|
  node2.vm.box = "Rendanic/oraclelinux-7.5"

  node2.vm.hostname = "ans2.node.example.com"
  node2.vm.network "private_network", ip: "192.168.55.82"
  node2.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
  node2.vm.provision :shell, :inline => "sudo yum install -y epel-release; sudo yum -y install python2; sudo curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py; python get-pip.py; sudo pip install -U pip;", run: "always"

  node2.vm.provider "virtualbox" do |node2|
    node2.memory = "1024"
  end
  node2.vm.provision "ansible" do |ansible|
    ansible.version = "latest"
    ansible.compatibility_mode = "2.0"
    ansible.limit = "all"
    ansible.playbook = 'playbooks/envplaybooks/master.yml'
  end
end
end
ansible 
vagrant 
automated 
ansible-playbook 
deployment 
virtualbox 
python