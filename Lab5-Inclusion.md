## Inclusion

Create a file named second.yml with below contents which has a list of tasks to install and start httpd (apache) service
```
vi tasks.yml
```
```
---
  - name: install the httpd package
    yum:
      name: httpd
      state: latest
      update_cache: yes

  - name: start the httpd service
    service:
      name: httpd
      state: started
      enabled: yes
```
**save the file using** `ESCAPE + :wq!`


Create another playbook named first.yaml, which has an inclusion for the earlier created task.
```
vi first.yml
```
```
---
- hosts: all
  gather_facts: no
  become: yes
  tasks:
  - name: install common packages
    yum:
      name: [wget, curl]
      state: present

  - name: inclue task for httpd installation
    include_tasks: tasks.yml
 ``` 

**save the file using** `ESCAPE + :wq!`
Execute the playbook named first.yaml using below command
```
ansible-playbook first.yml
```

Create another playbook named second.yaml as below
```
vi second.yml
```
```
---
- hosts: all
  gather_facts: no
  become: yes
  tasks:
    - name: install common packages
      yum:
        name: [wge, url]
        state: present
      register: out
      ignore_errors: yes  # This will ensure the playbook continues even if the task fails

    - name: list result of previous task
      debug:
        msg: "{{ out.rc }}"

    - name: include task for httpd installation
      include_tasks: tasks.yml
      when: out.rc != 0

```
Execute the playbook second.yaml
```
ansible-playbook second.yml
```

verify the installed packages on managed node by following commands#
```
ansible all -m command -a "yum list wget curl" -b
```
