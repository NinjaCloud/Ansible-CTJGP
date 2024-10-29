## Lab : Exploring Ad-Hoc Commands

```
sudo vi /etc/ansible/hosts
```

Add the given line, by pressing "INSERT" 
add localhost and add the connection as local so that it wont try to use ssh
```
localhost ansible_connection=local
```
**save the file using** `ESCAPE + :wq!`

In real life situations, one of the managed node may be used as the ansible control node. In such cases, we can make it a managed node, by adding localhost in hosts inventory file.

#### Get memory details of the hosts using the below ad-hoc command
```
ansible all -m command -a "free -h"
```
![image](https://github.com/user-attachments/assets/8f1fd295-a9df-4356-9b4e-99ee1e68b97a)



```
ansible all -a "free -h"
```
![image](https://github.com/user-attachments/assets/a892b7b1-218a-4dc9-b029-6f62d5bc982b)



Create a user ansible-new in the 2 nodes + the control node. This creates the new user and the home directory /home/ansible-new
```
ansible localhost -m user -a "name=ansible-test" --become
```
![image](https://github.com/user-attachments/assets/db7a27a6-9372-416f-93eb-7b8894e1b87a)



Lists all users in the machine. Check if ansible-new is present in the managed nodes / localhost
```
ansible localhost -a "cat /etc/passwd"
```
![image](https://github.com/user-attachments/assets/a075beb0-8202-4eba-9a08-54a58e38eafe)



List all directories in /home. Ensure that directory 'ansible-new' is present in /home. 
```
ansible managed-node2 -a "ls /home"
```
![image](https://github.com/user-attachments/assets/69d6a999-fa0d-42c9-b21f-7e749337e7b6)



Change the permission mode from '700' to '755' for the new home directory created for ansible-new
```
ansible managed-node1 -m file -a "dest=/home/ansible-new mode=755" --become
```
![image](https://github.com/user-attachments/assets/b342a740-592f-4411-bce0-8d00319a8e5c)



Check if the permissions got changed
```
ansible managed-node1 -a "sudo ls -l /home"
```
![image](https://github.com/user-attachments/assets/5ecb2682-7a73-4a11-aad4-308c0d14b1d3)



Create a new file in the new dir in node 1
```
ansible managed-node1 -m file -a "dest=/home/ansible-new/demo.txt mode=600 state=touch" --become
```
![image](https://github.com/user-attachments/assets/3d0b1f59-bfb6-4dda-8b0b-118cd5e25919)



Check if the permissions got changed
```
ansible managed-node1 -a "sudo ls -l /home/ansible-new/"
```
![image](https://github.com/user-attachments/assets/506242bc-6493-4a78-808f-e7fcfd959277)




Add content into the file
```
ansible managed-node1 -b -m lineinfile -a 'dest=/home/ansible-new/demo.txt line="This server is managed by Ansible"'
```
![image](https://github.com/user-attachments/assets/ac3389fa-9351-4cc2-ba77-6a3d5f06ad14)


Check if the lines are added in demo.txt
```
ansible managed-node1 -a "sudo cat /home/ansible-new/demo.txt"
```
![image](https://github.com/user-attachments/assets/2434ab18-1649-4ed6-b26e-d4c4cc2b4340)



You can remove the line using the parameter state=absent
```
ansible managed-node1 -b -m lineinfile -a 'dest=/home/ansible-new/demo.txt line="This server is managed by Ansible" state=absent'
```
![image](https://github.com/user-attachments/assets/17caee3f-9e82-48ee-9688-f828b2ccf53a)



Check if the lines are removed from demo.txt
```
ansible managed-node1 -b -a "sudo cat /home/ansible-new/demo.txt"
```
![image](https://github.com/user-attachments/assets/aab8e28d-cf9e-44f8-bd68-63977b194737)



Now copy a file from ansible-control node to host managed-node1
```
touch test.txt
```
```
echo "This file will be copied to managed node using copy module" >> test.txt
```
Now Copy. --become can be replaced by -b
```
ansible managed-node1 -m copy -a "src=test.txt dest=/home/ansible-new/test" -b 
```
![image](https://github.com/user-attachments/assets/f54b80d3-616d-41ff-9b13-4562b79327af)


Check if the file got copied to managed node.
```
ansible managed-node1 -b -a "sudo ls -l /home/ansible-new/test"
```
![image](https://github.com/user-attachments/assets/259fd58a-070b-42f4-adb7-e1ccf599a72a)

```
sudo vi /etc/ansible/hosts
```

Remove the below line from hosts inventory file. 
localhost ansible_connection=local
![image](https://github.com/user-attachments/assets/710199b0-d722-4086-87a1-fa2e88071c72)


**save the file using** `ESCAPE + :wq!`
