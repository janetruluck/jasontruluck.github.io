---
layout: post
title:  "Ansible Playbooks for Rails"
description: "Set of Ansible playbooks to deploy a Ruby on Rails web application."
github: "https://github.com/jasontruluck/ansiblize-rails"
date:   2014-03-27 00:00:00
categories: ruby rails ansible server
---

[Ansible](https://github.com/ansible/ansible) playbooks for provisioning Ubunutu servers for rails applications. This
is build with capistrano in mind, it sets up sudoers file for sudo-less management
required by [Capistrano 3](https://github.com/capistrano/capistrano).

##Current Playbooks:
* common - common tasks for ubuntu
  * update packages (via apt-get)
  * monit
  * iptables
  * fail2ban
  * basic ssh configuration
* appservers - provisions application servers
  * Unicorn
* webservers - provisions web servers
  * Nginx
* dbservers - provisions database server
  * Postgresql
* bgservers - provisions background job servers
  * Resque
* redisservers - provisions redis servers
  * Redis
* pubsubservers - provisions PubSub servers
  * Faye