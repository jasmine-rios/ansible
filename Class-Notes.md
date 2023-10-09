# Ansible Class Notes

## Chapter 1: Introduction and Overview

### Overview of Ansible Content

- Overview: Why, when, and where it is used and similar tools
- Setting up Environment: How to install Ansible and other tools for handson
- Tasks: Learn about adhoc commands and how to use Ansible command line tools. 
- Playbooks: Hands-on experience with writing and running playbooks for deployment, automation, and orchestration
- User Variables: Learn how to refactor Playbooks using variables and make automation more modular
- Quick Pick Ansible Topics

### What is Ansible?

It is an open source tool that enables automation, configuration, and orchestration.

#### Ansible Makes your Job Easier

- Automating deployment of applications.
- Easily manage multi-server systems.
- Make configurations one time.
- Reduces all around complexity.

#### Introduces Infrastructure as Code

- Build out entire systems in code.
- Store all code in source control.
- Roll back changes if needed.
- Share code with other team members.

#### Builds Confidents

- Produce reliable and repeatable systems.
- Ensures technologies and configurations are properly setup.
- Reduces human error and other risks.

#### Simple to Integrate 

- Simple to install and integrate into your current setup.
- Easy to learn syntax.
- Start automating configuration and infrastructure in minutes.

#### Other Key features

- Written in Python.
- Script commands using YAML syntax.
- Sends commands to nodes via SSH.
- Commands are executed sequentially on each node.
- Each node runs commands in parrallel.
- Ansible is owned by Read Hat
- Red Hat is owned by IBM.
- Ansible is owned by IBM?

### Why, When, and Where

**Mass Deployments**
Ansible helps solve the problems of manually updating servers one by one.

**Scaling**
Anible can be used as a configuration management tool to ensure identical environments when scaling to meet demands.

**Migrating Environments**
Ansible can be used as a tool for migrating applications from integration, testing, and production in a reliable and dependable way.

**Failure Prevention**
Ansible can be used as a tool for reviewing change logs and rolling back applications if failures do occur.

### Other Tools

There many Configuration Mangement Tools. 
For each tool there will be:

- **Installation Process**
    - Installed on a single control instance vs. installing agents on all machines.
- **Push vs. Pull**
    - Push based setups push out configuration changes from control machine.
    - Pull based setups constantly pull from a centralized server for configuration changes.
- Scripting Syntax
    - Wht type of syntax is used for creating and executing changes.
- Installation Requirements
    - What type of OS does the control instance need to be run on

#### Ansible

- Installation Process: Only install once.
- Push vs. Pull: Pushed Based.
- Syntax: Uses YAML syntax.
- Installation Requirements: Control machine must be Linux/Unix.

#### Salt

Salt is the name of the tool and the company that owns it is SaltStack.

- Installation Process: Must be installed on all machines.
- Push vs. Pull: Pull/Pushed based.
- Syntax: Uses YAML syntax.
- Installation Requirements: Control machine must be Linux/Unix.

#### Puppet 

- Installation Process: Must be installed on all machines.
- Push vs. Pull: Pull based.
- Syntax: Uses Puppet DSL as syntax.
- Installation Requirements: Control machine must be Linux/Unix.

#### Chef

- Installation Process: Must be installed on all machines.
- Push vs. Pull: Pull based.
- Syntax: Uses Ruby as syntax.
- Installation Requirements: Control machine must be Linux/Unix.

## Chapter 2: Setting Up Environment

### Setting Up Enviornment 

1. **Setup Inventory**
    - Create sandox servers that need to be configured and managed throughout the course using Ansible
2. **Installing Ansible**
    - Install Ansible locally on our control machine.
    - This is where we will run Ansible commands and playbooks to manage our inventory servers.
3. **Ensure End-to-End**
    - Confirm that we have Ansible set up Properly and are able to communicate with our inventory servers via SSH.

#### End Goal

- Set up simple two-tier system with multiple web servers and a single load balancer.
- Use Ansible to install packages, manage configurations, patches, etc.
- Must be able to SSH into each
- Need at least 3 VM or Physical servers to follow along

#### Steps to Set Up Environment

1. Go to course's GitHub repository and click the code drop down and choose "Download Zip".

2. Unzip to your favorite location as we will reference this code in later lessons.

### Setup Inventory

Create sandbox servers that will need to be configured and managed throughout the course using Ansible.

- Inventory, child, and node servers namming can be used interchangeably.
- These are the servers we will configure using Ansible throughout the course.
- Feel free to **skip** this setup if you already have a sandbox environment or inventory servers created and ready to use.
- You will need an AWS account if you decide to follow along

#### Steps to Setup Inventory

1. Navigate in a web broswer to the AWS console.

2. Set your region as North Virginia (us-east-1).

3. Go to EC2 Dashboard. Scroll down to Network & Security and choose Key Pairs

4. Click create key pair and name it
> ansible-course-key-pair

5. Create the key and it will download to your downloads folder. We will use this key to SSH into our servers.

6. Move your keypair into the hidden folder .ssh
In terminal do the following to do that

`mv ~/Downloads/ansible-course-key-pair.pem ~/.ssh/`

7. Change permission of the keypair to allow you to login to your servers using this key

`chmod 400 ~/.ssh/ansible-course-key-pair.pem`

8. In the AWS console, navigate to CloudFormation.

9. Click "Create Stack" and in the drop-down choose "with new resourses (standard)"

10. Choose Template is ready, choose upload a template file, then choose file.

11. Find the folder we downloaded from GitHub name "Course_Introduction_to_Ansible" and navigate to the "02_02_Introduction_To_Ansible folder.

12. Select the "setup-env.yml" and click upload.

13. In AWS Cloudformation click yes. The file will be uploaded into a S3 bucket.

14. Name the Stack.
> AnsibleCourseStack
Select the key pair we created earlier.
Enter this string for NameOfService
ansible-instances

Click next.

15. Review the changes and click Submit.

16. The Cloudstack will create the resources. Once the status is complete go to the tab Outputs. These are the IPs of our VMs CloudFormation created. 

### Install Ansible

Install Ansible locally on our control machine. This is where we will run Ansible commands and playbooks to manage our node servers.

- The control machine that we will run Ansible commands from.
- Preferably Linux/Unix based.
- You will need Python 2.x or 3.x installed.

Installing pip

`sudo easy_install pip`

Installing ansible

`sudo pip install ansible`

#### Steps to Install Ansible

1. In terminal, run the command to install brew.

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

2. Run this to install python.

`brew install python`

3. Run this to install pip3.

`brew install pip3`

4. Use pip3 to install ansible.

`sudo pip3 install ansible`

5. Check to see what ansible version was downloaded.

`ansible --version`

```sh
ansible --version
ansible [core 2.15.4]
  config file = None
  configured module search path = ['/Users/username/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /opt/homebrew/lib/python3.11/site-packages/ansible
  ansible collection location = /Users/jasminerios/.ansible/collections:/usr/share/ansible/collections
  executable location = /opt/homebrew/bin/ansible
  python version = 3.11.5 (main, Aug 24 2023, 15:09:45) [Clang 14.0.3 (clang-1403.0.22.14.1)] (/opt/homebrew/opt/python@3.11/bin/python3.11)
  jinja version = 3.1.2
  libyaml = True
```

6. Notice when running `ansible --version` there is no config file at this time. We will add one later.

### Ensure End-to-End


