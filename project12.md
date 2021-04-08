# Project 12
## Ansible Refactoring & Static Assignments (Imports and Roles)
====================================================

In this project you will continue working with ansible-config-mgt repository and make some improvements of your code. Now you need to refactor your Ansible code, create assignments, and learn how to use the imports functionality. Imports allow to effectively re-use previously created playbooks in a new playbook - it allows you to organize your tasks and reuse them when needed


## Code Refactoring

Refactoring is a general term in computer programming. It means making changes to the source code without changins expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

## Step 1 - Jenkins job enhancement

Go to your Jenkins-Ansible server and create a new directory called ansible-config-mgt - we will store there all artifacts after each build.

    mkdir /home/Iyanu/ansible-config-mgt
  ![image](https://user-images.githubusercontent.com/57386428/113708908-b0c6a000-9696-11eb-946c-35cb660d5d96.png)
Change permissions to this directory, so Jenkins could save files there -
    chmod -R 0777 /home/Iyanu/ansible-config-mgt
  ![image](https://user-images.githubusercontent.com/57386428/113709086-f1261e00-9696-11eb-81be-dfc34db17273.png)
Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins
  ![image](https://user-images.githubusercontent.com/57386428/113709397-47935c80-9697-11eb-8979-bc37d9b8d833.png)
 Create a new Freestyle project (you have done it in Project 9) and name it save_artifacts.
  ![image](https://user-images.githubusercontent.com/57386428/113709653-9ccf6e00-9697-11eb-897a-0634706c9772.png)

 This project will be triggered by completion of your existing ansible project. Configure it accordingly:
 ![image](https://user-images.githubusercontent.com/57386428/113709766-bec8f080-9697-11eb-8a3c-ca77cd5ab4c9.png)
 
 The main idea of save_artifacts project is to save artifacts into /home/Iyanu/ansible-config-mgt directory.
 
 To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/Iyanu/ansible-config-mgt as a target directory.
 ![image](https://user-images.githubusercontent.com/57386428/113710088-21ba8780-9698-11eb-913c-28c97cb467fb.png)
 
 Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).
 ![image](https://user-images.githubusercontent.com/57386428/113712403-ed949600-969a-11eb-91c1-ae2f36f0b8fd.png)
![image](https://user-images.githubusercontent.com/57386428/113712479-04d38380-969b-11eb-93e6-775f6d1c42b6.png)

After  both Jenkins jobs have completed one after another - you shall see your files inside /home/Iyanu/ansible-config-mgt directory and it will be updated with every commit to your master branch.

![image](https://user-images.githubusercontent.com/57386428/113712586-2af92380-969b-11eb-9359-22453b451bdd.png)
 Now our Jenkins pipeline is more neat and clean.
 
 ## Step 2 - Refactor Ansible code by importing other playbooks into
 
 Before starting to refactor the codes, ensure that you have pulled down the latest code from master (main) branch, and created a new branch, name it refactor.
 
 ![image](https://user-images.githubusercontent.com/57386428/113722799-9cd66a80-96a5-11eb-9edf-0a6a6467ad96.png)
 
Run ansible-playbook command against the dev environment

Since you need to apply some tasks to your dev servers and wireshark is already installed - you can go ahead and create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.
![image](https://user-images.githubusercontent.com/57386428/113723802-8f6db000-96a6-11eb-928a-31ab196a7dc9.png)
![image](https://user-images.githubusercontent.com/57386428/113724026-c9d74d00-96a6-11eb-99ab-8ea575010bb6.png)

Got this error while trying to run the playbook using the command below:
sudo ansible-playbook -i dev.yml /home/Iyanu/ansible-config-mgt/playbooks/site.yml -kK

![image](https://user-images.githubusercontent.com/57386428/113726552-2e93a700-96a9-11eb-9605-3971c2e1998b.png)

How to resolve this issue :
 a.Changed the state for the task: delete wirkeshark to "absent" as shown below:
 
 ![image](https://user-images.githubusercontent.com/57386428/113727868-651df180-96aa-11eb-92a4-c2b3a3ab5805.png)

Final result shows the playbook ran successfully 

![image](https://user-images.githubusercontent.com/57386428/113727978-841c8380-96aa-11eb-810a-6d35e9b9fc91.png)

Make sure that wireshark is deleted on all the servers by running wireshark --version

Now you have learned how to use import_playbooks module and you have a ready solution to install/delete packages on multiple servers with just one command.

## Step 3 - Configure UAT Webservers with a role ‘Webserver’

We have our nice and clean dev environment, so let us put it aside and configure 2 new Web Servers as uat. We could write tasks to configure Web Servers in the same playbook, but it would be too messy, instead, we will use a dedicated role to make our configuration reusable.

To create a role, you must create a directory called roles/, relative to the playbook file or in /etc/ansible/ directory.
There are two ways how you can create this folder structure:

1. Use an Ansible utility called ansible-galaxy inside ansible-config-mgt/roles directory (you need to create roles directory upfront)

![image](https://user-images.githubusercontent.com/57386428/113934238-59b1ef80-97aa-11eb-93e3-3ba85360afca.png)
After removing unnecessary directories and files, the roles structure should look like this
![image](https://user-images.githubusercontent.com/57386428/113934571-c88f4880-97aa-11eb-91f3-5789eb0b87de.png)
2. Update your inventory ansible-config-mgt/inventory/uat.yml file with IP addresses of your 2 UAT Web servers
![image](https://user-images.githubusercontent.com/57386428/113933051-fa071480-97a8-11eb-80e9-9c879826be07.png)
3. In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path = /home/Iyanu/ansible-config-mgt/roles, so Ansible could know where to find configured roles.
![image](https://user-images.githubusercontent.com/57386428/113935642-9d592900-97ab-11eb-85f1-0a331887f438.png)
4.It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:
    Install and configure Apache (httpd service)
    Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.
    Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
    Make sure httpd service is started
 ![image](https://user-images.githubusercontent.com/57386428/113936605-da71eb00-97ac-11eb-8425-e1b1123b3e54.png)
    
## Step 4 - Reference ‘Webserver’ role
1. Within the static-assignments folder, create a new assignment for uat-webservers uat-webservers.yml. This is where you will reference the role.

![image](https://user-images.githubusercontent.com/57386428/113942302-6ee04b80-97b5-11eb-9a72-f7c2469989be.png)

2. Remember that the entry point to our ansible configuration is the site.yml file. Therefore, you need to refer your uat-webservers.yml role inside site.yml.

So, we should have this in site.yml

![image](https://user-images.githubusercontent.com/57386428/113943116-da76e880-97b6-11eb-9737-bb602c4f2306.png)

## Step 5 - Commit & Test

Commit your changes, create a Pull Request and merge them to master branch, make sure webhook triggered two consequent Jenkins jobs, they ran successfully and copied all the files to your Jenkins-Ansible server into /home/Iyanu/ansible-config-mgt/ directory.

Now run the playbook against your uat inventory and see what happens:








