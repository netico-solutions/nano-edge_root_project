# Nano-Edge Root project

This is a root project containing everything that you need to setup development 
environment for Nano-Edge devices.

Terminology
* Host machine is a machine which you are sitting at. It has a OS of your 
  choice, but I recommend a recent Debian, Ubuntu LTS, OpenSuse. Please, don't
  use some non-mainstream Linux distro. If you choose to use Windows then you
  have my sympathy.
* Development machine is a virtualized Ubuntu 16.04 OS which contains all the
  tools you need for development of system software for Nano-Edge.
* Target machine is actual Linux that runs on Nano-Edge

## Initial setup

1. On you HOST machine install VirtualBox virtualization software as described 
   here: https://www.virtualbox.org/wiki/Linux_Downloads

2. Install Vagrant using your distro package manager. More details are here:
   https://www.vagrantup.com

## Starting and stopping the Development machine

Navigate to project folder containing the Vagrant file. Execute:
    
    vagrant up

wait until it starts and then

    vagrant ssh

To stop the running development machine you can do:

    vagrant halt

Also, see Vagrant help for suspending and resuming the development machine.

