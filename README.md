# Infrastructure as Code

## What is Infrastructure as Code (IaC)?

- Infrastructure as Code (IaC) is a software engineering approach to managing and provisioning infrastructure and its configurations through machine-readable code or scripts.

## What is Configure Management?

- Configuration management, often abbreviated as CM, is a set of practices and processes used in IT and systems engineering to manage and control the configuration of software, hardware, and infrastructure components in a systematic and consistent manner.
- The primary goal of configuration management is to ensure that systems and software remain stable, reliable, and predictable throughout their lifecycle.

## What is Orchestration?

- Orchestration is the coordination and automation of multiple tasks, processes, or operations to achieve a specific outcome or goal. 
- It is often used in the context of managing complex workflows, especially in IT, cloud computing, and application deployment. 
- Orchestration involves managing the sequence, timing, and dependencies of various tasks to ensure they work together harmoniously.

## What is Ansible?

![Alt text](<Images/1. Ansible diagram.jpg>)

- Ansible is an open-source automation tool designed to simplify and automate various IT tasks, including configuration management, application deployment, task automation, and orchestration. 
- It allows you to define and manage infrastructure as code, making it easier to provision and configure systems and applications consistently. 
- Ansible is known for its simplicity, agentless architecture, and the use of human-readable YAML files for defining automation tasks

## Ansible Connection on EC2

### Step 1 - Make an EC2 Instance on AWS

1) Go to the AWS Console and make a new EC2 instance.
- Ubuntu 20.04 lts.
- t2 micro
- `file.pem` SSH key pair
- Base Security group (22, 80, 3000)
2) Connect to your instance on GitBash.
3) Run:
````
sudo apt update
````
````
sudo apt upgrade -y
````

### Step 2 - Install Ansible

1) Check if you have ansible installed:
````
ansible --version
````
2) Run the following commands:
````
sudo apt install software-properties-common
````
````
sudo apt-add-repository ppa:ansible/ansible
````
3) Press "Enter"
4) Run update again:
````
sudo apt update -y
````
5) Install Ansible:
````
sudo apt install ansible -y
````
6) Check Ansible version again to see if its installed:
````
ansible --version
````
7) cd into the ansible folder:
````
cd /etc/ansible/
````
8) Make sure the correct Ansible files are there.

**NOTE**: Run the following commands to view files in a folder in a better way:
````
sudo apt install tree
````
```` 
tree
````

### Step 3 - Make sure the SSH key has been added to .ssh folder in Ubuntu

1) cd into the .ssh folder on Ubuntu
````
cd ~/.ssh
````
2) Open up a new GitBash terminal connected to your local OS and run the following command to copy the .pem file SSH key to Ubuntu:
````
scp -i "~/.ssh/tech254.pem" ~/.ssh/tech254.pem ubuntu@<Public Instance IP>:~/.ssh
````

## Connecting from Ansible Controller Instance to other Instances


1) Connect to your "ansible-controller" EC2 instance the normal way (command below):
````
ssh -i YourControllerKeyName.pem ubuntu@controller-instance-ip
````
2) Make sure each EC2 instance has the file.pem private key added to the .ssh folder (see Ansible Connection Step 3).
3) Input the following command using the name of the file.pem private key within the Target Instance you're connecting to plus the PRIVATE IP of that instance.
````
ssh -i YourTargetKeyName.pem ubuntu@target-instance-private-ip
````