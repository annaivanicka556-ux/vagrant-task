# -*- mode: ruby -*-
# vi: set ft=ruby :

# Shell script that installs Java 17 inside the VM
$install_deps = <<-'SHELL'
  apt-get update
  apt-get install -y openjdk-17-jdk
SHELL

Vagrant.configure("2") do |config|
  # Base box: README asks for ubuntu/jammy64, but it is x86-only.
  # On Apple Silicon (M1) I use the ARM64 box bento/ubuntu-24.04 instead.
  config.vm.box = "bento/ubuntu-24.04"


  # Private network: the VM is reachable from the Mac only, at a fixed IP
  config.vm.network "private_network", ip: "192.168.100.100"

  
  # VirtualBox resources: 2 CPUs and 4096 MB of RAM
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = 4096
  end


  # Provisioner: runs the install_deps script (installs openjdk-17-jdk)
  config.vm.provision "shell", inline: $install_deps


  # Trigger: after "vagrant up", start the Java app in the background
  config.trigger.after :up do |trigger|
    trigger.info = "Starting the Java application"
    trigger.run_remote = { inline: "cd /vagrant && nohup ./gradlew bootRun > /home/vagrant/app.log 2>&1 &" }
  end
end