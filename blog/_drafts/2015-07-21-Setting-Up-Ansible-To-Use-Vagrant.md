---
layout: post
title:  "Setting Up Ansible to Use Vagrant"
date:   2015-04-07 17:48:40
categories: ansible vagrant devops provisioning
---
<br/>

###Overview
[Ansible][ansible-homempage] is an awesome tool that alows you to write very maintainable playbooks to automate the provisioning of 
your infrastructure.

That being said, it is not so simple to test out your automations using traditional methods. Destroying a server,
bringing it back up, and running your ansible playbooks can take a lot of time.

Enter [Vagrant][vagrant-homempage]! The awesome virtualization tool that we will be able to use to test our automations easily and 
quickly. Vagrant allows us to reproduce our infrastructure (even complex ones) relatively easily on our local machine.
The only real limitation is the amount of resources you have available for use.

This quick guide will go over the process of setting up Ansible and Vagrant so they can be used together to test out your
playbooks.

###What we will be using
For this guide we will be using:

* [Ansible][ansible-homepage]
* [Vagrant][vagrant-homempage]
* [Debian Linux][debian-homempage]

###Philosophy
In general when testing out provisioning of any infrastrucure it is best to have the environment match the environment you
deploy to as closely as possible. This includes how you would run the provisioner of your choice, be it Chef, Ansible, Puppet, etc.

This brings me to how you use Ansible with Vagrant. Ansible has a great integration with vagrant via the [ansible provisioner][vagrant-ansible-provisioner] 
that is great for quickly testing out simple use cases and getting up and running as fast as possible. In my opinion though, this falls short in a few areas.
Namely that this takes away the experience of building your hosts file and running ansible via the command line like you would when 
provisioning remote infrastructure.

Because of this, this guide will treat the two systems independently, as if the VM's running via Vagrant were remote servers somewhere.
In order to do this though, we will need to do a bit of setup.

###Preparing Vagrant

First thing we will need to do is setup our Vagrantfile so that we can just run a quick `vagrant up` and the virtual machines
we need will be brought up for us to test our Ansible scripts against. Below is an example for a Vagrant file that will setup
everything for us:

{% highlight ruby linenos %}
# -*- mode: ruby -*-
# vi: set ft=rubyh:
require_relative 'key_authorization'

Vagrant.configure(2) do |config|
  config.vm.box = "deb8"
  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  authorize_key_for_root(config, '~/.ssh/id_rsa.pub')

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  {
    'web1'   => { ip: '192.168.33.110', ssh_port: 2200, forward_80: 8080 },
    'db1'    => { ip: '192.168.33.111', ssh_port: 2201, forward_80: 8081 },
    'app1'   => { ip: '192.168.33.112', ssh_port: 2202, forward_80: 8082 },
    'redis1' => { ip: '192.168.33.113', ssh_port: 2203, forward_80: 8083 },
  }.each do |short_name, args|
    config.vm.define short_name do |host|
      host.vm.network :private_network, ip: args[:ip]
      host.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh", disabled: true
      host.vm.network :forwarded_port, guest: 22, host: args[:ssh_port], auto_correct: true
      host.vm.network :forwarded_port, guest: 80, host: args[:forward_80]
      host.vm.hostname = "#{short_name}.ansible.dev"
    end
  end
end
{% endhighlight %}

This does a few things for us:

* `config.vm.box = "deb8"` - sets our VM's to use Debian linux
* `config.ssh.insert_key = false` tells Vagrant not to create an unsecure SSH key

[ansible-homempage]: http://www.ansible.com/home
[vagrant-homepage]: https://www.vagrantup.com
[vagrant-ansible-provisioner]: http://docs.vagrantup.com/v2/provisioning/ansible.html
[debian-homepage]: https://www.debian.org/
