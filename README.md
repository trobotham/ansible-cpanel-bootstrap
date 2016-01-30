![](https://lithiumhosting.com/images/logo_new_black.png)

# ANSIBLE CPANEL BOOTSTRAP
**from Lithium Hosting**  
We're always open to pull requests, feel free to make this your own or help us make it better.

### Copyright
(c) Lithium Hosting, llc

### License
This ansible playbook is licensed under the MIT license; you can find a full copy of the license itself in the file /LICENSE

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
  * [Domain Stats](http://www.gk-root.com/GK-Apps/Domains-Statistics)
  * [Account DNS Check](https://www.ndchost.com/cpanel-whm/addons/accountdnscheck/)

* * *

### Usage

The purpose of this package is to allow System Administrators to easily deploy cPanel to a new CentOS server.  
This package also supports provisioning a cPanel DNS-Only server and can be used to pre-install several software packages commonly used with cPanel.  

Please note, this playbook is not intended to be run on functional cPanel servers, it's sole purpose is for the initial provisioning / bootstrapping of a new server.  

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
install-kernelcare | Installs Kernelcare but requires a licensed IP address
install-cpanel | downloads and installs cPanel
install-cloudlinux | downloads and installs CloudLinux but requires a licensed IP address
install-cloudlinux-cagefs | installs cagefs, requires CloudLinux
install-cloudlinux-alt-packages | installs the Alt packages for PHP, Python and Ruby. requires CloudLinux
install-composer | Installs composer
install-configserver-csf | Installs ConfigServer Firewall
install-configserver-cmc | Installs ConfigServer Modsecurity Control
install-configserver-cmm | Installs ConfigServer Mail Manage
install-configserver-cmq | Installs ConfigServer Mail Queues
install-configserver-cse | Installs ConfigServer Explorer
install-configserver-cxs | Installs ConfigServer eXploit Scanner
install-rvskin | Installs RVSkin and requires a licensed IP
install-rvsitebuilder | Installs RVSiteBuilder and requires a licensed IP
install-softaculous | Installs Softaculous and requires a licensed IP
install-cloudflare | Installs the cPanel Cloudflare Plugin
install-attracta | Installs the Attacta plugin
install-postgresql | Installs the cPanel supported version of PostgreSQL
install-rfx-lmd | Installs Linux Malware Detect
install-rfx-lsm | Linux Socket Monitor
install-rfx-prm | Process Resource Monitor
install-rfx-sim | System Integrity Monitor
cpanel-comodo-waf | Installs the Comodo WAF rules for modsecurity
li_cpanel_whm_plugins | Installs some extra WHM plugins, Logview, RemoteMXWizard, Domain Stats and Account DNS Check

**Variables**  
All variables are defined as defaults in the Roles. This means you can override them with other values quite easily.  
We've provided a sample group_vars structure to get you started.  
Refer [to this document](http://http://docs.ansible.com/ansible/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable) for more information on variable precedence.

Some variables MUST be defined by you or certain tasks won't run.  Variables prefixed with "install_" are used to determine if a specific app should be installed.  Any install tasks will check if that variable is "true".  Variables prefixed with "use_" are used to determine if a specific role / task should be run (this may include installing a package like NTP but not an app like CloudLinux).  
Below is a list of those variables.  Please note the defaults, if you don't define a variable in your host_vars or group_vars, the default will be used. 

Variable | Default | Choices | Description
:------------- | :------------- | :------------- | :-------------
use_python | true | <ul><li>false</li><li>true</li></ul> | should we configure Python packages?
use_ntp | false | <ul><li>false</li><li>true</li></ul> | should ntp be installed and configured?
use_locale | true | <ul><li>false</li><li>true</li></ul> | should the locale be configured?
use_ssh | false | <ul><li>false</li><li>true</li></ul> | use a custom sshd_config file?
use_comodo_waf | false | <ul><li>false</li><li>true</li></ul> | should the comodo waf rules be added to modsecurity?
install_composer | false | <ul><li>false</li><li>true</li></ul> | do you want to install composer?
install_csf | false | <ul><li>false</li><li>true</li></ul> | do you want to install ConfigServer Firewall?
install_cmc | false | <ul><li>false</li><li>true</li></ul> | do you want to install ConfigServer ModSecurity Control??
install_cmm | false | <ul><li>false</li><li>true</li></ul> | do you want to install ConfiServer Mail Manage?
install_cmq | false | <ul><li>false</li><li>true</li></ul> | do you want to install ConfiServer Mail Queues?
install_cse | false | <ul><li>false</li><li>true</li></ul> | do you want to install ConfiServer Explorer?
install_cxs | false | <ul><li>false</li><li>true</li></ul> | do you want to install ConfiServer eXploit Scanner? (requires a licensed IP)
install_rvsitebuilder | false | <ul><li>false</li><li>true</li></ul> | do you want to install RVSitebuilder? (requires a licensed IP)
install_rvskin | false | <ul><li>false</li><li>true</li></ul> | do you want to install RVSkin? (requires a licensed IP)
install_softaculous | false | <ul><li>false</li><li>true</li></ul> | do you want to install RVSkin? (requires a licensed IP)
install_cloudflare | false | <ul><li>false</li><li>true</li></ul> | do you want to install the CloudFlare cPanel plugin?
install_pgsql | false | <ul><li>false</li><li>true</li></ul> | do you want to install the cPanel supported version of PostgreSQL?
install_attracta | false | <ul><li>false</li><li>true</li></ul> | do you want to install the Attracta cPanel plugin?
install_cloudlinux | false | <ul><li>false</li><li>true</li></ul> | do you want to install CloudLinux? (requires a licensed IP)
install_kernelcare | false | <ul><li>false</li><li>true</li></ul> | do you want to install KernelCare? (requires a licensed IP)
install_cloudlinux_cagefs | false | <ul><li>false</li><li>true</li></ul> | do you want to install CageFS? (requires CloudLinux)
install_cloudlinux_alt | false | <ul><li>false</li><li>true</li></ul> | do you want to install Alt-PHP, Alt-Python and Alt-Ruby? (requires CloudLinux)
install_rfx_lmd | false | <ul><li>false</li><li>true</li></ul> | do you want to install R-fx Networks (Linux Malware Detect)
install_rfx_sim | false | <ul><li>false</li><li>true</li></ul> | do you want to install R-fx Networks (System Integrity Monitor)
install_rfx_prm | false | <ul><li>false</li><li>true</li></ul> | do you want to install R-fx Networks (Process Resource Monitor)
install_rfx_lsm | false | <ul><li>false</li><li>true</li></ul> | do you want to install R-fx Networks (Linux Socket Monitor)


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

**This is just a starting point, internally we use another 20 roles to customize our servers.**  
I hope this has helped you get started, if you'd like to show your gratitude, we're trying to encourage knowledge-sharing between web-hosts and other IT professionals.  
Send us an email [info@lithiumhosting.com](mailto:info@lithiumhosting.com) with a link to an article, project or post that details something unique you or your organization does that you'd like to share.    
We'll add it to a list of contributors and give you credit.  Pay it forward and share your knowledge with others.  

We are also very open to pull requests, comments and feedback.  Please let us know if you have any issues, have suggestions for improving roles and tasks or would like to see new features.
