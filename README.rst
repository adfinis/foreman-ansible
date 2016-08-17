========================
Foreman Ansible Playbook
========================

|Travis| |License|

.. |Travis| image:: https://img.shields.io/travis/adfinis-sygroup/foreman-ansible.svg?style=flat-square
   :target: https://travis-ci.org/adfinis-sygroup/foreman-ansible
.. |License| image:: https://img.shields.io/github/license/adfinis-sygroup/foreman-ansible.svg?style=flat-square
   :target: LICENSE

Ansible playbook to deploy a complete up and running Foreman instance within
minutes.

Features
========
The goal of this playbook is to offer a fully automated way to deploy a
complete and ready-to-use Foreman instance within minutes.

It contains multiple different roles with numerous customizable variables,
which provide the following features:

* setup database (SQLite or MySQL)
* setup webserver (plain nginx as a proxy or nginx-passenger)
* setup isc-dhcp-server
* setup TFTP server
* setup foreman-proxy
* setup Foreman including configuration (templates, hosts, domains, etc.)

**None of the roles will install Puppet or use the official foreman-installer,
instead the plain Foreman packages are used!**

In addition this playbook makes use of `foreman-yml`_ to automatically configure
Foreman through the API based on a YAML file, which includes adding all 
templates, OS, media, hosts, etc. and linking them accordingly.

Please note that at the current time the following distributions are supported:

* Debian 7 & 8
* Ubuntu 14.04 & 16.04
* CentOS 6 & 7
* Red Hat Enterprise Linux 6 & 7

Requirements
============
The target machine should fulfill the following requirements before the
playbook is applied:

* FQDN configured
* SELinux disabled
* Required ports 67, 69, 80, 443, etc. open
* Internet and repository access (e.g. Red Hat Optional repository)

**Ansible 2.0+ is required to use this playbook!**

Installation
============
Below the required steps to execute the default playbook:

1. Clone this repository
2. Initialize the submodules containing the foreman-yml repository: ::

   $ git submodule update --init

3. Install and configure Ansible to manage the target server
4. Create an inventory file containing either the hostname or IP address of
   target machine: ::

    $ echo "$TARGET_IP" > /tmp/inventory

5. Use the playbook foreman.yml to deploy a default setup with MySQL,
   nginx-passenger, TFTP, DHCP and foreman-proxy: :: 

    $ ansible-playbook foreman.yml -i /tmp/inventory -u root

6. After a successful deployment you should be able to access Foreman through 
   http://$TARGET_IP/.

The password of the ``admin`` user is by default set to ``foreman``. In addition
``safemode_render`` is changed to ``false``.

Examples
========
The templates directory contains example `foreman-yml`_ YAML templates to
give you a head start to bootstrap Foreman.

In addition the variables overwritten in vars/example.yml are the minimum
amount of variables that need to be defined, e.g. the MySQL role does not
create any users or databases by default.

Roles
=====
Below a short overview of all included roles:

+-----------------+----------------------------------------------------+
| Name            | Description                                        |
+=================+====================================================+
| common          | update apt cache                                   |
+-----------------+----------------------------------------------------+
| foreman         | add repos and install Foreman                      |
+-----------------+----------------------------------------------------+
| foreman_proxy   | add repos, install and configure foreman-proxy     |
+-----------------+----------------------------------------------------+
| foreman_yml     | configure the Foreman instance with `foreman-yml`_ |
+-----------------+----------------------------------------------------+
| isc_dhcp_server | install and configure isc-dhcp-server              |
+-----------------+----------------------------------------------------+
| mysql           | install MySQL, create users and databases          |
+-----------------+----------------------------------------------------+
| nginx           | add upstream repos if requested and setup nginx    |
+-----------------+----------------------------------------------------+
| passenger_nginx | add repos and setup passenger-nginx                |
+-----------------+----------------------------------------------------+
| sqlite          | install sqlite and create db directory             |
+-----------------+----------------------------------------------------+
| tftp            | install and setup TFTP including PXE boot files    |
+-----------------+----------------------------------------------------+

Upcoming features
=================
See the issues page for a list of upcoming and planned features.

Contributions
=============
Contributions are more than welcome! Please feel free to open new issues or
pull requests.

License
=======
GNU GENERAL PUBLIC LICENSE Version 3

See the `LICENSE`_ file.

.. _LICENSE: LICENSE
.. _foreman-yml: https://github.com/adfinis-sygroup/foreman-yml
