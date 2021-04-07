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




