## Run Once with Ansible Playbook
Create and edit rolab.yml in the same labs directory
```
cd ~ansible-labs
```
```
vi rolab.yml
```
```
---
- hosts: all
  become: yes
  user: ec2-user
  connection: ssh
  gather_facts: no
  tasks:
    - name: Recording uptime 
      raw: /usr/bin/uptime >> /home/ec2-user/uptime
      run_once: true
```
save the file using ESCAPE + :wq!

Execute the playbook
```
ansible-playbook rolab.yml
```
Verify if the file exists and has the right contents on either of the client machines(manage nodes)
```
ansible all -a "cat /home/ec2-user/uptime"
```
Now open the file and edit parameter as run_once: false Execute the playbook again
```
ansible-playbook rolab.yml
```
Verify if the file exists and has the right contents on the client machines(manage nodes)

ansible all -a "cat /home/ec2-user/uptime"
