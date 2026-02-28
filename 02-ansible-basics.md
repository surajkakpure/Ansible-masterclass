# Ansible Basics

## 01. What is Configuration Management?

- Configuration management is a process for maintaining computer systems, servers, applications, network devices, and other IT components in a desired state.
- Using configuration management tools, administrators can set up an IT system, such as a server or workstation, then build and maintain other servers and workstations with the same settings.
- Configuration Management is also a method of ensuring that systems perform in a manner consistent with expectations over time.
- A Configuration management system allows the enterprise to define settings in a consistent manner, then to build and maintain them according to the established baselines.

## 02. Commonly used Configuration Management Tools

- Ansible
- Terraform
- Saltstack
- Puppet
- Chef

## 03. Role and Responsibilities of a `Configuration Management Tool`

### Examples of Configuration Management tasks

- Install, Remove and Update packages (softwares) on remote machines
- Update and Patch Servers
- User Management (Create, Disable, and Delete user accounts)
- Service Management (Enable, Disable, Restart and Reload services)
- Update the production SSL certificates
- Add a new database endpoint
- Change the password for dev, staging, and production email services.
- Add API keys for a new third-party integration
- IT Infrastructure provisioning

## 04. What is `Ansible`

What is Ansible?

- **Ansible** is an open source community project sponsored by Red Hat | Developed by **Michael DeHaan**
- Ansible is a powerful tool which can be used for automating IT tasks (e.g. software installation, configuring servers, deploying apps etc).
- Ansible is _agentless_, relying on temporary remote connections via SSH/WinRM.
- Red Hat acquired Ansible in October 2015.
- Code for Ansible is written in [YAML] (http://yaml.org/), which stands for _YAML Ain't Markup Language_.

## 05. Performing IT Management `with` and `without Ansible`

### Without Ansible

  <img src="images/cm-without-ansible.png" width="650" height="220">

### With Ansible

  <img src="images/cm-with-ansible.png" width="650" height="320">

## 06. How does Ansible work ?

  <img src="images/how-ansible-works.png" width="650" height="320">

## 07. Ansible Architecture

## 08. Ansible Configuration Settings

### Ansible Configuration Settings (ansible.cfg)

- Ansible supports several sources for configuring its behavior, including:

  1. ini file named **ansible.cfg**
  2. environment variables (ANSIBLE_CONFIG)
  3. command-line options ()
  4. playbook keywords
  5. variables

- The default location of ansible configuration file is `/etc/ansible/ansible.cfg`

- Changes can be made and used in a configuration file which will be searched for in the following order:

  - _ANSIBLE_CONFIG_ (environment variable if set)
  - _ansible.cfg_ (in the current directory)
  - _~/.ansible.cfg_ (in the home directory)
  - _/etc/ansible/ansible.cfg_

- For more details about ansible configuration settings, refer https://docs.ansible.com/ansible/latest/reference_appendices/config.html

## 09. Inventories

## 10. Ansible `Collections` & `Modules`

- <b>_Ansible Modules_</b> are discrete units of code that can be used from the command line or in a playbook task.
- Ansible executes each module, usually on the remote managed node, and collects return values.

- [Ansible Collections](https://docs.ansible.com/collections.html)

- [Index of all the Modules](https://docs.ansible.com/ansible/latest/collections/index_module.html)

## 11. Ansible Pricing

- **Ansible Community Edition**

  - Free, open-source
  - No subscription or support fees
  - Developers, small teams and start-ups can use who don't need enterprise-class support

- **AWX**

  - Free, open-source
  - Suitable for development and testing, but not intended for production environments
  - No subscription or support fees

- **Red Hat Ansible Automation Platform**

  - Comes in different subscription tiers: _Self-Support_, _Standard_, and _Premium_.
  - Prices can range from $5,000 to $20,000+ per year, according to [CyberPanel](https://cyberpanel.net/blog/ansible-pricing).
  - Offers support, enterprise features, and certified content.
  - Subscription fees apply
  - Can be used by large organizations with thousands of nodes who needs compliance and security governance
