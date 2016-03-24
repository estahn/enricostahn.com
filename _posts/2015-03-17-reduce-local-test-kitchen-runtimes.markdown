---
layout: post
title:  "Reduce local test kitchen runtimes"
date:   2015-03-17 09:00:00
categories: devops
tags:   chef kitchen devops
github_username: estahn
---

At [Zanui](http://www.zanui.com.au/) we've been working with Vagrant, Puppet and Chef for quite a while. We're happy with these tools, at least most of the time.

A major discomfort is the time taken to download any kind of assets during provisioning, e.g.

 - Omnibus: Chef Install
 - Apt update / packages
 - Pip packages
 - ...

While this is annoying it's usually not an issue for non-repetitive runs. These usually occur with a new team member or when a development box needs to be rebuild due to errors.

The opposite is the case for [test kitchen](https://github.com/test-kitchen/test-kitchen) runs. Testing your cookbook multiple times on multiple systems is crucial but a burden when it slows you down.

> <center><b>SquidMan to the rescue</b></center>
> <center><img src="/img/squidman.png"/></center>

[vagrant-cachier](https://github.com/fgrehm/vagrant-cachier) is the first idea that comes to mind to improve runtimes. Unfortunately kitchen [doesn't support](https://github.com/test-kitchen/kitchen-vagrant/pull/37) vagrant-cachier directly. Instead you need to create a Vagrantfile ERB template and eventually maintain it in your repository. Not pretty.

Another idea is to use an HTTP Proxy as an intermediate to store any kind of downloads over multiple kitchen runs. [Fletcher Nichol](https://gist.github.com/fnichol) created a nice little snippet that you just chuck into `$HOME/.kitchen/config.yml` then install a proxy such as [polipo](http://www.pps.univ-paris-diderot.fr/~jch/software/polipo/) and you're good to go.

I had mediocre success with polipo, even after building it from master. A more stable solution is [SquidMan](http://squidman.net/squidman/). It's a fancy interface on top Squid.
![SquidMan - Preferences - General](/img/squidman-preferences-general.png)
You want to bump up the *Cache size* and *Maximum object size* to maximum. And allow access to either your IP or an entire network range.
![SquidMan - Preferences - Clients](/img/squidman-preferences-clients.png)
