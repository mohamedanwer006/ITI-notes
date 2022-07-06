# Ansible

- Ansible is open source configuration management and provisioning tool. similar to Chef , Puppet , SaltStack , etc.

- it uses ssh to connect to remote hosts and run commands. 

- it uses a playbook to define the configuration of the remote hosts.
 


## why Ansible?

- No agents or scripts to configure remote hosts.

- Idempotent Ansible's whole architecture is structured around the concept of idempotency.
    > Idempotent compare state with state 
    > skip configured commands if they are already in the desired state.

- Declarative Not Procedural.
    > Declarative tells you what to do, not how to do it. 

- Tiny learning curve.

> script == playbook 

> Host == Inventory == ips address



### Authorized key

### Inventory
#### default path /etc/ansible/hosts

> can use ip or hostname
```
mail.example.com

[web-servers]
foo.example.com
192.168.12.1

[database]
db.example.com

```

### example of playbook

```yaml

---
- host: web-servers
  remote_user: root
  tasks:
    - name: install nginx
      apt:
        name: nginx
        state: latest
    
```

```yaml

---
- host: web-servers
  remote_user: root
  tasks:
    - name: install nginx
      yum:
        name: nginx
        state: latest
    -name : ensure nginx is running
      service:
        name: nginx
        state: started
        enabled: yes

```


ansible project

#### - hosts
```
192.168.1.1
```

### - ansible.cfg
```
[defaults]
inventory = ./hosts
remote_user = iti
ask_pass = false
roles_path = ./roles
host_key_checking = False

[privilege_escalation]
become = true
become_method = sudo
become_user = root
become_ask_pass = false

```

# on the manage node
# Add user iti to sudoers

/etc/sudoers.d/ansible
```
iti ALL=(ALL) NOPASSWD: ALL
```

### Ansible adhoc command  

```bash
ansible -i ./hosts -m ping all
ansible web-servers -m shell -a "hostname"
```
- -i for inventory
- -m for module
- -a for arguments

run command module 
- command
- shell
- raw 

### list modules

```bash
ansible-doc --list-modules
```
###  modules examples
```bash
ansible-doc <module name>
ansible-doc yum
```


### Ansible Playbook

```yaml
- name: first palybook
  hosts: web-servers
  become: true
  become_user: root
  tasks:
    - name: install nginx
      yum:
        name: nginx
        state: latest
    - name : ensure nginx is running
      service:
        name: nginx
        state: started
        enabled: yes
``` 

### run playbook

```bash
ansible-playbook -i ./hosts ./playbook.yml
```

### gather facts

```bash
ansible -i ./hosts -m setup all
ansible web-server -m setup
```

### encrypt using vault
```
ansible-vault 
encrypt lab.yaml--vault-password-file=~/vault_key 

```

