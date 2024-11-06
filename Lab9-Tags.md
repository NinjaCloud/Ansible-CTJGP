## Tags
Tags can be used for selective execution, task grouping, and debugging in a playbook.

Create the Playbook
Each task will be tagged so that you can see how tags allow for selective task execution.

yaml
Copy code
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
Explanation of Each Task
Install Apache: This task installs Apache using the yum package manager. It is tagged with install.
Configure Apache Web Page: This task creates a basic HTML page in the Apache web server's document root. It’s tagged with configure.
Start Apache Service: This task starts the Apache service. It’s tagged with start.
Running the Playbook with Tags
Now, let’s see how we can use tags to control which tasks are executed.

1. Run Only the Installation Task
To run only the install task (installing Apache) without configuring or starting the service, use:

bash
Copy code
ansible-playbook simple_webserver.yml --tags "install"
2. Run Only the Configuration Task
To create or update the HTML page without installing or starting Apache, use:

bash
Copy code
ansible-playbook simple_webserver.yml --tags "configure"
3. Run Only the Start Task
To start the Apache service without performing installation or configuration, use:

bash
Copy code
ansible-playbook simple_webserver.yml --tags "start"
4. Run Multiple Tags Together
You can run multiple tags by separating them with a comma. For example, to install and configure Apache (but not start it), use:

bash
Copy code
ansible-playbook simple_webserver.yml --tags "install,configure"
This will execute both the install and configure tasks, skipping the start task.

5. Skip Specific Tasks Using --skip-tags
If you want to run all tasks except for the configuration task, you can use the --skip-tags option:

bash
Copy code
ansible-playbook simple_webserver.yml --skip-tags "configure"
This will install Apache and start the service but skip the HTML page configuration.

6. Start from a Specific Task Using --start-at-task
If you’ve already installed and configured Apache and just want to resume starting the service, use:

bash
Copy code
ansible-playbook simple_webserver.yml --start-at-task "Start Apache Service"
