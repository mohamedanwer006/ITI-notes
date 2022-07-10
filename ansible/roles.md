# roles


### ansible project structure

```
ansible-|
        |
        |- configs
            |-ansible.cfg
        |- plays
            |- play-name.yml
            |- play-name2.yml
        |- roles
                |- role-name
                        |-tasks
                            |- main.yml
                        |- handlers
                            |- main.yml
                        |-templates   
```


#### `paly-name.yml`
```yaml
- hosts: all
  become: true
  roles:
    - roles/role1
    - roles/role2
```

#### roles=>roles1>tasks>`main.yml`
```yaml
- name : install-nginx
  apt: 
    name: nginx
    state: present
- name: install-php
  apt: 
    name: php
    state: present
```
### handlers

> handlebars on level tasks

```yaml
tasks:
    - name: install-nginx
    apt: 
        name: nginx
        state: present
        notify: # notify handler using handlers name
            - restart-nginx
handlers: # list of handlers
  - name: restart-nginx
    service:
      name: nginx
      state: restarted
```

> if there change in tasks then notify will call handler

> when happening a task, change handler will be executed

> if you use a module that is impotency then it will be call every time

> if there handler it will call it after runs all tasks




using variables

- in playbook
- in inventory not recommended deprecated
- in inclusion file

```yaml
- hosts: 
    vars:
        var1: value1
        var2: value2
        var3: value3
        var4: value4
    tasks:
        - name: task1
          debug:
            msg: "{{var1}}"
        - name: task2
          debug:
            msg: "{{var2}}"
        - name: task3
          debug:
            msg: "{{var3}}"
        - name: task4
          debug:
            msg: some dad d tt {{var4}} dfafsdf
```

using files

```
vars_files: 
    - vars/files.yml
tasks:
    -name: task1
      debug:
        msg: "{{var1}}"
```

`vars/files.yml`
```yaml
var1: value1
var2: value2
var3: value3
```

dir host_vars|
             | vim example.example.com

dir group_vars|
              |-vim lab1


```
app: apache2
```


### pass using cli

-e "app=apache2"

### host-file
```
[lamp]
ansible.example.com

[file]
ansible2.example.com

[<group_name>:children]
lamp
file
```

```yaml

  users:
    - username: linda
      shell: /bin/bash
    - username: ahmed
      shell: /bin/bash
```

using 
users.lind.shell

```yaml
-name: task1
  debug:
    msg:  "{{item.username}}"
    loop: "{{users}}"
```

```yaml
-name: task1
  debug:
    msg:  "{{item}}"
    loop:
        - ahmed
        - linda
        - bbo
```

create users
```yaml
- name: create users 
    users:
        name: "{{item.name}}"
        group: "{{item.group}}"
    ```
```

### register
like variable hold return json data from module
```yaml
- name: test register
  hosts: lab1
  tasks:
    - name: first task
      yum: 
       name: " {{item}}"
       state: latest
      loop:
       - httpd
       - nginx
      register: result
    - name: second task
        debug:
            var: result

```

"rc" return code

```
vars:
    supported_distros:
        - ubuntu
        - redhat


when : ansible_distribution in supported_distros

```

in , or,and ,== ,> 

    
```yaml
- name : get the crond server status
  command: systemctl status is-active crond
  register: crond_status
- name : restat sshd service
    service:
        name: sshd
        state: restarted
    when: crond_status.rc == 0 
```

### blocks
handel errors

```yaml
tasks:
- name: itendd
  block:
    - name: rmove file
  rescue:
    - name: create a file 

  always:
    - name: always write a message
      debug:


```


run handler always]

```yaml 
hosts:
force_handlers: true
```

```
ansible-galaxy init ansible-role-name
```

<!-- &4dE2J8rC6\jr:\ -->