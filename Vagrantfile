# -*- mode: ruby -*-
# vi: set ft=ruby :

# Ensure a minimum Vagrant version to prevent potential issues.
Vagrant.require_version '>= 1.5.0'

# Configure using Vagrant's version 2 API/syntax.
Vagrant.configure(2) do |config|
  # Ubuntu 14.04, 64 bit
  config.vm.box         = 'ubuntu/trusty64'
  config.vm.box_version = '~> 14.04'

  config.vm.synced_folder '.', '/vagrant', disabled: true

  # Providers
  config.vm.provider :virtualbox do |v|
    v.customize ['modifyvm', :id, '--memory', '512', '--ioapic', 'on']
  end

  # SSH
  config.ssh.username = 'vagrant'

  # Port Forwarding
  config.vm.network :forwarded_port, guest: 8080,  host: 8080
  config.vm.network :forwarded_port, guest: 28015, host: 28015
  config.vm.network :forwarded_port, guest: 29015, host: 29015

  # Provisioning

  # Provisioning RethinkDB
  config.vm.provision "install-rethinkdb", type: "shell" do |sh|
      sh.inline = <<-EOF

        # Install RethinkDB
        source /etc/lsb-release && echo "deb http://download.rethinkdb.com/apt $DISTRIB_CODENAME main" | sudo tee /etc/apt/sources.list.d/rethinkdb.list
        wget -qO- http://download.rethinkdb.com/apt/pubkey.gpg | sudo apt-key add -
        sudo apt-get --assume-yes update
        sudo apt-get install --assume-yes rethinkdb;

      EOF
  end

  # Provisioning RethinkDB
  config.vm.provision "run-rethinkdb", type: "shell" do |sh|
      sh.inline = <<-EOF

        # Creating the default.conf for RethinkDB so it can start as a service
        sed -e 's/# bind=127.0.0.1/bind=all/g' /etc/rethinkdb/default.conf.sample > /etc/rethinkdb/instances.d/default.conf;

        # Start RethinkDB
        service rethinkdb start;
      EOF
  end

  # Provisioning Git
  config.vm.provision "install-git", type: "shell" do |sh|
    sh.inline = <<-EOF

      # Install Git
      sudo apt-get -y install git;

    EOF
  end

  # Provisioning Gearz
    config.vm.provision "install-gearz", type: "shell" do |sh|
      sh.inline = <<-EOF

        git clone https://github.com/gearz-lab/gearz.git;
        cd gearz;
        npm install;

      EOF
    end

end