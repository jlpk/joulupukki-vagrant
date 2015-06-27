===================
Joulupukki Vagrant
===================

Deploy Joulupukki with Vagrant


Prerequisite
============

* Vagrant 1.7.2
* Virtualbox

Installation
============

::

    git clone https://github.com/jplk/joulupukki-vagrant
    cd joulupukki-vagrant
    vagrant up 
    vagrant ssh 

Access to tmuxs
===============

::

    tmux a -t web
    tmux a -t api
    tmux a -t dispatcher
    tmux a -t worker


This vagrant file use
=====================

    * using box: ubuntu/trusty64
    * grunt
    * virtualenv
    * pip
    * npm
    * git 
    * ruby
    * ruby-sass

Tests
=====


Contributing
============

