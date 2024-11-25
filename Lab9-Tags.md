## Tags
Tags can be used for selective execution, task grouping, and debugging in a playbook.

Create the Playbook. Each task will be tagged so that you can see how tags allow for selective task execution.
```
cd ~/ansible-labs
```
```
vi tags.yaml
```
```
---
- name: Task Demonstration
  hosts: localhost
  become: yes
  tasks:
    - name: Check if target directory exists
      stat:
        path: /tmp/target_directory
      register: target_dir
      tags: check_directory

    - name: Create directory if not present
      file:
        path: /tmp/target_directory
        state: directory
        mode: '0755'
      when: not target_dir.stat.exists
      tags: create_directory

    - name: Copy sample file if directory exists
      copy:
        content: "This is a sample file content."
        dest: /tmp/target_directory/sample_file.txt
      when: target_dir.stat.exists
      tags: copy_file

    - name: Check if user 'exampleuser' exists
      command: id -u exampleuser
      register: user_exists
      ignore_errors: yes
      tags: check_user

    - name: Create user 'exampleuser' if not exists
      user:
        name: exampleuser
        state: present
      when: user_exists.rc != 0
      tags: create_user
```

Run Task with check_directory Tag
```
ansible-playbook tags.yaml --tags "check_directory"
```
Run Task with create_directory Tag
```
ansible-playbook tags.yaml --tags "create_directory"
```
Run Task with copy_file Tag
```
ansible-playbook tags.yaml --tags "copy_file"
```
Run Task with check_user Tag
```
ansible-playbook tags.yaml --tags "check_user"
```
Run Task with create_user Tag
```
ansible-playbook tags.yaml --tags "create_user"
```
Run Multiple Tags Together
You can run multiple tasks by combining tags in a comma-separated list.
```
ansible-playbook tags.yaml --tags "create_directory,copy_file"
```
Run check_directory, create_directory, and check_user Tasks
```
ansible-playbook tags.yaml --tags "check_directory,create_directory,check_user"
```
You can also skip specific tasks by using the --skip-tags option. This is useful if you want to run all tasks except certain ones.
```
ansible-playbook tags.yaml --skip-tags "check_user"
```
If you want to skip the first task (Check if target directory exists) and start from the Create directory if not present task, use:
```
ansible-playbook tags.yaml --start-at-task
```
