---
layout: post
title: "Creating custom Vagrant boxes with Veewee"
date: 2013-11-05 16:48:18 +1100
comments: false
categories: [vagrant, veewee, debian, virtualbox] 
---
Easily build Vagrant boxes for your favourite distro...
-------------------------------------------------------

I have been using [Vagrant](http://vagrantup.com/) for a while to quickly and
simply generate reproducible development environments. It's very useful and
works great.

Vagrant provides several different [official](https://github.com/mitchellh/vagrant/wiki/Available-Vagrant-Boxes)
"base boxes" that you can use to [get started quickly](http://docs.vagrantup.com/v2/getting-started/index.html).
However, all of them are different versions of Ubuntu - 32 and 64 bits versions
of the supported LTS editions (Precise and Lucid at the moment).

I have been thinking about switching to Debian for my base server needs for a
while, given all the controversy surrounding Ubuntu lately. It seems that
Canonical is making a lot of one-sided decisions, and it is unclear what this
strategy will mean for the future of their server offerings.
See [here](http://www.omgubuntu.co.uk/2013/10/ubuntu-wins-big-brother-austria-privacy-award)
and [here](http://www.networkworld.com/news/2013/062013-canonical-mir-271072.html)
for some background.

There are several distributions like Fedora or CentOS that would have been a
good alternative as well, I have played with them in the past without issues.
However, I am most comfortable using tools like apt, dpkg, etc. so I am coming
back to Debian.

<!-- more -->

There are no official Debian Vagrant boxes though. You can definitely get some
in various unofficial [places](http://www.vagrantbox.es/) - but I am not very
comfortable with using OS images from random people on the web. So it looks like
it's time to roll up our sleeves and build our own, but it does not sound like a
very productive proposition.

Enter [Veewee](https://github.com/jedi4ever/veewee), a tool to repeatably and
quickly build Vagrant boxes. It has a collection of
[templates](https://github.com/jedi4ever/veewee/tree/master/templates) for many
distributions, including Debian Wheezy which I used successfully to build my
Debian image for Virtualbox. The base template did everything that I needed, and
Veewee would even download the basic netboot Debian image to kickstart the
build.

You can find the simple sequence of steps to a working Virtualbox images using
Veewee in [this](http://www.ducea.com/2011/08/15/building-vagrant-boxes-with-veewee/) or
[this](http://stacktoheap.com/blog/2013/06/19/building-a-debian-wheezy-vagrant-box-using-veewee/)
blog post. But this is a quick summary:

{% codeblock lang:bash %}
veewee vbox templates # list available templates
veewee vbox define 'debian-72' 'Debian-7.2.0-amd64-netboot'
veewee vbox build 'debian-72'
veewee vbox export 'debian-72'
vagrant box add 'debian-72' debian-72.box # add the new box to Vagrant
vagrant init 'debian-72' # create a new VM from the image
{% endcodeblock %}
