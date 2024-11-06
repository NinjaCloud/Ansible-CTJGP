## Ansible Roles

### Task 1: Implementing Ansible Roles
```
cd ~/
```
Lets uninstall httpd. After that, we will use ansible role to install it.
```
ansible-playbook /home/ec2-user/ansible-labs/uninstall-httpd-pb.yml
```

Install tree. A tree is a recursive directory listing program that produces a depth-indented listing of files. 
```
sudo yum install tree -y
```

You can view your home directory structure in tree format with below command tree 
```
tree /home/ec2-user/ansible-labs
```
Lets create the code for Role labs
```
cd ~/
```
```
mkdir role-labs && cd role-labs
```

Now inside the roles directory, create two different directories for different roles, namely webrole and dbrole. Then switch to the directory dbrole and then create tasks directory inside dbrole
```
mkdir webrole dbrole
```
```
cd dbrole
```
```
mkdir tasks
```

This main.yml is the playbook which will get executed to make an effect of this role and put the below content in the main.yml file
```
vi tasks/main.yml
```
```
---
- name: Install MariaDB server package
  yum: 
    name: mariadb-server 
    state: present
- name: Start MariaDB Service
  service: 
    name: mariadb 
    state: started 
    enabled: true
```
**save the file using** `ESCAPE + :wq!`


Now change your directory to webrole 
```
cd .. && cd webrole/
```
```
mkdir files tasks && cd files/
```
```
vi index.html
```
```
<html>
  <body>
  <h1>We are performing the Roles Lab</h1>
  <img src= "https://d3ffutjd2e35ce.cloudfront.net/assets/logo1.png">
  </body>
</html>
```
**save the file using** `ESCAPE + :wq!`

Then go to the task directory as below and create main.yml 
```
cd .. && cd tasks/
```
```
vi main.yml
```
Add the given content, by pressing "INSERT" 
```
---
- name: install httpd
  yum: 
    name: httpd 
    update_cache: yes 
    state: latest

- name: uploading default index.html for host
  copy:
     src: /home/ec2-user/role-labs/webrole/files/index.html
     dest: /var/www/html

- name: Setting up attributes for file
  file:
    path:  /var/www/html/index.html
    owner: apache
    group: apache
    mode:  0644

- name: start httpd
  service:
    name=httpd 
    state=started
```
**save the file using** `ESCAPE + :wq!`

After the creation of this file, we are done with the complete hierarchy of roles, so we will have a look at how it is exactly using tree command
```
cd ../..
```
```
tree
```
![image](https://github.com/user-attachments/assets/ce302bb1-820a-46a5-a003-528d42a79e6c)

Now change the directory to ansible directory and create the playbook as implement-roles.yml
```
cd /home/ec2-user/ansible-labs/role-labs/
```
```
vi implement-roles.yml
```

Add the given content, by pressing "INSERT".
```
---
 - hosts: all
   become: yes 

   roles:
     - webrole
     - dbrole
```   
**save the file using** `ESCAPE + :wq!`

Execute the playbook
```
ansible-playbook implement-roles.yml
```
Check the home page on browser. (Public DNS)
It will show the webpage with msg "We are performing the Roles Lab"

----------------------------------------------------------------------------------------------
### Task 2: Installing Java through Ansible Galaxy Roles galaxy

Install java form ansible galaxy role from galaxy.ansible.com   

Now Install the role 'geerlingguy.java' from ansible galaxy repository. 
```
ansible-galaxy install geerlingguy.java
```
Now change into labs directory by running below command and create YAML file
```
cd /home/ec2-user/ansible-labs/
```
```
vi implement-java.yml
```

Add the given content, by pressing "INSERT" 
```
---
 - hosts: all
   become: yes
   roles:
     - geerlingguy.java
```
save the file using "ESCAPE + :wq!"

Before running the playbook, check if java is installed in managed nodes.
```
ansible all -m command -a "java -version"
```
you will get error
execute playbook from the control node
```
ansible-playbook implement-java.yml
```
Now, check if java is installed in managed nodes.
```
anible all -a "java -version"
```
***********************************************************************************************************************************
## Extra links and references
1. Difference between ppk and pem: https://www.c-sharpcorner.com/article/difference-between-pem-and-ppk/
2. Convert pem to ppk: https://www.youtube.com/watch?v=6OzOxjFSc90
3. DevOps skills: https://youtube.com/shorts/4sJ_1vDdeS0?si=6mjtDStfeyo688-9
4. Basic networking: https://www.youtube.com/watch?v=_IOZ8_cPgu8
5. DevOps basics: https://youtube.com/playlist?list=PLy9YqdkGQ_pkrmQudUcOmroaY4txr7UP7&si=5kuN324pr8jsv4T2
6. Cloud basics: https://youtube.com/playlist?list=PLy9YqdkGQ_plZcJl_u4S-dxB6_72kpsuS&si=pvGXPt0f37pK_NGY
