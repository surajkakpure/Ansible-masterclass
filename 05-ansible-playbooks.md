# Ansible Playbooks

## Controlling where to run the task ?

- Localhost
- Server
- Group of Servers
- all

## Ansible Playbook - `Structure`

```
---
- name: play-1
  hosts: localhost
  tasks:
    - name: play-1-task-1
      ansible.builtin.debug:
        msg:
          - "Ansible is awesome"
    - name: play-1-task-2
      ansible.builtin.debug:
        msg:
          - "Ansible contributes in DevOps"

- name: play-2
  hosts: localhost
  tasks:
    - name: play-2-task-1
      ansible.builtin.debug:
        msg:
          - "Ansible is awesome"
    - name: play-2-task-2
      ansible.builtin.debug:
        msg:
          - "Ansible contributes in DevOps"
```

## 01. Print a message or a variable

- Connect to your ansible server.
- In order to print the message or a variable during execution, use [ansible.builtin.debug](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/debug_module.html) module.

### Step-1.1: Create a playbook (.yaml/.yml)

- The below playbook is for printing a hello message on ansible server.

```
---
- name: debug module demo
  hosts: localhost
  tasks:
    - name: debug module
      ansible.builtin.debug:
        msg: hello
```

- Save the above the playbook with .yml/.yaml extension.

### Step-1.2: Execute the Ansible playbook

```
# Syntax
$ ansible-playbook <playbook_name>.yml

# Example
$ ansible-playbook debug.yml
```

- The above command will execute the playbook on localhost (ansible server) and display a hello message.

### Step-1.3: Print a variable value during execution

- Create another playbook, say _debug-with-var.yml_ with a variable **greeting**.

```
---
- name: debug module demo
  hosts: localhost
  vars:
    greeting: "Ansible"
  tasks:
    - name: debug module
      ansible.builtin.debug:
        msg: "Hello, from {{ greeting }}"

```

- Now, execute the playbook to see the result

```
ansible-playbook debug-with-var.yml
```

## 02. Pause an Ansible module execution

- In order to pause an ansible module execution, use [ansible.builtin.pause](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/pause_module.html) module.

- Create a new ansible playbook, say _pause-demo.yml_

```
---
- name: pause module lab
  hosts: localhost
  vars:
    wait_seconds: 10
  tasks:
    - name: pause for {{ wait_seconds | int }} seconds
      ansible.builtin.pause:
        seconds: "{{ wait_seconds | int }}"
```

- Now, execute the playbook to see the result

```
ansible-playbook pause-demo.yml
```

## 03. How to pass variable to Ansible playbooks?

- There are multiple ways to pass variables to Ansible playbooks:

  1. --extra-vars "greeting=hello"
  2. --extra-vars '{"greeting":"hello"}'
  3. --extra-vars "@file.json"
  4. --extra-vars "@file.yml"

- Let's see how you can pass a variable using #1 method.

### Step-3.1: Create an Ansible Playbook

- Create a new playbook, say _var-lab-pb.yml_

```
---
- name: var pass lab demo
  hosts: localhost
  vars:
    greeting: "Hello"
  tasks:
    - name: display greeting msg
      ansible.builtin.debug:
        msg: "{{ greeting }} there"
```

### Step-3.2: Run the playbook with exiting variable (greeting) value

```
ansible-playbook <playbook_name>.yml
```

### Step-3.3: Run the playbook by passing new variable value

- Pass a new value to variable "greeting" via command line using _extra-vars_ flag. It will override the existing variable value.

```
# Pass variable to playbook using #1 method
ansible-playbook --extra-vars="greeting=Hi" <playbook_name>.yml

# Pass variable to playbook using #2 method
ansible-playbook --extra-vars='{"greeting": "Hi"}' <playbook_name>.yml

```

## 04. Break a string over mutiple lines

- "|" - **Literal block scaler**
- ">" - **Folded block scaler**

- Create a new ansible playbook, say _var-lab-playbook.yml_ with two variables (variable1 with "|" and variable2 with ">")

```
---
- name: debug lab demo
  hosts: localhost
  vars:
    variable1: |
      these are the
      three lines of wonderful
      poetry
    variable2: >
      these are the
      three lines of wonderful
      poetry
  tasks:
    - name: print var1
      ansible.builtin.debug:
        var: variable1

    - name: print var2
      ansible.builtin.debug:
        var: variable2

```

- Now, execute the playbook to see the result

```
ansible-playbook var-lab-playbook.yml
```

## 05. `ansible_hostname` vs `inventory_hostname`

## 06. Execute a command on Ansible host

- There are multiple ways to execute a command on ansible host:
  1. Connection plugin
  2. delegate_to: localhost
  3. local_action

## 07. Limiting playbooks to particular hosts and groups

- In this case (assuming your inventory file contains a webservers group), even if the playbook is set to `hosts: all`, or includes hosts in addition to what is defined in the `webservers` group, it will only be run on the hosts defined in _webservers_.

```
ansible-playbook playbook.yml --limit webservers
```

- You could also limit the playbook to one particular host:

```
ansible-playbook playbook.yml --limit xyz.example.com
```

- If you want to see a list of hosts that would be affected by your playbook before you actually run it, use `--list-hosts` option:

```
ansible-playbook playbook.yml --list-hosts
```

## 08. Setting user and sudo options with ansible-playbook

## Hands-on Labs

1. Create a simple playbook to print a messgae on console of localsystem
2. Create a playbook to ping a remote machine to check connectivity
3. Create a playbook to install a package (httpd/nginx) on remote machine and start the associated service
4. Create a playbook to install a package (httpd/nginx) on mixed platforms (diff OS families)
5. Demonstrate the use of Ansible Variables
6. Create a playbook to create a user account on a remote linux machine
   07: Create a playbook to configure a webserver (remote) with below specs:
   Install httpd
   Start the httpd service
   Enable the httpd service
   Copy the index.html from ansible controller to remote machine (/var/www/html/index.html)
   Restart the httpd service
