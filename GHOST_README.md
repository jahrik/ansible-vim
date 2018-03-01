# Creating and testing an ansible role with molecule.

Vim is my favorite text editor and a tool that I use every day at work and at home.  If you are in the IT field, you can relate that a text editor is one of your most powerful tools.  Being comfortable and proficient in "{{ text_editor_of_your_choice }}" seams like a basic skill, but is a very valuable skill to have.  Getting used to the default install with base configs is a good idea.  As a sysadmin, you will most likely only be using the default settings across all the systems you manage, but on your personal system and workstation you should go nuts and customize the hell out of it to give yourself as much ease of use as possible. I like a lot of customization to vim when I set it up on a new workstation and install vundle to handle plugins.  I end up with a customized .vimrc file and a bunch of other things and if I had to do this setup manually, it would take a few hours at least.  Half of that time Googling how I did something before.  With tools like ansible, you can easily recreate the customization you prefer across all of your systems, be that at work or your homelab and save yourself hours of frustration and repetition.

Ansible is a very reliable tool for managing your local workstation.  A good place to start automating, is at the base of your every day setup.  Text editor, user, groups, base packages, and every day tools inevitably installed anytime you get a new system or accidentally lose or kill your old one.  If you are setting up Ansible for the first time, you'll want to start with their [install docs](http://docs.ansible.com/ansible/latest/intro_installation.html).  Once you've got it installed and are ready to go, you can follow along locally or on a VM.  My initial plan was to do this with a couple of vagrant boxes in my traditional way of testing new things by creating a Vagrantfile and going from there, but after getting started in writing this and also considering test kitchen I've decided to check out [molecule](https://molecule.readthedocs.io/en/latest/index.html).

This write-up is intended to:
* walk through the creation of a new ansible role
* write and test it with molecule
* upload it to ansible galaxy for use in a later playbook

## Requirements

* Python2.7
* [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html)
* [Molecule](https://molecule.readthedocs.io/en/latest/installation.html)
* [Docker](https://docs.docker.com/install/)
* Tested on arch, ubuntu-16.04, and fedora-27

## Molecule

If you decide to set up a virtualenv and install molecule that way, you can do the following.  If you need an intro on python virtualenv you can find it [here](https://homelab.business/python-virualenv-the-why-and-how/).

Create a virtual environment for molecule and install it.

    mkvirtualenv -p /usr/bin/python2.7 ansible
    pip install docker-py molecule

## Role

A role will allow for easy reuse of tasks.  I want this role to work across multiple operating systems and be very customizable with the use of variables.

At the most basic level, if all you needed was a playbook to install vim on arch, ubuntu, and fedora, it would look like this.  Very simple and uses the package module, which will act as a wrapper for many other ansible modules including: apt, yum, dnf, and pacman.

**playbook.yml**

    ---
    - hosts: all
      tasks:
        - name: Install vim
          package:
            name: vim
            state: present

### Molecule

To initialize a new role with molecule, you would run the following.

    # -r role
    # -d driver

    molecule init role -r vim -d docker                                                                            

    --> Initializing new role vim...
    Initialized role in /home/wgill/vim successfully.

But I had already created this role with `ansible-galaxy init` and had to initialize molecule this way

    # From role root dir

    molecule init scenario -r vim -d docker

    --> Initializing new scenario default...
    Initialized scenario in /home/wgill/vim/molecule/default successfully.

With molecule initialized you should see a directory structure close to this.

    ├── defaults
    │   └── main.yml
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── molecule
    │   └── default
    │       ├── create.yml
    │       ├── destroy.yml
    │       ├── Dockerfile.j2
    │       ├── INSTALL.rst
    │       ├── molecule.yml
    │       ├── playbook.yml
    │       ├── prepare.yml
    │       └── tests
    │           ├── test_default.py
    │           └── test_default.pyc
    ├── README.md
    ├── tasks
    │   └── main.yml
    └── vars
        └── main.yml

Set the containers you will be testing this on in `./molecule/default/molecule.yml`.  For me, this was defaulting to image: centos:7, so I just updated that to be the images I want to test against: ubuntu and fedora.

**./molecule/default/molecule.yml**

    ---
    dependency:
      name: galaxy
    driver:
      name: docker
    lint:
      name: yamllint
    platforms:
      - name: fedora-27
        image: fedora:27
      - name: ubuntu-16.04
        image: ubuntu:16.04
    provisioner:
      name: ansible
      lint:
        name: ansible-lint
    scenario:
      name: default
    verifier:
      name: testinfra
      lint:
        name: flake8

### Vim

Copy your original playbook to `./tasks/main.yml` without the `- hosts: all` or `tasks:` lines.

**./tasks/main.yml**

    ---
    - name: Install vim
      package:
        name: vim
        state: present

With that, it's ready to go!  Test it by running `molecule test`. There is a lot more to the output below, but these are the steps it takes.

    molecule test

    --> Test matrix

    └── default
        ├── lint
        ├── destroy
        ├── dependency
        ├── syntax
        ├── create
        ├── prepare
        ├── converge
        ├── idempotence
        ├── side_effect
        ├── verify
        └── destroy

### Vundle

Vim should be successfully passing tests and installing in both environments by now.
