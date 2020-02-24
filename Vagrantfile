VAGRANTFILE_API_VERSION = "2"
VAGRANT_DISABLE_VBOXSYMLINKCREATE = "1"
file_to_disk1 = './disk-0-1.vdi'
file_to_disk2 = './disk-0-2.vdi'
file_to_disk3 = './disk-0-3.vdi'
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
# Use same SSH key for each machine
config.ssh.insert_key = false
config.vm.box_check_update = false

# Repo Server
config.vm.define "repo" do |repo|
  repo.vm.box = "rdbreak/ansible27repo"
#  repo.vm.hostname = "repo.ansi.example.com"
  repo.vm.network "private_network", ip: "192.168.55.59"
  repo.vm.provider "virtualbox" do |repo|
    repo.memory = "512"
  end
end

# Node 1
config.vm.define "node1" do |node1|
  node1.vm.box = "rdbreak/ansible27node"
#  node1.vm.hostname = "node1.ansi.example.com"
  node1.vm.network "private_network", ip: "192.168.55.61"
  node1.vm.provider "virtualbox" do |node1|
    node1.memory = "512"
  
    unless File.exist?(file_to_disk1)
      node1.customize ['createhd', '--filename', file_to_disk1, '--variant', 'Fixed', '--size', 2 * 1024]
      node1.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
      node1.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk1]
      end
    end
      node1.vm.provision "shell", inline: <<-SHELL
      yes| sudo mkfs.ext4 /dev/sdb
      SHELL
      node1.vm.synced_folder ".", "/vagrant"
end


# Node 2
config.vm.define "node2" do |node2|
  node2.vm.box = "rdbreak/ansible27node"
#  node2.vm.hostname = "node2.ansi.example.com"
node2.vm.network "private_network", ip: "192.168.55.62"
  node2.vm.provider "virtualbox" do |node2|
    node2.memory = "512"
  
   unless File.exist?(file_to_disk2)
     node2.customize ['createhd', '--filename', file_to_disk2, '--variant', 'Fixed', '--size', 2 * 1024]
     node2.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
     node2.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk2]
     end
  end
     node2.vm.provision "shell", inline: <<-SHELL
     yes| sudo mkfs.ext4 /dev/sdb
     SHELL
     node2.vm.synced_folder ".", "/vagrant"
end


# Node 3
config.vm.define "node3" do |node3|
  node3.vm.box = "rdbreak/ansible27node"
#  node3.vm.hostname = "node3.ansi.example.com"
node3.vm.network "private_network", ip: "192.168.55.63"
  node3.vm.provider "virtualbox" do |node3|
    node3.memory = "512"
  
   unless File.exist?(file_to_disk3)
     node3.customize ['createhd', '--filename', file_to_disk3, '--variant', 'Fixed', '--size', 2 * 1024]
     node3.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
     node3.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk3]
     end
    end
     node3.vm.provision "shell", inline: <<-SHELL
     yes| sudo mkfs.ext4 /dev/sdb
     SHELL
     node3.vm.synced_folder ".", "/vagrant"
end

# Control Node
config.vm.define "control" do |control|
  control.vm.box = "rdbreak/ansible27node"
#  control.vm.hostname = "control.ansi.example.com"
  control.vm.network "private_network", ip: "192.168.55.60"
  control.vm.provider :virtualbox do |control|
    control.customize ['modifyvm', :id,'--memory', '2048']
  end
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