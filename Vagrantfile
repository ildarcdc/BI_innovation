Vagrant.configure("2") do |config|
  config.vm.box = "eurolinux-vagrant/oracle-linux-8"
  config.vm.box_version = "8.10.5"
  config.vm.disk :disk, size: "100GB", primary: true
  config.vm.box_check_update = false
  config.vm.synced_folder ".", "/shared",
    owner: "root", group: "root"
  
  # Create etcd01 serveer
  config.vm.define "etcd01", primary: true do |etcd|
    etcd.vm.network "public_network", ip: "192.168.1.100"
    etcd.vm.provider "virtualbox" do |vb|
      
      vb.cpus = "4"
      vb.memory = "8192"
      vb.name = "etcd01"
	  end
    etcd.vm.provision "shell", inline: <<-SHELL
      hostnamectl set-hostname etcd01
	  touch /etc/sysconfig/network-scripts/route-enp0s8
	  echo -e "default via 192.168.1.1 dev enp0s8 \n192.168.1.0/24 via 192.168.1.1 dev enp0s8 " > /etc/sysconfig/network-scripts/route-enp0s8
      systemctl stop firewalld
      systemctl disable firewalld
	  sed -c -i "s/\SELINUX=.*/SELINUX=disabled/" /etc/sysconfig/selinux
      sudo yum install -y python3
	  adduser ansible
      echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo EDITOR='tee -a' visudo
      su - ansible
	  mkdir /home/ansible/.ssh
	  timedatectl set-timezone Asia/Almaty
	  reboot
    SHELL
  end
  
  # Create pgsql01 server
  config.vm.define "pgsql01", primary: true do |pgsql|
    pgsql.vm.network "forwarded_port", guest: 5432, host: 5432
	pgsql.vm.network "forwarded_port", guest: 8080, guest_ip: "192.168.1.101", host: 8080, host_ip: "127.0.0.1"
	pgsql.vm.network "public_network", ip: "192.168.1.101"
    pgsql.vm.provider "virtualbox" do |vb|
      
      vb.cpus = "4"
      vb.memory = "8192"
      vb.name = "pgsql01"
	  end
    pgsql.vm.provision "shell", inline: <<-SHELL
      hostnamectl set-hostname pgsql01
	  touch /etc/sysconfig/network-scripts/route-enp0s8
	  echo -e "default via 192.168.1.1 dev enp0s8 \n192.168.1.0/24 via 192.168.1.1 dev enp0s8 " > /etc/sysconfig/network-scripts/route-enp0s8
      systemctl stop firewalld
      systemctl disable firewalld
	  sed -c -i "s/\SELINUX=.*/SELINUX=disabled/" /etc/sysconfig/selinux
      sudo yum install -y python3
	  adduser ansible
      echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo EDITOR='tee -a' visudo
      su - ansible
	  mkdir /home/ansible/.ssh
	  dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm
	  timedatectl set-timezone Asia/Almaty
	  reboot
      SHELL
  end	
  
  # Create pgsql02 serveer
  config.vm.define "pgsql02", primary: true do |pgsql|
    pgsql.vm.network "forwarded_port", guest: 5432, host: 6432
    pgsql.vm.network "forwarded_port", guest: 8080, guest_ip: "192.168.1.102", host: 8081, host_ip: "127.0.0.1"
    pgsql.vm.network "public_network", ip: "192.168.1.102"
    pgsql.vm.provider "virtualbox" do |vb|
      
      vb.cpus = "4"
      vb.memory = "8192"
      vb.name = "pgsql02"
	  end
    pgsql.vm.provision "shell", inline: <<-SHELL
      hostnamectl set-hostname pgsql02
	  touch /etc/sysconfig/network-scripts/route-enp0s8
	  echo -e "default via 192.168.1.1 dev enp0s8 \n192.168.1.0/24 via 192.168.1.1 dev enp0s8 " > /etc/sysconfig/network-scripts/route-enp0s8
      systemctl stop firewalld
      systemctl disable firewalld
	  sed -c -i "s/\SELINUX=.*/SELINUX=disabled/" /etc/sysconfig/selinux
      sudo yum install -y python3
	  adduser ansible
      echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo EDITOR='tee -a' visudo
      su - ansible
	  mkdir /home/ansible/.ssh
	  dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm
	  timedatectl set-timezone Asia/Almaty
	  reboot
      SHELL
  end
  
  # Create docker01 server
  config.vm.define "docker01", primary: true do |docker|
    docker.vm.network "forwarded_port", guest: 9090, host: 9090
    docker.vm.network "forwarded_port", guest: 3000, host: 3000
	docker.vm.network "public_network", ip: "192.168.1.103"
	docker.vm.provider "virtualbox" do |vb|
	  vb.gui = true
	  vb.cpus = "4"
	  vb.memory = "8192"
	  vb.name = "docker01"
	  end
	docker.vm.provision "shell", inline: <<-SHELL
	  hostnamectl set-hostname docker01
	  touch /etc/sysconfig/network-scripts/route-enp0s8
	  echo -e "default via 192.168.1.1 dev enp0s8 \n192.168.1.0/24 via 192.168.1.1 dev enp0s8 " > /etc/sysconfig/network-scripts/route-enp0s8
	  systemctl restart NetworkManager
	  systemctl stop firewalld
      sed -c -i "s/\SELINUX=.*/SELINUX=disabled/" /etc/sysconfig/selinux
      dnf install -y epel-release
	  dnf install ansible -y
	  ansible --version
	  ansible-playbook -i localhost /shared/docker_installation.yml
	  adduser ansible
      echo 'ansible ALL=(ALL) NOPASSWD:ALL' | sudo EDITOR='tee -a' visudo
      su - ansible
	  mkdir /home/ansible/.ssh
	  yes | cp /shared/hosts /etc/ansible/hosts
	  yes | cp /shared/ansible.cfg /etc/ansible/ansible.cfg
	  export ANSIBLE_HOST_KEY_CHECKING=False
	  ansible-playbook /shared/ssh_playbook.yml
	  sed -i "s%ansible_user=root%ansible_user=ansible%g" "/etc/ansible/hosts"
	  ansible-playbook /shared/docker_install_hosts.yml
	  timedatectl set-timezone Asia/Almaty
	  ansible-playbook /shared/playbook.yml
      docker compose -f /shared/docker-compose.yml up -d
	  SHELL
  end
end

