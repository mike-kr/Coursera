# Introduction to Automation Scale
## What is scale?
A scalable system is a flexible one.  
## Motivation for Automation at Scale
Infrastructure-as-code (IaC) - revolves around the creation of configuration files that describe how a machine should be provisioned and managed,  It's then combined with some automatic tooling that applies these configurations to the different nodes in your infrastructure.  
* Can use VCS
* Can run tests
* Repeatable
* Automatically deployable

## What is configuration management?
Configuration management system
:    tools that let an IT specialist or system administrator use all the benefits of IaC in one package
* automatic error-correction

It provides the format for you configuration files along with some kind of automation to deploy or apply them  
* [Chef](https://www.chef.io/)
* [Puppet](https://puppet.com/)
* [Ansible](https://www.ansible.com/)
* [CFEngine](https://cfengine.com/)

Cloud Environments
* [Amazon EC2](https://aws.amazon.com/ec2/)
* [Microsoft Azure](https://azure.microsoft.com/en-us/)
* [Google Cloud Platform](https://cloud.google.com/)

## Summary
IaC:
* Consistent
* Versionable
* Reliable 
* Repeatable

Configuration management system
:    Provides a platform to maintain and provision that infrastructure in an automated way
# Configuration Management Using Chef
## Intro to Chef
Configuration management (CM)
:    A system used to cinfigure and manage IT infrastructure, including the bare-metal, virtual, and cloud-based varieties  
* Recipes
* Cookbooks

Node
:    Some machine managed by a Chef server, which could be anything from a web server to a network router  
**Intent-based automation paradigm** - you tell Chef what it should be, not how to get there  
## Idempotency and Convergence
Idempotent - something that can be performed over and over again without changing the system once the action has been performed and with no unintended side effects  
Execute is not idempotent usually because they are typically unique to the environment they are run

Test and repair - only makes changes if necessary.  First tests to see if the resource being managed, like a file or package, actually needs to be modified  

Converging the system
:    The set of steps chef takes to move the system into the desired state  

* Chef-client - uses program called Ohai to gather information about a node and stores the information in *Attributes*  
[Ohai](https://docs.chef.io/ohai.html)  
## Chef and Ruby
Majority of chef written in Ruby while parts of the server written in Erlang  
Domain-specific language (DSL)
:    A specialized computer language that's used for a specific purpose, like declaring resources within a Chef recipe  
* [Google's Borgmon system](https://landing.google.com/sre/book/chapters/practical-alerting.html)
* [Gradle build tools](https://gradle.org/)
* [Facebook's spam fighting FXL](https://code.facebook.com/posts/460136060761165/fighting-spam-with-pure-functions/)
[Erlang](http://www.erlang.org/)
## Chef Configurations
Resources
:    Statements of configuration that describe the desired state of an item of configuration, like a file, software package, or even a script  
```ruby
type "name" do
    attribute "value"
    action :type_of_action
end
```
```ruby
package "apache2" do
    # make sure apache2 is installed
    action :install
end

service "apache2" do
    #make sure the http service automatically comes up if the node reboots and is started
    action [ :enable, :start ]
end
```
A resource generally has four components:
* type like package
* name like Apache 2
* one or more properties
* one or more actions like install

Some of these components are provided through default values like the package name for package resource so you don't have to explicitly write them out.  For example, the default action for the package resource is install e.g.
`package "apache2"`  
**Cookbooks** generally represent a set of configuration for a single infrastructure unit or scenario, like it's stated in the official Chef documentation.  
Chef Supermarket (like ruby gems)  
Test Kitchen - configured using .kitchen.yml
* Hyperbox  
* Vagrant
```
driver:
    name: vagrant

provisioner:
    name: chef_zero

platforms:
    - name: ubuntu-16.04

suites:
    - name: default
    run_list:[your_cookbook_here::default]
    attributes: 
```
kitchen create command  
kitchen destroy command  
chef-project/ > cookbooks/ > my-cookbook/ > kitchen.yml  
## Chef Local Mode
```
mkdir chef-code

file "/tmp/omelette" do
    content "eggs, cheese, and mushrooms"
end

chef-client --local-mode omelette_recipe.rb
less /tmp/omlette

file "/tmp/omlette"
    action :delete
end

chef-client --local-mode cleanup.rb
```
## Chef Ecosystem and Architecture
Chef Development Kit (Chef DK)  
Chef-client - install on node
Chef-server - hubs
Knife tool - the knife.rb file is loaded every time you use the knife too, and it's usually stored in the ~/.chef directory.
[Chef Knife tool](https://docs.chef.io/config_rb_knife.html)
## Summary
# Managing Nodes with Chef and Chef Server with an Example
## Overview
Goal
:    Install and configure the Apache2 server on a node to host a simple website  
* Planning
* Development
* Testing
* Deployment

## Planning
1. Install the apache2 program onto your machine.
2. Make sure the apache2 service is running.
3. Create an html file in the right place to represent the web page we want to serve.

* A package resource to install apache2 on our node
* A service to make sure that apache2 is running
* A template to represent the html page

```ruby
template "/var/www/html/index.html" do
    source 'index.html.erb'
end
```
## Creating the Configuration
`mkdir chef-project/ && mkdir chef-project/cookbooks && cd chef-project/cookbooks`  
Chef generate cookbook command
`chef generate cookbook apache2`  
`vim apache2/recipes/default.rb`  
```ruby
#
# Cookbook:: apache2
# Recipe:: default
#
# Copyright:: 2017, The Authors, All Rights Reserved.

apt_update 'update' do
    action :update
end

package 'apache2'

service 'apache2' do
    action [ :enable, :start ]
end

template '/var/www/html/index.html' do
    source 'index.html.erb'
end
```
`cd apache2`  
`chef generate template index.html.erb`  
`vim templates/index.html.erb`  
```html
<html>
    <body>
        <title>Hi! The name of this computer is <%= node['fqdn'] %></title>
    </body>
</html>
```
[chef generate cookbook](https://docs.chef.io/ctl_chef.html#chef-generate-cookbook)  
## Testing the Configuration
`kitchen create` - Start test env  
`kitchen list` - Status  
`kitchen converge` - start kitchen vm  
`kitchen exec` - send commands to test vm  
`kitchen exec -c "curl localhost"`  
`kitchen destroy`  
Automatic testing
* [ChefSpec](https://docs.chef.io/chefspec.html)
* [ServerSpec](https://serverspec.org/)  
## Deploying
knife command line tool is used to interact with the server  
* Key
* Certificate

Stored on the development workstation next to the knife.rb file in the .chef directory  
```
Server

knife cookbook upload apache2
knife cookbook list 
```
```
Client

knife ssh 'name:node1-ubuntu' --ssh-port 22 --ssh-user vagrant -ssh-password vagrant 'uptime' 
knife ssh 'name:*' --ssh-port 22 --ssh-user vagrant -ssh-password vagrant 'uptime'

knife ssh 'name:node1-ubuntu' -ssh-port 22 --ssh-user vagrant -ssh-password vagrant 'sudo chef-client'
```
Run list
:    ordered list of recipes that the Chef client will apply during a run

```
knife node run_list add node1-ubuntu 'recipe[apache2::default]'
knife ssh 'name:node1-ubuntu' -ssh-port 22 --ssh-user vagrant -ssh-password vagrant 'sudo chef-client'
curl node1-ubuntu
```
[Chef handles security for Chef Server](https://docs.chef.io/server_security.html)
## Scaling Chef
One way to scale - have Chef-Client pull from the Chef-Server periodically
## Extending Chef
Script resource - can execute arbitrary programming code using one of the many available interpreters, like Bash, Ruby, or Python  
Chef has the ability to add custom resources and modify tools in Chef DK
## Summary
First, we **planned** out what we needed to do to achieve that goal.  
Then, we jumped into the development phase to write the configuration to **exectue** our plan.  
To make sure it worked, we **tested** that configuration in a disposable sandbox environment.  
Then we deployed to node.  
[Learn Chef Rally and Chef Tutorials](https://learn.chef.io/)  
[Chef Documentation](https://docs.chef.io/)  
[Puppet self-paced learning](https://learn.puppet.com/category/self-paced-training)  
[Ansible get-started](https://www.ansible.com/resources/get-started)