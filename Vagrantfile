VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network "forwarded_port", guest: 8080, host: 8080 
  config.vm.network "forwarded_port", guest: 9990, host: 9990
  config.vm.network "forwarded_port", guest: 5432, host: 5432

  config.vm.define 'jboss-01' do |server|
    server.vm.hostname = "jboss-01.localdomain"
    server.vm.network "private_network", ip: "10.0.3.20"
    config.vm.provider :virtualbox do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
  end

  config.vm.synced_folder ".", "/vagrant", :mount_options => ["dmode=777","fmode=777"]

#  config.vm.provision "ansible" do |ansible|
#    ansible.playbook = "postgresql.yml"
#    # ansible.tags = "debug"
#  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo add-apt-repository ppa:openjdk-r/ppa -y
    sudo apt-get update

    sudo apt-get -y install openjdk-8-jdk
    sudo update-ca-certificates -f
    sudo add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -sc)-pgdg main"
    sudo wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    sudo apt-get update
    sudo apt-get install postgresql-9.6

    curl -s -OL http://ftp.unicamp.br/pub/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
    sudo tar -zxf ./apache-maven-3.6.3-bin.tar.gz -C /usr/local/
    sudo ln -s /usr/local/apache-maven-3.6.3/bin/mvn /usr/bin/mvn

    groupadd jboss
    useradd -s /bin/bash -g jboss -d /opt/jboss jboss
    wget -q https://download.jboss.org/wildfly/11.0.0.Final/wildfly-11.0.0.Final.zip
    mkdir /opt/jboss
  SHELL
end
