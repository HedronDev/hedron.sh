---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---
# Hedron
Hedron is an PHP and Docker-based tool for generating ops and development artifacts that work in tandem to give developers a local environment with the flexibility to match their production environment. Hedron leverages many of the tools PHP developers are already used to. Out of the box you'll find:

+ Familiar Symfony components
+ A Drupal 8-inspired plugin system rewritten from scratch
+ Tooling designed to respond to git push commands
+ On demand development environments per git branch
+ A helpful CLI tool
+ Plenty of room to contribute

### Requirements
Hedron has a short list of requirements most developers will already have installed.

+ zip
+ git
+ composer
+ php7+
+ Docker (and docker-compose)

### How Hedron Works
Hedron has a number of powerful features for controlling the workflow of a project, but at its heart, Hedron operates on top of git to watch what files have changed from one commit to another. This gives unprecidented control over the operations that can be performed because Hedron can actually inspect and react to the changing state of your project.

### Getting Started with HedronCLI
Hedron is broken up into two primary components. The first is HedronCLI which is a command line utility designed to help manage your Hedron projects as well as installing and updating Hedron itself. To get HedronCLI simply perform the following commands in your terminal.
{% highlight bash %}
curl http://hedron.sh/installer -o hedron.phar
mv hedron.phar /usr/local/bin/hedron
chmod a+x /usr/local/bin/hedron
{% endhighlight %}

### Installing Hedron Core
Once HedronCLI is in place, it has a simple command for installing Hedron's core.
{% highlight bash %}
hedron core:install
{% endhighlight %}

### Updating Hedron Core
If Hedron's core is already installed, you can use HedronCLI to update to the newest release of Hedron.
{% highlight bash %}
hedron core:update
{% endhighlight %}

> Note: HedronCLI does not yet have a self-update command.
