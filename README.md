lancsgrid_vac
=========

An Ansible role that installs and configures vac on SL/RHEL/CentOS 6.x .

Current version: v0.19.0.  If the tag in master differs from the version of this readme, then the readme is out of date.  Please email me to encourage me to update it.

Version description:  This is a stable, tested version.  It will configure vac with a static configuration - in furture releases I hope this will be more customisable through variables.  Until then there are only a few variables, which are listed below.

Requirements
------------

So far only tested on an SL6 machine.

Role Variables
--------------

There are three variables used in this role. They all need defining.
`vac_gateway_ip` - this is the ip address or range that is permitted to log into the server.  
`vac_vm_cert` and `vac_vm_key` are the location of the host_cert.pem and host_key.pem that need copying to the server.


Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: username.vac}

License
-------

GPL v3

Author Information
------------------

Created by Robin Long (GridPP)
