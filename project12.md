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

