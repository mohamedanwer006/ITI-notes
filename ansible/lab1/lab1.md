# lab1 


## check ssh
#### on local machine, create ssh key

```
ssh-keygen 

echo id_rsa.pub >> ~/.ssh/authorized_keys

cat id_rsa.pub

``` 


#### ssh on the remote machine and create user ansible and add public key to authorized_keys


```
sudo useradd ansible
su  ansible
mkdir ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

echo "-----BEGIN OPENSSH PRIVATE KEY-----" >> ~/.ssh/authorized_keys

```
> try to ssh to the remote machine


### on local
```
mkdir -p ~/ansible/lab1
cd ~/ansible/lab1

touch ansible.cfg
touch inv

```

###  ansible.cfg 

```md

[defaults]
inventory = ./inv
remote_user = ansible
ask_pass = false
roles_path = ./roles
host_key_checking = False

[privilege_escalation]
become = true
become_method = sudo
become_user = root
become_ask_pass = false

```

### inv
```md
[lab1]
192.168.10.20
```


-----

## install httpd

```
ansible-vault decrypt --vault-password-file=~/vault_key lab.yaml

```
create `lab.yaml`file
```yaml
 - name: first palybook
  hosts: lab1
  tasks:
    - name: install httpd
      yum:
        name: httpd
        state: latest
    - name : ensure httpd is running
      service:
        name: httpd
        state: started
        enabled: yes
    - name: Write backup script for each app
      shell: |
        echo 'ITI 2022' > /var/www/html/index.html
  # copy
    - name: httpd restart
      ansible.builtin.service:
        name: httpd
        state: restarted
        enabled: true
```

run the playbook

```
ansible-playbook lab.yaml
```

### vault_key
#### create random password

```
echo "radf#$msals534dfkdaag" > ~/.vault_key
```
### encrypt the lab.yaml file
```
ansible-vault encrypt lab.yaml --vault-password-file ~/.vault_key
```
### run playbook
```
 ansible-playbook lab.yaml --vault-password-file ~/.vault_key
```
### decrypt the lab.yaml file
```
ansible-vault decrypt lab.yaml --vault-password-file ~/.vault_key
```
