## Tags
Tags can be used for selective execution, task grouping, and debugging in a playbook.

Create the Playbook. Each task will be tagged so that you can see how tags allow for selective task execution.
```
cd ~/ansible-labs
```
```
vi simple_webserver.yml
```
---
- name: Simple Apache Web Server Setup
  hosts: localhost
  become: yes
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present
      tags: install

    - name: Configure Apache Web Page
      copy:
        content: "<h1>Welcome to the Apache Server!</h1>"
        dest: /var/www/html/index.html
      tags: configure

    - name: Start Apache Service
      service:
        name: httpd
        state: started
      tags: start
```

To run only the install task (installing Apache) without configuring or starting the service, use:
```
ansible-playbook simple_webserver.yml --tags "install"
```
To create or update the HTML page without installing or starting Apache, use:
```
ansible-playbook simple_webserver.yml --tags "configure"
```
To start the Apache service without performing installation or configuration, use:
```
ansible-playbook simple_webserver.yml --tags "start"
```
You can run multiple tags by separating them with a comma. For example, to install and configure Apache (but not start it), use:
```
ansible-playbook simple_webserver.yml --tags "install,configure"
```
If you want to run all tasks except for the configuration task, you can use the --skip-tags option:
```
ansible-playbook simple_webserver.yml --skip-tags "configure"
```
If you’ve already installed and configured Apache and just want to resume starting the service, use:
```
ansible-playbook simple_webserver.yml --start-at-task "Start Apache Service"
```
