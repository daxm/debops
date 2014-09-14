
## [![DebOps project](http://debops.org/images/debops-small.png)](http://debops.org) apt



[![Travis CI](http://img.shields.io/travis/debops/ansible-apt.svg?style=flat)](http://travis-ci.org/debops/ansible-apt) [![test-suite](http://img.shields.io/badge/test--suite-ansible--apt-blue.svg?style=flat)](https://github.com/debops/test-suite/tree/master/ansible-apt/)  [![Ansible Galaxy](http://img.shields.io/badge/galaxy-debops.apt-660198.svg?style=flat)](https://galaxy.ansible.com/list#/roles/1551) [![Platforms](http://img.shields.io/badge/platforms-debian%20|%20ubuntu-lightgrey.svg?style=flat)](#)






`debops.apt` configures and manages APT package manager in Debian and other
derivative distributions. Specifically, it will manage:

* list of APT sources
* centralized APT cache (using `apt-cacher-ng`)
* automatic package updates (using `unattended-upgrades` and `apticron`)
* Debian Preseed configuration
* local APT repository (using `reprepro`)
* installation of custom packages specified in Ansible inventory





### Installation

This role requires at least Ansible `v1.7.0`. To install it, run:

    ansible-galaxy install debops.apt

#### Are you using this as a standalone role without DebOps?

You may need to include missing roles from the [DebOps common
playbook](https://github.com/debops/debops-playbooks/blob/master/playbooks/common.yml)
into your playbook.

[Try DebOps now](https://github.com/debops/debops) for a complete solution to run your Debian-based infrastructure.





### Role dependencies

- `debops.secret`
- `debops.apt_preferences`
- `debops.etc_services`
- `debops.ferm`
- `debops.nginx`
- `debops.reprepro`





### Role variables

List of default variables available in the inventory:

    ---
    
    # Manage APT package manager (set to False to disable)
    # Set to FQDN of a host to specify it as host for
    #    apt-cacher-ng - apt-get cache
    #    Debian Preseed configuration (planned)
    #    reprepro - Debian local repositories
    apt: True
    
    # Default distribution used by APT
    apt_codename: '{{ ansible_lsb.codename }}'
    apt_codename_backports: '{{ apt_codename }}-backports'
    
    # Send mail notifications from APT to these email addresses
    apt_mail_notifications: [ 'root' ]
    
    
    # ---- Update notifications ----
    
    # Enable mail notifications about updates (apticron and apt-listchanges)
    apt_update_notifications: True
    
    # What type of notifications to send (choices: news, changelogs, both)
    apt_update_notifications_type_listchanges: 'news'
    apt_update_notifications_type_apticron: 'both'
    
    # Send only information about changes from the last notification?
    apt_update_notifications_diff: True
    
    
    # ---- unattended-upgrades ----
    
    # Enable unattended-upgrades
    apt_unattended_upgrades: True
    
    # Do not automatically upgrade these packages
    apt_unattended_upgrades_blacklist: [ 'vim', 'libc6' ]
    
    # If True, send mail notifications about all performed automatic upgrades
    # If False, send mail notifications only on errors
    apt_unattended_upgrades_notify_always: True
    
    # Automatically remove unused dependencies
    apt_unattended_upgrades_autoremove: True
    
    
    # ---- Other ----
    
    apt_debian_http_mirror: 'cdn.debian.net'
    
    apt_acng_port: 3142
    apt_acng_login: 'admin'
    apt_acng_password: 'password'
    
    # Allow access to apt-cacher-ng service from specified IP addresses or CIDR networks.
    # If not specified, allows access from all networks
    apt_acng_allow: []
    
    # Default base packages to install
    # This list will be included in Debian Preseed configuration
    apt_base_packages: [ 'ed', 'python', 'python-apt', 'lsb-release', 'make', 'sudo', 'gnupg-curl',
                         'git', 'wget', 'curl', 'rsync', 'netcat-openbsd', 'bridge-utils', 'vlan',
                         'openssh-server', 'openssh-blacklist', 'openssh-blacklist-extra',
                         'python-pycurl', 'python-httplib2', 'apt-transport-https', 'acl' ]
    
    # List of additional "global" packages to install
    apt_packages: []
    
    # List of packages for a group of hosts (only one group supported)
    apt_group_packages: []
    
    # List of packages to install on a given host
    apt_host_packages: []
    
    apt_debian_preseed_hostname: '{{ ansible_hostname }}'
    apt_debian_preseed_domain: '{{ ansible_domain }}'
    apt_debian_preseed_locale: 'en_US.UTF-8'
    apt_debian_preseed_language: 'English'
    apt_debian_preseed_timezone: 'UTC'
    apt_debian_preseed_keyboardvariant: 'American English'
    apt_debian_preseed_mirror_country: 'United States'
    apt_debian_preseed_rootpw: 'debian'
    apt_debian_preseed_rootpw_length: '20'
    apt_debian_preseed_filesystem: 'ext4'



List of internal variables used by the role:

    nginx_server_default
    apt_debian_preseed_username
    apt_debian_preseed_rootpw
    apt_debian_preseed_sshkey






### Authors and license

`apt` role was written by:

- Maciej Delmanowski | [e-mail](mailto:drybjed@gmail.com) | [Twitter](https://twitter.com/drybjed) | [GitHub](https://github.com/drybjed)

License: [GPLv3](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)



***

This role is part of the [DebOps](http://debops.org/) project. README generated by [ansigenome](https://github.com/nickjj/ansigenome/).
