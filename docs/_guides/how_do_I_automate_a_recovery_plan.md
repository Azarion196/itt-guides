---
layout: guide
date:   2018-01-03 21:00:39 +0200
title:  How do I automate a recovery plan
categories: tech
status: beta
---

# Use case

The first recovery plan are usually cumbersome with lots of manual steps. A good recovery plan is highly automated, to enable quick restoration of the system.

When considering this, you have already made the recovery guide. If no, start [here](creating_recovery_plans.html) for the guidelines.

# Automating 

Your guide will have some distinct parts, depending on which baseline you have decided upon.
1. Setting up host and hypervisor.
2. create and setup virtual machines in the hypervisor environment
3. orchestrate the virtual machines

## setting up host and hypervisor

1. assume that you have a functioning debian host

2. create a script to automate installation of hypervisor.

    Go [here](https://gist.github.com/moozer/831ee13acec17dfc9a35d32caafe067f/raw/96dbe927a6186295d2cc9bdfd3b9d15d53e65916/install_vmware.sh) for a complete install script of vmware workstation on Debian

    It includes whatever patcehs are need to make vmware work.
    
3. set up vmnets
   
   Look into the commandline tools called `vmware-networks`

references: 
* [official vmware documentation](https://docs.vmware.com/en/VMware-Workstation-Pro/index.html)

# create and setup virtual machines in the hypervisor environment

(untested)

1. Have the base files

    Your VMware workstation VMs reside in VMX files. There might be an import for ova files.
    
2. clone the machines

    copy the machines from backup to where you want them
    
    Perhaps `vmrun clone`is relevant
    
    
3. use `vmrun`

    The tool `vmrun`has many option. See `vmrun --help`. 
    
    It is able to start, stop and lots of other things.
    
    
# orchestrate the virtual machines

1. log into the machine

2. create script to run on the VMs

    a) linux: Use a script

    The script includes the commands that you otherwise need to type manually,

    `wget` on is useful.

        ```
        #!/bin/sh

        apt-get install apache
        wget config
        cp to somewhere

        cd /var/www/http
        git clone website

        service restart apache
        ```

    b) Junos: `load override`

    You default image will not have much in terms of config.

    A sequence like

        ```
        edit
        load override scp://<serverip>/config
        commit
        ```

    This could work well with `archive-sites`

3. Use `expect` in bash or paramiko to execute the scripts

    A bit on [Expect](https://likegeeks.com/expect-command/)
    
    A bit on [paramiko](https://likegeeks.com/expect-command/)

# Remarks

* This is guide work-in-progress
