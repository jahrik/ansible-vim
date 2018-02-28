# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = '2'.freeze
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #
  # Virtualbox configs
  config.vm.provider :virtualbox do |vb|
    vb.cpus = 2
    vb.gui = false
    vb.customize ['modifyvm', :id, '--memory', '2048']
    vb.customize ['modifyvm', :id, '--cpus', '2']
    vb.customize ['modifyvm', :id, '--cpuexecutioncap', '50']
  end
  config.ssh.forward_x11 = true
  config.vm.synced_folder '', '', disabled: true

  # Fedora box
  config.vm.define 'fedora' do |fedora|
    fedora.vm.hostname = 'fedora27'
    fedora.vm.box = 'bento/fedora-27'
    fedora.vm.provision 'shell', inline: 'dnf -y install python'
    fedora.vm.network 'public_network', ip: '192.168.33.11', bridge: %w[
      en0
      eth0
      enp0s31f6
      wlp4s0
    ]
  end

  # Ubuntu box
  config.vm.define 'ubuntu' do |ubuntu|
    ubuntu.vm.hostname = 'ubuntu16'
    ubuntu.vm.box = 'bento/ubuntu-16.04'
    ubuntu.vm.provision 'shell', inline: 'apt-get update && apt-get -y install python'
    ubuntu.vm.network 'public_network', ip: '192.168.33.12', bridge: %w[
      en0
      eth0
      enp0s31f6
      wlp4s0
    ]
  end
end
