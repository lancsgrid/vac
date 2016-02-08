lancsgrid_vac
------------

An Ansible role that installs and configures vac on SL/RHEL/CentOS 6.x .

Current version: v0.19.2.  If the tag in master differs from the version of this readme, then the readme is out of date.  Please email me to encourage me to update it.

Version description:  This is a stable, tested version. It allows the setting of all variables in vac.

Requirements
------------

So far only tested on an SL6 machine.

Role Variables - Generic
--------------

These are variables specific to the vac role:
`vac_gateway_ip` - this is the ip address or range that is permitted to log into the server.  
`vac_vm_cert` and `vac_vm_key` are the location of the host_cert.pem and host_key.pem that need copying to the server.

The config file for vac is built using the jinja2 templating engine. The variables in the template come in 2 forms, those with default values, and those with out. The purpose of the variables can be found here: https://www.gridpp.ac.uk/vac/vac.conf.5.html.   The variables all have the same name as their counter parts in vac except that they are prefixed with "vac_".   The variables with defaults are:

`vac_udp_timeout_seconds: 5.0`

`vac_domain_type: kvm`

`vac_delete_old_files: True`

`vac_cpu_per_machine: 1`

`vac_mb_per_cpu: 2048`

`vac_hs06_per_cpu: 1`

`vac_version_logger: True`

`vac_total_machines: "{{ ansible_processor_vcpus }}"`

`vac_cpu_total: "{{ ansible_processor_vcpus }}"`

`vac_overload_per_cpu: 2.0`

`vac_volume_group: vac_volume_group`

`vac_scratch_gb: 40`

`vac_fix_networking: True`


There are four generic variables that lack defaults in vac, so they have no been given in this role either. They are: `vac_vac_space, vac_forward_dev, vac_vacmon_hostport, vac_shutdown_time, vac_gocdb_sitename`.  vac_vac_space and vac_gocdb_sitename need to be set as they are needed by vac but lack default values.

Role Variables - vmtype
--------------

The final set of variables for vac are those for the vms you wish to run.  These should be specified as a list of dictionaries.  Once list item for each vmtype, with a dictionary containing all the varaiables you wish to set, or modify from the default.  A list of these varaiables can be found in the vac documentation.  An example is listed below in the example playbook.


Dependencies
------------

None.

Example Playbook
----------------

    - hosts: vac-servers
      roles:
         - { role: root}
      vars: 
        vac_gateway_ip: 192.30.252.130
	vac_gateway_ip: 148.88.67.50
	vac_vm_cert: group_files/vac/vm-cert.pem

	# Generic settings for vac
	vac_hs06_per_cpu: 10
	vac_vac_space: pygrid-vac.hec.lancs.ac.uk
	vac_scratch_gb: 25
	vac_volume_group: VolGroup
	vac_gocdb_sitename: UKI-NORTHGRID-LANCS-HEP
	vac_total_machines: 6

	# Settings for vm types (note lac of vac_prefix, these follow vac names exactly.)
	vmtypes:
	  - atlas:
	       heartbeat_file: vm-heartbeat
	       heartbeat_seconds: 600
     	       root_image: https://repo.gridpp.ac.uk/vacproject/atlas/cernvm3.iso
     	       root_public_key: /root/.ssh/id_rsa.pub
     	       user_data: https://repo.gridpp.ac.uk/vacproject/atlas/user_data
     	       user_data_option_queue: UKI-NORTHGRID-LANCS-HEP_VAC
     	       user_data_option_cvmfs_proxy: http://squid.server.ac.uk:3128
     	       user_data_file_hostcert: /etc/vac.d/vm-cert.pem
               user_data_file_hostkey: /etc/vac.d/vm-key.pem
               user_data_option_default_se: dpm.server.ac.uk
               user_data_option_http_logs: http://%f/
               backoff_seconds: 600
               fizzle_seconds: 600
               max_wallclock_seconds: 172800
               log_machineoutputs: True
               accounting_fqan: /atlas/Role=NULL/Capability=NULL
               target_share: 98
	  - lhcb:
               heartbeat_file: vm-heartbeat
               heartbeat_seconds: 600
               root_image: https://lhcbproject.web.cern.ch/lhcbproject/Operations/VM/cernvm3.iso
               root_public_key: /root/.ssh/id_rsa.pub
               user_data: https://lhcbproject.web.cern.ch/lhcbproject/Operations/VM/user_data
               user_data_option_dirac_site: VAC.Site.uk
               user_data_option_cvmfs_proxy: http://squid.server.ac.uk:3128
               user_data_file_hostcert: /etc/vac.d/vm-cert.pem
               user_data_file_hostkey: /etc/vac.d/vm-key.pem
               backoff_seconds: 600
               fizzle_seconds: 600
               max_wallclock_seconds: 172800
               log_machineoutputs: True
               accounting_fqan: /lhcb/Role=NULL/Capability=NULL
               target_share: 1
          - gridpp:
               heartbeat_file: vm-heartbeat
               heartbeat_seconds: 600
               root_image: https://repo.gridpp.ac.uk/vacproject/gridpp/cernvm3.iso
               root_public_key: /root/.ssh/id_rsa.pub
               user_data: https://repo.gridpp.ac.uk/vacproject/gridpp/user_data
               user_data_option_dirac_site: VAC.Site.uk
               user_data_option_submit_pool: gridppPool
               user_data_option_cvmfs_proxy: http://squid.server.ac.uk:3128
               user_data_file_hostcert: /etc/vac.d/vm-cert.pem
               user_data_file_hostkey: /etc/vac.d/vm-key.pem
               backoff_seconds: 600
               fizzle_seconds: 600
               max_wallclock_seconds: 172800
               log_machineoutputs: True
               accounting_fqan: /gridpp/Role=NULL/Capability=NULL
               target_share: 1


License
-------

GPL v3

Author Information
------------------

Created by Robin Long (GridPP)
