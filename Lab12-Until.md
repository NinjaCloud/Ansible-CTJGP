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
Since the httpd server is only installed and not  started second task would be performed 5 times
![image](https://github.com/user-attachments/assets/7b3edf6c-6c02-4e79-b154-a9ecdc71981c)

Login to any one of managed node and start the httpd service.
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
```
exit
```
Execute the playbook again.  You would notice the second task is getting performed 5 times son the other node, but on this node, it runs successfully.
![image](https://github.com/user-attachments/assets/2ea25894-a409-4902-8367-87527bd4fd91)

