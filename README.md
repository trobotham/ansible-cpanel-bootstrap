![](https://lithiumhosting.com/images/logo_new_black.png)

# ANSIBLE CPANEL BOOTSTRAP
**from Lithium Hosting**  
We're always open to pull requests, feel free to make this your own or help us make it better.

### Copyright
(c) Lithium Hosting, llc

### License
This library is licensed under the MIT license; you can find a full copy of the license itself in the file /LICENSE

### Requirements
- Ansible 2.0+
- CentOS 6.x Server targeted for deployment of cPanel (not currently supporting CentOS 7)
- Knowledge of Ansible, SSH and authenticating to a remote server

### Description
Ansible Playbook for installing cPanel / cPanel DNS-Only on a fresh CentOS server  
Support for installing / configuring the following Applications:  
* cPanel
  * PostgreSQL
  * SSH
  * Apache
  * EasyApache3 Profiles
  * MySQL
* [CloudLinux](http://cloudlinux.com)
* [KernelCare](http://kernelcare.com)
* [ConfigServer](http://configserver.com)
  * CSF
  * CXS
  * CMM
  * CMQ
  * CSE
  * CMC
* [RFX Apps](http://www.rfxn.com/)
  * LSM
  * SIM
  * LMD
  * PRM
* [RVSkin](http://rvskin.com)
* [RVSiteBuilder](http://rvsitebuilder.com)
* [Softaculous](http://softaculous.com)
* WHM Plugins
  * [CloudFlare](https://www.cloudflare.com)
  * [Attracta SEO Tools](https://www.attracta.com)
  * [LogView](http://log-view.com)
  * [RemoteMXWizard](http://www.gk-root.com/GK-Apps/Remote-MX-Wizard)

* * *

### Usage

The purpose of this package is to allow System Administrators to easily deploy cPanel to a new CentOS server.  
This package also supports provisioning a cPanel DNS-Only server and can be used to pre-install several software packages commonly used with cPanel.  

**Inventory Files**  
By default, we've only included a production file called production.example.  Rename this to production before using.  This file structure can be used in a development or testing inventory file as per your needs  
The group names in the file are simply a suggestion, you're welcome to make any changes as needed.

**Inventory Groups**  

Group Name | Purpose
:------------- | :-------------
cpanel | This group should contain your cPanel servers
cpanel-dnsonly | This group is for DNS Only servers

**Roles**  

Role Name | Description
:------------- | :-------------
os-centos | OS updates, repositories, packages, users, passwords, ssh keys, ssh

**Variables**  
All variables are defined as defaults in the Roles. This means you can override them with other values quite easily.  
We've provided a sample group_vars structure to get you started.  
Refer [to this document](http://http://docs.ansible.com/ansible/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable) for more information on variable precedence.

Some variables MUST be defined by you or certain tasks won't run.  Variables prefixed with "install_" are used to determine if a specific app should be installed.  Any install tasks will check if that variable is "true".  Variables prefixed with "use_" are used to determine if a specific role / task should be run (this may include installing a package like NTP but not an app like CloudLinux).  
Below is a list of those variables.  

Variable | Default | Choices | Description
:------------- | :------------- | :------------- | :-------------
install_cloudlinux | false | <ul><li>false</li><li>true</li></ul>  | should we install and configure CloudLinux?
use_python | true | <ul><li>false</li><li>true</li></ul> | should we configure Python packages?
use_ntp | false | <ul><li>false</li><li>true</li></ul>  | should ntp be installed and configured?
use_locale | true | <ul><li>false</li><li>true</li></ul>  | should the locale be configured?

**Supported Tags**  

Tag Name | Description
:------------- | :-------------
yum | Any yum related task
python | Any python related task
ntp | Any ntp related task
users | Any user related task
ssh | Any ssh server related task
locale | Any locale server related task

**Running a Play**  
In the event of a failure, the play will stop for the failed host.  By declaring --force-handlers, any successful changes that require a service restart will still be applied.  

Run a play and use vault password file, in this example we used a file called .vault-pass which contains the vault password
```
ansible-playbook -i production cpanel.yml --force-handlers --vault-password-file .vault-pass
```
Run a play and prompt for the vault password
```
ansible-playbook -i production cpanel.yml --force-handlers --ask-vault-pass
```

**Vault Encryption / Decryption**  
Encrypt your vault files.  Be sure to reference all of them if you use more than one. Refer above to using a vault password file or prompt
```
ansible-vault encrypt group_vars/all/vault --vault-password-file .vault-pass
```
Decrypt them.
```
ansible-vault decrypt group_vars/all/vault --vault-password-file .vault-pass
```
If you have host key check issues, try this on your ansible server:  
```
export ANSIBLE_HOST_KEY_CHECKING=False
```



### todo
- [x] Basic CentOS Configuration
  - [x] Package Management
    - [x] Extra Yum Repos
    - [x] Default Yum Packages
    - [x] Yum Update
  - [x] Default Python Packages
  - [x] User Management
    - [x] Passwords
    - [x] SSH Keys
  - [x] NTP
- [ ] Deploy cPanel
  - [ ] ...
- [ ] Deploy cPanel DNS-Only
  - [ ] ...
