---
layout: page
title: Quick Start
permalink: /quickstart/
---
# Foundational knowledge

Hedron relies on docker-compose for building out development environments. Docker uses the name of the containing folder to determing exactly how it is going to name containers within the cluster. In order to prevent conflicting container names, Hedron uses a client/project/branch paradigm for building out environments. If you had a client named "Foo" and a project named "Bar", then Hedron would end up maintaining appropriate directory structures for all of this as well as ensuring that docker specific directories were in the pattern of foo-bar-{git-branch-name}.

> You cannot start a new project for a new client without first creating an entry for the client.

## Quickstart

Assuming you have no clients created yet or that this is a new project for a new client:

{% highlight bash %}
hedron client:create
{% endhighlight %}

This command will prompt you for the name of a client to create.

{% highlight bash %}
hedron project:create
{% endhighlight %}

This command will prompt you for which client the project belongs to, a project name and type. Project types are currently freeform in nature, but Hedron has a plugin type for project types so eventually the selectable project should be limited to that list. For the moment, Hedron currently packages a Drupal 8 project type for demo purposes, so when prompted for project type type "drupal8".

The project create command finishes with a bit of useful information. Most importantly is a git clone command you can copy and paste in order to begin working on your project. In addition to this, Hedron makes available docker variables for your docker-compose.yml file for shared directories for web and mysql. A demo cluster designed to work with Hedron for Drupal 8 projects is publicly available at: [Hedron Drupal 8 Docker Cluster](https://github.com/HedronDev/DockerClusterDrupal8)

## Working on your new project

The first task is to clone your new project from its git repository.

{% highlight bash %}
git clone /Users/{user_name}/.hedron/repositories/foo/bar foo_bar
{% endhighlight %}

This is an empty repository, so you are literally starting a new project from zero. Hedron is focussed as a build-process tool. This means tools like composer are our default assumption for building out project artifacts. Since this is a Drupal example, let's build a composer.json file and specify which version of Drupal 8 we wish to install.

{% highlight json %}
{
  "require": {
    "drupal/drupal": "8.2.4"
  }
}
{% endhighlight %}

Before committing our work, let's define an initial cluster of docker containers for our Drupal to run on. Creating a docker directory within your git working directory gives you direct control over the cluster that your site is running on. Let's extract the example docker-compose demo into this directory.

{% highlight bash %}
curl -L https://github.com/HedronDev/DockerClusterDrupal8/archive/master.zip | bsdtar -xvf-
mv DockerClusterDrupal8-master docker
{% endhighlight %}

This should give you a docker directory within your git working directory and a composer.json from the previous step. This is all we need to get started so **ensure that docker is running and then**:

{% highlight bash %}
git add .
git commit -m 'some message'
git push
{% endhighlight%}

## Where the magic happens

With your first successful push, you should see way more than a normal git commit fly across your screen. As part of the git push, Hedron leverages a git post-receive hook and begins parsing the pushed files to determine what to do next. As part of this it will build the appropriate project out, downloading the correct version of Drupal 8, setup the appropriate directory structures, and instantiate our docker-compose container cluster and give us list of running containers at the end of the build. Find the container with the suffix "nginx_1" on it, check what port was mapped to port 80 (probably some number above 32000) and visit localhost:{your_port_number}. You should find a Drupal 8 site ready to be installed.

## Installing Drupal

The biggest point of interest here is that you will need to manually specify your mysql server to Drupal since it has a non-standard address. Within this setup the server name is literally "sql". The root password is setup as "abcdefg123ABCDEFG".

## New environments

Hedron can and will of course setup completely new environments for each project you create. However Hedron can also create new environments per branch of your repository. This is done through standard git branching practices.

{% highlight bash %}
git checkout -b new_branch_name
git push -u origin new_branch_name
{% endhighlight %}

This will result in a completely new cluster of containers being spun up to work against. You can make changes to your Drupal 8 by requiring new Drupal modules in your composer.json. You can add custom modules/themes/profiles by creating directories in the root named the same and adding your custom modules/themes/etc into them. You can directly edit and change aspects of the docker cluster, and Hedron will rebuild to your specifications with each push.
