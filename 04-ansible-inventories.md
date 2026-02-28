# Ansible Inventory

## 01. What is an Ansible `Hosts` file?

- Ansible Host file (also known as inventory) defines the managed nodes configurations that you automate, with groups so you can run automation tasks on multiple hosts at the same time.
- Once your inventory is defined, you use patterns to select the hosts or groups you want Ansible to run actions on it.
- The simplest inventory is a single file with a list of hosts and groups.
- The default location for this file is <b>_/etc/ansible/hosts_</b>.
- You can specify a different inventory file at the command line using the -i <path> option or in configuration using inventory.
- The most common inventory file formats are:
  - <b>_INI_</b>
  - <b>_YAML_</b>
- Sample inventory file

  ```
    appserver.novatec.com

    [webservers]
    webserver1.novatec.com
    webserver2.novatec.com
    webserver3.novatec.com

    [dbservers]
    dbserver1.novatec.com
    dbserver2.novatec.com
  ```

## 02. How to connect to remote host using Ansible inventory file (INI/YAML) ?

```
# With inventory in INI format
ansible -i 02-sample-inventory.ini -m ping
```

```
# With inventory in YAML format
ansible -i 04-sample-inventory.yaml -m ping
```

## 03. Passing multiple inventory sources

- You can target multiple inventory sources at the same time by giving multiple inventory parameters from the command line or by configuring ANSIBLE_INVENTORY.
- This can be useful when you want to target normally separate environments, like staging and production, at the same time for a specific action.
- To target two inventory sources from the command line:

```
ansible-playbook get_logs.yml -i staging-inventory.yml -i prod-inventory.yml
```

## 04. Organizing Inventory in a custom directory

- You can consolidate multiple inventory sources in a single directory. The simplest version of this is a directory with multiple files instead of a single inventory file.
- A single file gets difficult to maintain when it gets too long.
- If you have multiple teams and multiple automation projects, having one inventory file per team or project lets everyone easily find the hosts and groups that matter to them.
- You can also combine multiple inventory source types in an inventory directory.
- This can be useful for combining static and dynamic hosts and managing them as one inventory.
- The following inventory directory combines an inventory plugin source, a dynamic inventory script, and a file with static hosts:
  ```
  inventory/
     openstack.yml          # configure inventory plugin to get hosts from OpenStack cloud
     dynamic-inventory.py   # add additional hosts with dynamic inventory script
     on-prem                # add static hosts and groups
     parent-groups          # add static hosts and groups
  ```
- You can also configure the inventory directory in your _ansible.cfg_

## 05. Adding variables to the inventory file

- https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#connecting-to-hosts-behavioral-inventory-parameters

## 06. Assigning variables to many hosts: Group Variables

- If all hosts in a group share a variable value, you can apply that variable to an entire group at once.
- In INI inventory file, it will be like:

  ```
  [mumbaiServers]
     host1
     host2

  [mumbaiServers:vars]
     smtp_server=smtp.mumbai.novatec.com
     proxy=proxy.mumbai.novatec.com
  ```

- In YAML inventory file, it will be like:
  ```
   mumbai:
     hosts:
       host1:
       host2:
     vars:
       smtp_server: smtp.mumbai.novatec.com
       proxy: proxy.mumbai.novatec.com
  ```

## 07. [Ansible Inventory Parameters (Variables)](https://github.com/novatecstack/ansible-masterclass/blob/main/Module-05_Ansible_Inventory/inventory-variables.md)

## 08. [Ansible Inventory Variables](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#connecting-to-hosts-behavioral-inventory-parameters)

Following variables control how Ansible interacts with remote hosts:

- <b>_ansible_connection_</b></br>
  Connection type to the host. This can be the name of any of ansible’s connection plugins. SSH protocol types are smart, ssh or paramiko. The default is smart.
  General for all connections:
- <b>_ansible_host_</b></br>
  The name of the host to connect to, if different from the alias you wish to give to it.

- <b>_ansible_port_</b></br>
  The connection port number, if not the default (22 for ssh)

- <b>_ansible_user_</b></br>
  The user name to use when connecting to the host

- <b>_ansible_password_</b></br>
  The password to use to authenticate to the host (never store this variable in plain text; always use a vault. See Keep vaulted variables safely visible)
  Specific to the SSH connection:

- <b>_ansible_ssh_private_key_file_</b></br>
  Private key file used by ssh. Useful if using multiple keys and you don’t want to use SSH agent.

- <b>_ansible_ssh_common_args_</b></br>
  This setting is always appended to the default command line for sftp, scp, and ssh. Useful to configure a ProxyCommand for a certain host (or group).

- <b>_ansible_sftp_extra_args_</b></br>
  This setting is always appended to the default sftp command line.

- <b>_ansible_scp_extra_args_</b></br>
  This setting is always appended to the default scp command line.

- <b>_ansible_ssh_extra_args_</b></br>
  This setting is always appended to the default ssh command line.

- <b>_ansible_ssh_pipelining_</b></br>
  Determines whether or not to use SSH pipelining. This can override the pipelining setting in ansible.cfg.

- <b>_ansible_ssh_executable (added in version 2.2)_</b></br>
  This setting overrides the default behavior to use the system ssh. This can override the ssh_executable setting in ansible.cfg.
  Privilege escalation (see Ansible Privilege Escalation for further details):

- <b>_ansible_become_</b></br>
  Equivalent to ansible_sudo or ansible_su, allows to force privilege escalation

- <b>_ansible_become_method_</b></br>
  Allows to set privilege escalation method

- <b>_ansible_become_user_</b></br>
  Equivalent to ansible_sudo_user or ansible_su_user, allows to set the user you become through privilege escalation

- <b>_ansible_become_password_</b></br>
  Equivalent to ansible_sudo_password or ansible_su_password, allows you to set the privilege escalation password (never store this variable in plain text; always use a vault. See [Keep vaulted variables safely visible](https://docs.ansible.com/ansible/latest/tips_tricks/ansible_tips_tricks.html#tip-for-variables-and-vaults))

- <b>_ansible_become_exe_</b></br>
  Equivalent to ansible_sudo_exe or ansible_su_exe, allows you to set the executable for the escalation method selected

- <b>_ansible_become_flags_</b></br>
  Equivalent to ansible_sudo_flags or ansible_su_flags, allows you to set the flags passed to the selected escalation method. This can be also set globally in ansible.cfg in the sudo_flags option
  Remote host environment parameters:

- <b>_ansible_shell_type_</b></br>
  The shell type of the target system. You should not use this setting unless you have set the ansible_shell_executable to a non-Bourne (sh) compatible shell. By default commands are formatted using sh-style syntax. Setting this to csh or fish will cause commands executed on target systems to follow those shell’s syntax instead.

- <b>_ansible_python_interpreter_</b></br>
  The target host python path. This is useful for systems with more than one Python or not located at /usr/bin/python such as \*BSD, or where /usr/bin/python is not a 2.X series Python. We do not use the /usr/bin/env mechanism as that requires the remote user’s path to be set right and also assumes the python executable is named python, where the executable might be named something like python2.6.

## 09. Reference Links

- [Ansible Inventory Official Documentation](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html)
