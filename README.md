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

## Why use Ansible?

- Simplicity: Ansible uses a simple, human-readable YAML syntax for defining automation tasks, making it accessible to both system administrators and developers. You don't need to be a programmer to work effectively with Ansible.
- Agentless: Ansible operates in an agentless mode, which means it doesn't require any software to be installed on the target systems. This makes it easy to set up and manage.
- Idempotence: Ansible ensures idempotence, which means that applying a configuration multiple times has the same effect as applying it once. This reduces the risk of unintended changes and simplifies the automation process.

## What are Playbooks?

- Playbooks are written in YAML (Yet Another Markup Language) and are used to define a series of tasks, configurations, and automation steps that Ansible will execute on remote systems. 
- Playbooks provide a way to describe how to achieve a specific automation goal, such as configuring servers, deploying applications, or managing infrastructure.
- Playbooks are central to Ansible's configuration management and automation capabilities, allowing you to define, document, and execute complex tasks and operations on remote systems in a repeatable and consistent manner.

## What are ad-hoc commands?

- Ad-hoc commands in Ansible are one-off, single-line commands that you can use to perform quick tasks or gather information on remote hosts without the need for a separate playbook. 
- These commands are executed using the ansible command-line tool and are useful for tasks that don't require the creation of a complete playbook.

## What is YAML?

- Stands for "YAML Ain't Markup Language" or "Yet Another Markup Language" (depending on who you speak to) is a human-readable data serialization format. 
- Often used for configuration files and data exchange between languages with different data structures. 
- YAML was designed to be easily read by humans and to be expressive enough to represent complex data structures.
- Human-Readable: YAML files are written in a format that resembles natural language, making it relatively easy for humans to read and write.
- Structured Data: YAML allows you to represent structured data like lists, dictionaries (maps), and scalars in a clear and concise manner.
- Data Types: YAML supports various data types, including strings, numbers, booleans, and more. It also allows for complex data structures like lists and dictionaries.
- Comments: YAML files can include comments to provide additional information or context.

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

### Set up instances for Ansible

1) Set up 3 instances, "ansible-controller", "ansible-app", "ansible-db".
2) Run `sudo apt update` and `sudo apt upgrade-y` on all instances.
- This can be done later for the app and db instances using a Playbook for update and upgrade.
3) On "ansible-controller", follow the Ansible Install steps (or create from a previous AMI with Ansible Installed).
4) Make sure all 3 EC2 instances have the file.pem private key added to the .ssh folder (see Ansible Connection Step 3).
5) Good practice to manually connect to each instance first to check connections.

### Connecting from Ansible Controller Instance to other Instances

1) Connect to your "ansible-controller" EC2 instance the normal way (command below):
````
ssh -i YourControllerKeyName.pem ubuntu@controller-instance-ip
````
2) Give the file.pem in your "ansible-controller" instance the correct permissions:
````
cd ~/.ssh
````
````
sudo chmod 400 file.pem
````
3) Make sure you're in the .ssh folder on "ansible-controller":

4) Check that you can connect to the other instances using the file.pem SSH connection and the public IP of the instance you want to connect to from the controller. e.g.
````
sudo ssh -i "controller_file.pem" ubuntu@ec2-3-250-69-186.eu-west-1.compute.amazonaws.com
````
5) Do `uname -a` to check that you've successfully SSH'd into the required instance.
6) Type `exit` then press enter to go back to the controller instance.
7) Move to the Ansible folder in your controller:
````
cd /etc/ansible/
````
