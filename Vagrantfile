# -*- mode: ruby -*-
# vi: set ft=ruby :

builds = {
  'zookeeper1.com' => {
    id: "11",
    ip: "192.168.10.25",
    role: "zookeeper",
    memory: "512",
    deployId: "1",
    hostname: "zookeeper1"
  },
  'zookeeper2.com' => {
    id: "12",
    ip: "192.168.10.26",
    role: "zookeeper",
    memory: "512",
    deployId: "2",
    hostname: "zookeeper2"
  },
  'zookeeper3.com' => {
    id: "13",
    ip: "192.168.10.27",
    role: "zookeeper",
    memory: "512",
    deployId: "3",
    hostname: "zookeeper3"
  },
  'kafka1.com' => {
    id: "21",
    ip: "192.168.10.35",
    role: "kafka",
    memory: "512",
    deployId: "1",
    hostname: "kafka1"
  },
  'kafka2.com' => {
    id: "22",
    ip: "192.168.10.36",
    role: "kafka",
    memory: "512",
    deployId: "2",
    hostname: "kafka2"
  },
  'kafka3.com' => {
    id: "23",
    ip: "192.168.10.37",
    role: "kafka",
    memory: "512",
    deployId: "3",
    hostname: "kafka3"
  },
  'kafkamisc.com' => {
    ip: "192.168.10.51",
    role: "kafkamisc",
    memory: "512",
    hostname: "kafkamisc"
  },
  'kafkamonitor.com' => {
    ip: "192.168.10.52",
    role: "kafkamonitor",
    memory: "2048",
    hostname: "kafkamonitor"
  }
}

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
  config.vm.box = "centos/7"
  config.vm.synced_folder ".", "/vagrant"
  config.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"

  builds.each_pair do |name, opts|

    config.vm.define name.to_sym do |guest|

      guest.vm.network "private_network", ip: opts[:ip]

      guest.vm.provider "virtualbox" do |vb|
        vb.memory = opts[:memory]
      end

      case opts[:role]
      when "kafka"
        guest.vm.provision :shell, :path => "deployKafka.sh", :args => "#{opts[:deployId]}  #{opts[:hostname]}"
      when "zookeeper"
        guest.vm.provision :shell, :path => "deployZookeeper.sh", :args => "#{opts[:deployId]}  #{opts[:hostname]}"
      when "kafkamisc"
        guest.vm.provision :shell, :path => "deployMisc.sh", :args => "#{opts[:ip]}  #{opts[:hostname]}"
      when "kafkamonitor"
        guest.vm.provision :shell, :path => "deployMonitor.sh", :args => "#{opts[:ip]} #{opts[:id]} #{opts[:hostname]}"
      end

    end
  end
end
