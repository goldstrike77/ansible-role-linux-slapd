![](https://img.shields.io/badge/Ansible-openldap-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_openldap.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Software Versions](#Software-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
OpenLDAP is a free, open-source implementation of the Lightweight Directory Access Protocol.

FusionDirectory is an identity management solution - IAM, Provides daily management of data stored in an LDAP directory. Becoming the cornerstone of the information system, the corporate directory becomes more complex offering more data and managing more infrastructure services. Simple and can be used to delegate fully or partly the data management to non-specialists.

FusionDirectory provides a simplified interface for identity management while being extensible. The FusionDirectory project aims to fill this gap by providing a nice web application that allows you not only to manage your classical OpenLDAP data like users, groups, services... but also offer an API allowing to write new plugins to the extent the application to be more useful to you.

Bundled with many plugins today ranging from user management to service management and systems management, everything managed trough your LDAP server.

* User, group, roles, sudo ...
* Systems
* Network services : SMTP / DNS / DHCP / Samba
* and much more ...

FusionDirectory is user-friendly and includes a number of features and modes including:

* A copy and paste system
* Template mode for all objects stored with FusionDirectory
* Snapshot mode
* Dashboard (user, password, expiration of users, installation and deployment)
* Trigger to create action on other systems after saving, modifying, removing
<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/fusiondirectory.png" /></p>

## Requirements
### Operating systems
This Ansible role installs OpenLDAP and FusionDirectory on Linux operating system, including establishing a filesystem structure and server configuration with some common operational features, support for replication for multimaster, Will works on the following operating systems:

  * CentOS 7

### Software versions

The following list of supported the Software releases:

* OpenLDAP 2.4+
* FusionDirectory 1.3+

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `slapd_root_user`: Specify the DN to access control or administrative restrictions for operations.
* `slapd_root_pass`: Specify a password for the DN for the rootdn.
* `slapd_ssl`: A boolean value, whether Encrypting client and cluster communications.
* `slapd_path`: Specify the OpenLDAP database and logs main-directory.

##### Listen port
* `slapd_port_arg`: Defines communication port.

##### Backup parameters
* `slapd_backupset_arg.keep`: Backup retention cycle in days.
* `slapd_backupset_arg.encryptkey`: BackupSet encryption key.
* `slapd_backupset_arg.cloud_rsync`: Whether rsync for cloud storage.
* `slapd_backupset_arg.cloud_drive`: Specify the cloud storage providers.
* `slapd_backupset_arg.cloud_bwlimit`: Controls the bandwidth limit.
* `slapd_backupset_arg.cloud_event`: Define transfer events.
* `slapd_backupset_arg.cloud_config`: Specify the cloud storage configuration.

##### Fusiondirectory parameters
* `slapd_fd_dept`: A boolean value, whether to use FusionDirectory for management.
* `slapd_fd_php_version`: Specify the php-fpm version.
* `slapd_fd_php_fpm_port`: Defines Php-fpm instance listening port.
* `slapd_fd_user`: Specify the Fusiondirectory administrator username.
* `slapd_fd_pass`: Specify the Fusiondirectory administrator password.
* `slapd_fd_tz`: Defines the timezone used within FusionDirectory to handle date related tasks.
* `slapd_fd_pass_hash`: Defines the default password hash to choose for new accounts.
* `slapd_fd_pass_min_length`: Determines the minimum length of a new password entered to be considered valid.
* `slapd_fd_pass_min_differ`: Determines how many characters that must be different from the previous password.
* `slapd_fd_uid_base`: Base number for user id.
* `slapd_fd_gid_base`: Base number for group id.

##### NGinx parameters
* `slapd_ngx_block_agents`: Enables or disables block unsafe User Agents.
* `slapd_ngx_block_string`: Enables or disables block includes Exploits / File injections / Spam / SQL injections.
* `slapd_ngx_compress`: Enables or disables compression.
* `slapd_ngx_domain`: Defines domain name.
* `slapd_ngx_pagespeed`: Enables or disables pagespeed modules.
* `slapd_ngx_port_http`: NGinx HTTP listen port.
* `slapd_ngx_port_https`: NGinx HTTPs listen port.
* `slapd_ngx_ssl_protocols:` Defines SSL protocol profile.
* `slapd_ngx_version`: extras or standard
* `slapd_ngx_site_path`: Specify the NGinx site directory.
* `slapd_ngx_logs_path`: Specify the NGinx logs directory.

##### Service Mesh
* `environments`: Define the service environment.
* `datacenter`: Define the DataCenter.
* `domain`: Define the Domain.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

### Other parameters
There are some variables in vars/main.yml:

## Dependencies
- Ansible versions >= 2.8
- Python >= 2.7.5
- [NGinx](https://github.com/goldstrike77/ansible-role-linux-nginx)
- [PHP-FRM](https://github.com/goldstrike77/ansible-role-linux-phpfpm)

## Example

### Hosts inventory file
See tests/inventory for an example.

    [ldap]
    node01 ansible_host='192.168.1.10'
    node02 ansible_host='192.168.1.11'
    node03 ansible_host='192.168.1.12'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: ldap
  roles:
     - role: ansible-role-linux-slapd
```

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`.

```yaml
slapd_root_user: 'ldapadm'
slapd_root_pass: 'v9rJsZFtHTmW'
slapd_ssl: true
slapd_path: '/data'
slapd_port_arg:
  ldap: '389'
  ldaps: '636'
  exporter: '9330'
slapd_backupset_arg:
  keep: '15'
  encryptkey: 'aEU2y9FcKkeQ'
  cloud_rsync: true
  cloud_drive: 'azureblob'
  cloud_bwlimit: '10M'
  cloud_event: 'sync'
  cloud_config:
    account: 'blobuser'
    key: 'base64encodedkey=='
    endpoint: 'blob.core.chinacloudapi.cn'
slapd_fd_dept: true
slapd_fd_php_version: '73'
slapd_fd_php_fpm_port: '9000'
slapd_fd_user: 'fd-admin'
slapd_fd_pass: 'JZjL9DWmBdJw'
slapd_fd_tz: 'Asia/Shanghai'
slapd_fd_pass_hash: 'crypt/sha-512'
slapd_fd_pass_min_length: '12'
slapd_fd_pass_min_differ: '1'
slapd_fd_uid_base: '1100'
slapd_fd_gid_base: '1100'
slapd_ngx_block_agents: true
slapd_ngx_block_string: true
slapd_ngx_compress: true
slapd_ngx_domain: 'fusiondirectory.example.com'
slapd_ngx_pagespeed: false
slapd_ngx_port_http: '80'
slapd_ngx_port_https: '443'
slapd_ngx_ssl_protocols: 'modern'
slapd_ngx_version: 'standard'
slapd_ngx_site_path: '/data/nginx/site'
slapd_ngx_logs_path: '/data/nginx/logs'
environments: 'Development'
datacenter: 'dc01'
domain: 'local'
tags:
  subscription: 'default'
  owner: 'nobody'
  department: 'Infrastructure'
  organization: 'The Company'
  region: 'China'
exporter_is_install: false
consul_public_register: false
consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
consul_public_http_prot: 'https'
consul_public_http_port: '8500'
consul_public_clients:
  - '127.0.0.1'
```

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
