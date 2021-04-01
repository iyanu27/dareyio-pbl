# dareyio-pbl
DAREY.IO PROJECTS

Ansible Configuration Management
 What is Ansible?
Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code. 

How does Ansible works?


Step 1 - Set up an Ansible EC2 Instance
Update Name tag on your Jenkins server to Jenkins-Ansible. We will use this server to run playbooks.
In your GitHub account create a new repository and name it ansible-config-mgt.

Install Ansible with the following command:
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python3 get-pip.py
sudo python3 -m pip install ansible
Check your Ansible version by running ansible --version

 
4. Configure Jenkins build job to save your repository content every time you change it
Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.

Configure Webhook in GitHub and set webhook to trigger ansible build.
         To get the Hook URL of Jenkins, Open the Jenkins Dashboard.
Go to: Manage Jenkins > Configure System
Under GitHub Plugin Configuration, Click on ‘Advanced' tab. 


Open your repository on GitHub.
Click ‘Settings’ on the navigation bar on the right-hand side of the screen.
Click ‘Webhooks ’ on the navigation bar on the left-hand side of the screen.
Click ‘Add webhook’ to add the webhook. 

You should now see the webhook you just added in the list of Webhooks for that repository.
 
Configure a Post-build job to save all (**) files

Lets test  the setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder.
Before making changes to the Readme.md file there was no build as shown below:

After making changes to the Readme.md file,the build was done automatically as shown below:

To make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder : /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
  i.  Install the Copy Artifact plugin on jenkins

 
 Ii.     click on build step and click on copy artifacts from another project


Then save and apply
Step 2 - Prepare your development environment using Visual Studio Code
Download Visual studio code
configure it to connect to your newly created GitHub repository.

Step 3 - Begin Ansible Development

Checkout the newly created feature branch() to your local machine and start building your code and directory structure
Use the following command to checkout: “git checkout -b feature/ansible-project”

Create a directory and name it playbooks - it will be used to store all your playbook files.
Create a directory and name it inventory - it will be used to keep your hosts organised.

d.Within the playbooks folder, create your first playbook, and name it common.yml

E. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production)

 
Step 4 - Set up an Ansible Inventory
NOTE: I will be testing this playbook with 2 virtual machines due to the limitation on my Azure account but it can applied to more than 2 Virtual machines


An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.
Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.
Update your inventory/dev.yml file with this snippet of code :

To confirm if our dev.yml is correct use the command 
ansible-inventory -i dev.yml --list

To confirm if ansible can ping the hosts use the command :
ansible all -i dev.yml -m ping

I got this error as shown above,to resolve this issue by adding --ask pass to the command 
   ansible all -i dev.yml -m ping --ask-pass

After this is successful for one of the servers will replicate it for others as shown below:


Step 5 - Create a Common Playbook
It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.
In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.
Update your playbooks/common.yml file with following code:

 
Step 6 - Update GIT with the latest code
Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.
In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT. In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes - it is also called “Four eyes principle”.
Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.
Commit your code into GitHub:




Create a Pull request (PR)
Wear a hat of another developer for a second, and act as a reviewer.
If the reviewer is happy with your new feature development, merge the code to the master branch.
Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes

Once the pull request has been done,let us merge the request

 
 
 
 
 
 
 
Step 7 - Run first Ansible test
Now, it is time to execute ansible-playbook command and verify if your playbook actually works:
 sudo ansible-playbook -i dev.yml /home/Iyanu/playbooks/common.yml --ask-become-pass

B. You can go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version

 
 
To check if our directory is being created

 
Congratulations
You have just automated your routine tasks by implementing your first Ansible project! There is more exciting projects ahead, so lets keep it moving!
 
Credits:
https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04
Darey.io
Ansible Documentation — Ansible Documentation
What is Ansible and How To Use Ansible in Docker? (simplilearn.com)
How to Merge Branches and Pull Requests in GitHub (zepel.io)

