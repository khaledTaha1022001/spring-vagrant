# -*- mode: ruby -*-
# vi: set ft=ruby :
$script = <<-SCRIPT
sudo apt-get update
sudo apt-get upgrade
wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.deb
sudo apt-get -qqy install ./jdk-19_linux-x64_bin.deb
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-19/bin/java 1919
sudo apt-get update
sudo apt-get upgrade
sudo echo deb http://nginx.org/packages/mainline/ubuntu/ CODENAME nginx >> /etc/apt/sources.list
sudo wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
sudo apt install unzip zip
curl -s https://get.sdkman.io | bash
source "/root/.sdkman/bin/sdkman-init.sh"
sdk install springboot
sdk install gradle 7.5.1
wget https://mirrors.estointernet.in/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
tar -xvf apache-maven-3.6.3-bin.tar.gz
mv apache-maven-3.6.3 /opt/
sudo ln -s /opt/apache-maven-3.6.3/bin/mvn /bin/
cd /home/vagrant/
sudo apt install -y git
mkdir -p ~/.ssh
chmod 700 ~/.ssh
ssh-keyscan -H github.com >> ~/.ssh/known_hosts
ssh -T git@github.com
git clone --branch=khaled git@github.com:khaledTaha1022001/QuizPlus.git
cd QuizPlus
sudo mvn package
sudo mvn spring-boot:run

SCRIPT



$postgres = <<-SCRIPT
sudo apt install wget ca-certificates
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
sudo apt update
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql.service
sudo -u postgres psql -d postgres -c "create table crud(id bigint primary key,age integer , first_name varchar(64), last_name varchar(64), occupation varchar(64));"

sudo -u postgres psql -d postgres -c "ALTER USER postgres WITH PASSWORD 'postgres';"
sudo chmod 777 /etc/postgresql/9.5/main/pg_hba.conf

sudo echo "host    all             all              0.0.0.0/0                 md5
host    all             all              ::/0                            md5" >> /etc/postgresql/9.5/main/pg_hba.conf

sudo vi /etc/postgresql/9.5/main/pg_hba.conf

sudo chmod 777 /etc/postgresql/9.5/main/postgresql.conf
echo "listen_addresses = '*'" >> /etc/postgresql/9.5/main/postgresql.conf

sudo ufw allow 5432 
sudo /etc/init.d/postgresql restart
SCRIPT


# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.

  config.vm.define "postgres" do |db|
    db.vm.box = "ubuntu/xenial64"
    db.vm.provision "shell", inline: $postgres
    db.vm.hostname="postgres.db"
    db.vm.network "private_network", ip: "192.168.0.20",hostname: true
    db.vm.network "public_network"	  
  end


 
 config.vm.define "spring" do |spring|
    spring.vm.box = "ubuntu/xenial64"
    spring.ssh.forward_agent = true
    spring.vm.provision "shell", inline: $script
    spring.vm.hostname = "spring"
    spring.vm.network "private_network", ip: "192.168.0.21"
    spring.ssh.forward_agent = true
    spring.vm.network "public_network"	
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
    config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

end
