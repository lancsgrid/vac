lancsgrid_vac

An Ansible role that installs and configures vac on SL/RHEL/CentOS 6.x .

Current version: v0.19.1.  If the tag in master differs from the version of this readme, then the readme is out of date.  Please email me to encourage me to update it.

Version description:  This is a stable, tested version.  This is the first version to really intoduce variables to vac's ansible install.  More will be added as I dive further into the documentation, but at the current commit only variables that we declare already are customisable from ansible.

Requirements
------------

So far only tested on an SL6 machine.

Role Variables
--------------

These are variables specific to the vac role:
`vac_gateway_ip` - this is the ip address or range that is permitted to log into the server.  
`vac_vm_cert` and `vac_vm_key` are the location of the host_cert.pem and host_key.pem that need copying to the server.

The following variables all releate to variables in vac, so it is recommended that you consult the vac documentation for their meanings.  All of the variables modify a parameter in vac.conf, and are listed with their defaults.  Variable names are the same as in vac.conf - they are prefixed with either vac_ (if they are generic) or vac_<experiment> (when they are specicif to a vmtype.

Generic defaults for vac.conf:
vac_mb_per_machine: 2048
vac_total_machines: 6
vac_scratch_gb: 25
vac_volume_group: VolGroup

Generic vm type variables.
vac_vm_model: cernvm3
vac_heartbeat_file: vm-heartbeat
vac_heartbeat_seconds: 600
vac_root_public_key: /root/.ssh/id_rsa.pub
vac_user_data_option_cvmfs_proxy: http://pygrid-kraken.hec.lancs.ac.uk:3128
vac_user_data_file_hostcert: /etc/vac.d/vm-cert.pem
vac_user_data_file_hostkey: /etc/vac.d/vm-key.pem
vac_backoff_seconds: 600
vac_fizzle_seconds: 600
vac_max_wallclock_seconds: 172800
vac_log_machineoutputs: True
vac_target_share: 1


Atlas vm type variables. Unless these override a variable listed in the generic vm type, they inherit from those.
# defaults for [vmtype atlas]
vac_atlas_root_image: https://repo.gridpp.ac.uk/vacproject/atlas/cernvm3.iso
vac_atlas_user_data: https://repo.gridpp.ac.uk/vacproject/atlas/user_data
vac_atlas_user_data_option_queue: UKI-NORTHGRID-LANCS-HEP_VAC
vac_atlas_user_data_option_default_se: fal-pygrid-30.lancs.ac.uk
vac_atlas_user_data_option_http_logs: http://%f/
vac_atlas_accounting_fqan: /atlas/Role=NULL/Capability=NULL

LHCb vm type variables. Unless these override a variable listed in the generic vm type, they inherit from those.
# defaults for [vmtype lhcb]
vac_lhcb_root_image: https://lhcbproject.web.cern.ch/lhcbproject/Operations/VM/cernvm3.iso
vac_lhcb_user_data: https://lhcbproject.web.cern.ch/lhcbproject/Operations/VM/user_data
vac_lhcb_user_data_option_dirac_site: VAC.Lancaster.uk
vac_lhcb_accounting_fqan: /lhcb/Role=NULL/Capability=NULL

GridPP vm type variables. Unless these override a variable listed in the generic vm type, they inherit from those.
# defaults for [vmtype gridpp]
vac_gridpp_root_image: https://repo.gridpp.ac.uk/vacproject/gridpp/cernvm3.iso
vac_gridpp_user_data: https://repo.gridpp.ac.uk/vacproject/gridpp/user_data
vac_gridpp_user_data_option_dirac_site: VAC.Lancaster.uk
vac_gridpp_user_data_option_submit_pool: gridppPool
vac_gridpp_accounting_fqan: /gridpp/Role=NULL/Capability=NULL

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
