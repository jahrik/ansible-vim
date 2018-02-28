# Managing vim with ansible

Vim is my favorite text editor and a tool that I use every day at work and at home.  I'm writing this text with vim.  If you are in the IT field, you can relate that a text editor is one of your most powerful tools.  Being comfortable and proficient in "{{ text_editor_of_your_choice }}" seams like a basic skill, but is a very valuable skill to have.  Getting used to the default install with base configs is not a bad idea, either.  As a sysadmin, you will most likely only be using the default settings accross all the systems you manage, but on your personal system you should go nuts and customize the hell out of it.

Ansible is my favorite tool for managing a local system.  I like chef for orchestrating larger infrastructure, but Ansible just makes more sense when it comes to configuring one or two local systems.  There's just way less setup involved.  If you are setting up Ansible for the first time, you'll want to start with their [install docs](http://docs.ansible.com/ansible/latest/intro_installation.html).  Once you've got it installed and are ready to go, you can follow along locally or on a VM.  I'll do so with a couple of vagrant boxes.


## Requirements
* [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html)
* [Vagrant](https://www.vagrantup.com/docs/installation/) - optional
  * I also have the following vagrant plugins installed:
    * vagrant-vbguest (0.15.1) # Automatically install guest additions
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads) - optional
* Tested on ubuntu, arch, and redhat linux

## Vagrant

I'm going to fire up a couple of virtual machines to test this using Vagrant and Virtualbox.  If you're already on a linux machine, chances are this playbook should work locally. I'm running arch locally and want to also test this playbook in ubuntu and fedora virtual machines.

**[Vagrantfile]()**

    ```
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
    ```

## Install vim 

**playbook.yml**

    ---
    - hosts: all
      tasks:
        - name: Install vim
          package:
            name: vim
            state: present
          tags:
            - vim


# ---
# - hosts: all
#   tasks:
#     - name: Install vim
#       package:
#         name: vim
#         state: present
#       tags: [ "vim" ]
# 
#     - name: create ~/.vim diretory
#       file:
#         path: "/home/{{ ansible_user }}/.vim/"
#         state: directory
#         owner: "{{ ansible_user }}"
#         group: "{{ ansible_user }}"
#         mode: 0755
#       tags: [ "vim" ]
# 
#     - name: Clone vundle from github
#       git:
#         repo: https://github.com/VundleVim/Vundle.vim.git
#         dest: "/home/{{ ansible_user }}/.vim/bundle/Vundle.vim"
#         version: master
#       tags: [ "vim" ]
# 
#     - name: Generate ~/.vimrc
#       template:
#         src: vimrc.j2
#         dest: "/home/{{ ansible_user }}/.vimrc"
#         owner: "{{ ansible_user }}"
#         group: "{{ ansible_user }}"
#         mode: "0644"
#       notify: install plugins
#       tags: [ "vim" ]
# 
#     - name: Touch spell-check file
#       file:
#         path: "/home/{{ ansible_user }}/.vim/en.utf-8.add"
#         owner: "{{ ansible_user }}"
#         group: "{{ ansible_user }}"
#         state: touch
#         mode: 0644
#       tags: [ "vim" ]
# 
#     - name: vim info
#       debug:
#         msg: "See ':h vundle' for more details."
#       tags: [ "vim" ]
