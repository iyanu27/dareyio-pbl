# dareyio-pbl
DAREY.IO PROJECTS 

## Link to the github repository that consists of the playbook and inventory https://github.com/iyanu27/ansible-config-mgt.git

## PROJECT 11- Ansible Configuration Management

# What is Ansible?
 Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code. 

How does Ansible works?

Ansible works by connecting to your nodes and pushing out small programs, called "Ansible modules" to them. Ansible then executes these modules (over SSH by default), and removes them when finished. Your library of modules can reside on any machine, and there are no servers, daemons, or databases required.

## Step 1 - Set up an Ansible EC2 Instance
 1. Update Name tag on your Jenkins server to Jenkins-Ansible. We will use this server to run playbooks.
 2. In your GitHub account create a new repository and name it ansible-config-mgt.
 
 ![image](https://user-images.githubusercontent.com/57386428/113287377-5c8c7c00-92a2-11eb-822f-9b9fdacc2e86.png)
  
  3. Install Ansible with the following command:
  
  curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
  
  sudo python3 get-pip.py
  
  sudo python3 -m pip install ansible
  
  Check your Ansible version by running ansible --version
   ![image](https://user-images.githubusercontent.com/57386428/113287727-cdcc2f00-92a2-11eb-918d-7359c8984134.png)

  4. Configure Jenkins build job to save your repository content every time you change it
  
      a. Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.
      ![image](https://user-images.githubusercontent.com/57386428/113287908-0e2bad00-92a3-11eb-9cc3-d1b3450251da.png)
      b. Configure Webhook in GitHub and set webhook to trigger ansible build.
      
         To get the Hook URL of Jenkins, Open the Jenkins Dashboard.
         
         Go to: Manage Jenkins > Configure System
         
        ![image](https://user-images.githubusercontent.com/57386428/113288169-711d4400-92a3-11eb-82bd-ff966ad36438.png)

      Under GitHub Plugin Configuration, Click on ‘Advanced' tab. 
      
      Open your repository on GitHub.
      
      Click ‘Settings’ on the navigation bar on the right-hand side of the screen.
      
      Click ‘Webhooks ’ on the navigation bar on the left-hand side of the screen.
      
      Click ‘Add webhook’ to add the webhook. 
     ![image](https://user-images.githubusercontent.com/57386428/113288550-f43e9a00-92a3-11eb-9b48-063d3f232389.png)
    
     You should now see the webhook you just added in the list of Webhooks for that repository.
   C. Configure a Post-build job to save all (**) files
   ![image](https://user-images.githubusercontent.com/57386428/113288826-4b446f00-92a4-11eb-9bca-49fc4a25e2c3.png)
  D. Lets test  the setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files     (build artifacts) in following folder.
  
  Before making changes to the Readme.md file there was no build as shown below:
  ![image](https://user-images.githubusercontent.com/57386428/113289012-7f1f9480-92a4-11eb-9737-4c5e6e372fdf.png)
  
  After making changes to the Readme.md file,the build was done automatically as shown below:
  ![image](https://user-images.githubusercontent.com/57386428/113289100-9d859000-92a4-11eb-8929-bf2d4f025bb0.png)

  To make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder : 
  
  /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
      Install the Copy Artifact plugin on jenkins
  ![image](https://user-images.githubusercontent.com/57386428/113289279-db82b400-92a4-11eb-882c-e30a9b542ff6.png)
      click on build step and click on copy artifacts from another project
  ![image](https://user-images.githubusercontent.com/57386428/113289428-0e2cac80-92a5-11eb-9269-3e52516c74ef.png)
   
![image](https://user-images.githubusercontent.com/57386428/113289485-23094000-92a5-11eb-9936-2aa8f523b845.png)
![image](https://user-images.githubusercontent.com/57386428/113290269-41237000-92a6-11eb-82b5-1ea5ad67cef6.png)
  Then save and apply
  
## Step 2 - Prepare your development environment using Visual Studio Code
  a. Download Visual studio code
  
  b. configure it to connect to your newly created GitHub repository.
  ![image](https://user-images.githubusercontent.com/57386428/113291059-4f25c080-92a7-11eb-839b-86aa1bc07f79.png)
  
  Step 3 - Begin Ansible Development
  
  a. Checkout the newly created feature branch() to your local machine and start building your code and directory structure
  
  b. Use the following command to checkout: “git checkout -b feature/ansible-project”
  
![image](https://user-images.githubusercontent.com/57386428/113291273-a4fa6880-92a7-11eb-9cf2-af766778af80.png)
 c. Create a directory and name it playbooks - it will be used to store all your playbook files
 
 ![image](https://user-images.githubusercontent.com/57386428/113291653-2e119f80-92a8-11eb-93e5-7147dab0ed47.png)

 d. Within the playbooks folder, create your first playbook, and name it common.yml
 ![image](https://user-images.githubusercontent.com/57386428/113292453-64035380-92a9-11eb-9409-89440643cf73.png)
 
 e. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production)
 
![image](https://user-images.githubusercontent.com/57386428/113293255-64501e80-92aa-11eb-9cdb-57cf3392ba64.png)

## Step 4 - Set up an Ansible Inventory
 NOTE: I will be testing this playbook with 2 virtual machines due to the limitation on my Azure account but it can applied to more than 2 Virtual machines


An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.
Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.
  a. Update your inventory/dev.yml file with this snippet of code :
  ![image](https://user-images.githubusercontent.com/57386428/113294615-13d9c080-92ac-11eb-8f5e-542d5b28cecb.png)
  
  b. To confirm if our dev.yml is correct use the command
   ansible-inventory -i dev.yml --list
   ![image](https://user-images.githubusercontent.com/57386428/113295213-c3af2e00-92ac-11eb-8822-1074f1179a14.png)
   
  c. To confirm if ansible can ping the hosts use the command :
  
    ansible all -i dev.yml -m ping
![image](https://user-images.githubusercontent.com/57386428/113295345-f3f6cc80-92ac-11eb-9f48-bd86b6b264b6.png)

d. I got this error as shown above,to resolve this issue by adding --ask pass to the command 
    ansible all -i dev.yml -m ping --ask-pass
    
![image](https://user-images.githubusercontent.com/57386428/113295626-4c2dce80-92ad-11eb-9e29-7ef2948d0d03.png)

e. After this is successful for one of the servers will replicate it for others as shown below:
![image](https://user-images.githubusercontent.com/57386428/113295758-73849b80-92ad-11eb-92fb-a64803de186c.png)
![image](https://user-images.githubusercontent.com/57386428/113295814-85663e80-92ad-11eb-85bc-ad7eda0eae57.png)

## Step 5 - Create a Common Playbook
It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.

In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

Update your playbooks/common.yml file with following code:
![image](https://user-images.githubusercontent.com/57386428/113295949-b5addd00-92ad-11eb-9ca1-5a37eec25269.png)

## Step 6 - Update GIT with the latest code

Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT.

In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes - it is also called “Four eyes principle”.

Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.
Commit your code into GitHub:
 ![image](https://user-images.githubusercontent.com/57386428/113296118-ebeb5c80-92ad-11eb-8514-3af660a69ed2.png)
 ![image](https://user-images.githubusercontent.com/57386428/113296902-e2162900-92ae-11eb-9f0c-d396a48389bc.png)
![image](https://user-images.githubusercontent.com/57386428/113296996-f78b5300-92ae-11eb-9728-df734e58e7eb.png)

Create a Pull request (PR)

Wear a hat of another developer for a second, and act as a reviewer.

If the reviewer is happy with your new feature development, merge the code to the master branch.

Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes

![image](https://user-images.githubusercontent.com/57386428/113297151-21dd1080-92af-11eb-99cc-8a4dd44752f1.png)

Once the pull request has been done,let us merge the request
![image](https://user-images.githubusercontent.com/57386428/113297317-50f38200-92af-11eb-962d-753278a92db1.png)

## Step 7 - Run first Ansible test
 Now, it is time to execute ansible-playbook command and verify if your playbook actually works:
 sudo ansible-playbook -i dev.yml /home/Iyanu/playbooks/common.yml --ask-become-pass
 ![image](https://user-images.githubusercontent.com/57386428/113297478-8304e400-92af-11eb-8a84-ebb5dcc59cc0.png)
b. You can go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version
![image](https://user-images.githubusercontent.com/57386428/113297583-a16adf80-92af-11eb-965a-86deae3877d9.png)

c. To check if our directory is being created
![image](https://user-images.githubusercontent.com/57386428/113297661-bd6e8100-92af-11eb-99ce-469fdfbcd81e.png)

Congratulations
You have just automated your routine tasks by implementing your first Ansible project! There is more exciting projects ahead, so lets keep it moving!
 
Credits:

https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04

Darey.io

Ansible Documentation — Ansible Documentation

What is Ansible and How To Use Ansible in Docker? (simplilearn.com)

How to Merge Branches and Pull Requests in GitHub (zepel.io)








