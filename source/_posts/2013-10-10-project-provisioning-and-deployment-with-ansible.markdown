---
layout: post
title: "Project provisioning and deployment with Ansible"
date: 2013-10-10 18:44:30 +1100
comments: false
categories: [ansible, deployment]
---
deployment is so easy with today's tools that there is simply no excuse...
-------

I had been meaning to start using a real configuration management tool and
leave my Fabric scripts behind for a while. The problem for me was that tools
like [Puppet](http://puppetlabs.com/) or [Chef](http://www.opscode.com/chef/)
seemed like overkill in most of my use cases.

When I first learned about [Salt](http://www.saltstack.com/), the fact that was
based on **Python** was a big plus and motivated me to give it a closer look
but still all the overhead needed for simple projects seemed too much (I
don't manage _thousands_ of servers...)

Enter [Ansible](http://www.ansibleworks.com/) - it let's you automate
everything without having to install anything on your server at the beginning -
just **SSH** will do! Later as the project scales you can make your
provisioning and deployment as complex as needed but getting started is easy.
No big setup at all for simple projects.

<!-- more -->

Ansible revolves around the concept of an **Inventory**, that includes all the
machines that will be managed and **Playbooks**, the list of states that
describe the final state of the servers in the inventory.

I found this [excellent article](http://www.stavros.io/posts/example-provisioning-and-deployment-ansible/)
about using Ansible on Django projects, even including a special setup for
local VMs like [Vagrant](http://www.vagrantup.com/) - it even allows for
different virtualenv requirements.

You basically just specify a couple of yaml files for Ansible to work with and
you are done:

* **hosts** - The inventory file for Ansible, it specifies the ip addresses,
  nicknames, groups and other server designations.
* **vars.yaml** - Includes the project name, target location, packages to be
  installed, etc.
* **provision.yaml** - This playbook that setups a brand new server (install
  base packages, etc.)
* **deploy.yaml** - As the name implies this playbook performs the actual code
  deployment (pull code, update packages, run migrations, etc.)
* **handlers.yaml** - This file specifies the actions to run if one of the
  previous playbooks makes certain changes, for example if the nginx
  configuration changes, the handler will be notified and nginx will be
  restarted.

Just for illustration purposes I am copying here one of the yaml files from the blog
post - **_vars.yaml_**

{% codeblock lang:yaml vars.yaml %}
---
project_name: myproject
project_root: /var/projects/myproject
project_repo: git@bitbucket.org:myuser/myproject.git
system_packages:
  - build-essential
  - git
  - libevent-dev
  - nginx
  - postgresql
  - postgresql-server-dev-all
  - python-dev
  - python-setuptools
  - redis-server
  - postfix
python_packages:
  - pip
  - virtualenv
initfiles:
  - gunicorn
{% endcodeblock %}

I will stop here but there's more detail on the blog post and much more on
Ansible's website, there are hundreds of module beyond apt, pip and friends,
and if you don't find what you need you can extend the system using Python...
