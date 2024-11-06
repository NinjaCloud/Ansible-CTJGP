## Until function
Create and edit untillab.yml in the same labs directory
```
cd ~/ansible-labs
```
```
vi untillab.yml
```
```
---
- hosts: all
  become: yes
  connection: ssh
  user: ec2-user
  tasks:
  - name: Install Apache Web Server
    yum:
       name: httpd
       state: latest
  - name: Verify Status of Service
    shell: systemctl status httpd
    register: result
    until: result.stdout.find("active (running)") != -1
    retries: 5
    delay: 10
```
save the file using "ESCAPE + :wq!"

Execute the playbook Notice the output of the command is shown along with the Ansible until command output
```
ansible-playbook untillab.yml
```
Login to the managed node from another window and start the httpd service You can use the same key (as used for CN) to login to managed node
```
ssh ec2-user@< managed_node_private_ip >
```
```
sudo service httpd start
```
You can check the status of httpd by
```
sudo service httpd status
```
