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

Confirm that we have ansible set up properly and are able to communicate with our child server via SSH.

- Last step is to ensure we have communication to our inventory servers
- Simply SSH into one (or all) of your instances and confirm
- You are all set
- Mac/Linux setup first

#### SSH using terminal

1. Go to terminal and do
`ssh -i ~/.ssh/ansible-course-key-pair.pem ec2-user@<IP address>`

The IP address is found on Cloudformation in the outputs section

2. Type yes when asked to remember fingerprint.

3. Repeat for other EC2 instances.

### Setup Inventory File

Before running our first commands, we need to give Ansible a refrence to our inventory servers we want to execute commands on.

- Create an **inventory file** which contains your inventory server information. This file lists the hostnames and groups
e.g.
```INI
# host-dev

[webservers] #groups
app1.site.com #host names
app2.site.com

[loadbalancers]
lb.site.com

```

- Can be static file or dynamic.
- Can be INI-like or YAML format

- The inventory file can include inventory specific parameters
e.g.

```INI
# hosts-dev

[webservers]
weirdo.site.com:5309 # Non standard SSH Port
app2.site.como

[loadbalancers]
lb ansible_host=12.34.56.78 # Create aliases
```
- Default Ansible inventory is located in the /etc/ansible/hosts
- Reference a diffrent inventory by using the -i <path> option in the command line.

Follow along in the 0206 folder for ansible.

#### Create Inventory file in Visual Studio Code

1. Open up Visual Studio Code. This will be used in the rest of the course.

2. Create a new file called "hosts-dev"

3. Put the first line as `# hosts-dev`

4. Add the groups `[webservers]`, `[loadbalancers]`, `[local]`

5. `[local]` is needed for ansible to work under that add `control`

6. Paste in the IP address associated with `[webservers]` and `[loadbalancers]` under the correct groups. 
Remember these are found in outputs in our CloudFormation stack. 

7. In terminal in VSCode, do this command

`ansible --list-hosts all`

It will return that it doesn't see the inventory because we didn't specify the file when we created ansible.

8. Do this to point to the inventory file for ansible to use

`ansible -i ansible/hosts-dev --list-host all`

This returns all the hosts in our inventory file.

9. As we run ansible commands it will attempt to SSH into our hosts in our inventory file. We need to tell it not to for local.
Next to control add

`ansible_connection=local`

10. Save the hosts-dev file and run the same command again. 
Nothing changes when running it again.

### Ansible Configuration

Often we have the need to configure our local Ansible environment with global specific properties associated with out setup.

- Creating a **configuration file** (ansible.cfg) to control your local Ansible environment settings.
e.g.

```INI
[defaults] # default settings
inventory = ./hosts-dev # inventory file
...
...

```

- The configuration file can include environment-specific parameters to global Ansible commands ran.

- Configuration file will be searched in the following order:

    `ANSIBLE_CONFIG` (environment variable if set)

    `ansible.cfg` (in the current directory)

    `~/.ansible.cfg` (in the home directory)

    `/etc/ansible/ansible.cfg`

Follow along to create the ansible.cfg in our current directory using the 02-07 folder as reference

#### Create a new file

1. Create a new file in the ansible directory named `ansible.cfg`

2. Create a new comment 

`# ansible.cfg`

3. Create defaults group

`[defaults]`

4. Under `[defaults]` add 

`inventory = ./hosts-dev`


This tells Ansible to use the inventory file that we created in the previous lab.
Now everytime we run ansible commands it will use this new configuration file and set the global parameters around it.

5. Run the list-host command and we shouldn't have to specifiy where our inventory file is.

`ansible --list-host all`

I had to not create a new directory as listed in past videos, named ansible, or else `ansible --list-hosts all` doesn't work.

6. You are also able to target subsets of our inventory using a command like this

`ansible --list-host webservers`

7. We can also do the same for our loadbalancers

`ansible --list-host loadbalancers`

#### Create Alias for IP addresses

1. Since IP addresses can be sometimes difficult to read, we want to create alias for it. 
Go to your hosts-dev file and we can do this by adding `ansible_hosts=` before the IP address.
We can define the alias before `ansible_hosts` like using `app1`
It would look like this

`app1 ansible_host=18.211.170.81`

`app2 ansible_host=18.205.195.138`

2. We will do the same for loadbalancers

`lb1 ansible_host=52.207.53.232`

3. Save it and run this command and we see the aliases instead of the IP addresses

`ansible --list-hosts webservers`

4. Run this command and we will see the alias instead of the IP address

`ansible --list-hosts loadbalancers`

#### Using Patterns

1. We can also use patterns to list-hosts such as "*" which is the same as all

`ansible --list-hosts "*"`

2. We can also use wildcards like regular expressions

`ansible --list-hosts "app*"`

3. We can also use the or configuration by seperating the groups by colon

`ansible --list-hosts webservers:loadbalancers`

4. We can also use the negate functionality with

`ansible --list-hosts \!control`

We must escape the bang because this is bash

5. We can also use array syntax for our groups

`ansible --list-hosts "webservers[0]"`

This is the first element in the array of webservers.

## Chapter 3: Tasks

### Ansible Tasks

Ansible tasks are a way to run adhoc commands against our inventory in a one-line single executable. 
Tasks are the basic building blocks of Ansible's execution and configuration.

Tasks consists of two parts: The command you want to run and where you want to run it against
- Commands consist of the Ansible command, options, and host-pattern
e.g.

```sh
$ ansible options <host-pattern>
```

- Example of pinging all the hosts associated with our inventory
e.g.

```sh
$ ansible -m ping all
```

In this command above,
_ansible_ is ansible command
_-m_ is the module flag
_ping_ is module name
_all_ is inventory 


- When we run this task, `$ ansible -m ping all`, the results returned will give us important information about the execution on the end host

Let's jump into our ansible environment and run tasks and see what the results are.
You can reference 03_01 to follow along.

#### Run Tasks

1. In VScode terminal run this to ping all

`ansible -m ping all`

We had a sueccess ping into our local control host but failures in the other inventory items.
This is because whenever ansible tries to SSH into each one of our inventory items, it's using the default user associated with our local machine. 

2. Add some properties into our ansible.cfg file that gives it an idea on how to ssh into our instances.
We can set things like the global user, global key pair, and also how to check the fingerprint.
In ansible.cfg, create a new property under `[defaults]`

`remote_user = ec2-user`

3. Specify what private key we want to use to SSH into our instances

`private_key_file = ~/.ssh/ansible-course-key-pair.pem`

4. Set property to ensure that the host key fingerprint does not get checked when we try to SSH into our inventory items.
We will make the assumption that we never SSH into our inventory items so we don't need to check the fingerprint file when we SSH into them for the first time. 

`host_key_checking = False`

5. Save the file and rerun the ping command
We successfully logged into each one of instances and pinged them.
The results were returned as pong and no changes were made.
Running the ping command is a great way to troubleshot and ensure that you have connectivity to all your in-hosts.

6. Run ansible using the shell module and use -a for agruements. The agruement will pass in as "uname" and run it on all our webservers and all our loadbalancers

`ansible -m shell -a "uname" webservers:loadbalancers`

We can seee what the return results, that run the "uname" command on each one of webservers and loadbalancers.
We can also see the return status has changed which is misleading but anytime we run the command or the shell module, it's going to return a change status.
This is because ansible had to use the shell on the end host to run the so it returned to change status.
Don't worry about this, as we'll discuss this in a later topic.

We also returned rc or return code = 0.
In this case, this is successful since a return code of 0 means success.
A return code of 1 means failure.

7. To get a return command of false, run the ansible command with the command module, -a for agruments then "/bin/false", and run this on all our inventory except our local group

`ansible -m command -a "/bin/false" \!local`

Looking at the return results, we can see that Ansible marked it as a failure because the return code was 1. 
Since the bin flase returns a non-zero status code, ansible treats it as a failure.

As were are writing task and executing commands, using ansible will get change statuses, successful statuses, and failure statuses.
This is important because it helps us determine what was actually completed or not completed, what was successful, and what was a failure. 

## Chapter 4: Playbooks

### Playbooks

What are playbooks?

Playbooks are a way to congregate ordered processes and manage configuration needed to build out a remote system.

- Playbooks make configuration management easy and gives us the ability to deploy to a multi-machine setup.

- Playbooks can declare configurations and orchestrate steps (normally done in a manual ordered process), and, when run, can ensure our remote system is configured as expected.

- The written tasks within a playbook can be run synchronously or asynchronously.

- Playbooks gives us the ability to create infrastructure as code and manage it all in source control.

#### Playbooks are like grocery lists

Grocery List
```markdown
Produce
    salad
    starberries
    avocados

Meats
    Becon
    Chicken 

Dairy
    Cheese
    Milk

Breads
    Bagels
    Tortillas
```

Just like in playbooks, grocery lists:
- List our everything we need to purchase.
- Group each item with similar products.
- Ensure they are in a logically defined order.
- Shop for each item according to the order they are listed.

How are grocery lists like a playbook?

Playbook
```markdown
Update
    update all packages
    patching needed

Install 
    install item x
    install item y

Configure
    setup services
    update config files
    restart services

Check status
    Ensure up status

```
In a playbook we:
- List our everything we need to apply to each instance.
- Group them according to configuration usage.
- Ensure they are in a logically defined order.
- Run each tasks according to the order they are listed.

#### Deeper Dive into Playbooks

- Playbooks use YAML syntax which allows you to model a configuration or a process.
- Playbooks are composed of one or more plays in a list.
- The goal of a play is to map a group of host (defined in our inventory file) to a tasks that are used to call Ansible modules.
- By composing a playbook of multiple plays, it makes it possible to orchestrate multi-machine deployments and allows us to run certain steps on all machines in a group.

Example of a task we ran earlier:

```sh
$ ansible -m ping all
```

If we wanted to write this task as a play within a playbook, we could use something like the following:

```yml
# ping.yml          #comment on top
---                 # 3 dashes to tell ansible it is a playbook
    - hosts: all    # What hosts we want to run this play on
    tasks:          # Tasks we want to run on these items
        - name: Pinging all servers
        action: ping
```

Once we have a playbook, we can run the playbook by using the command:

```sh
$ ansible-playbook ping.yml
```

After running this playbook we will get a successful response if it able to ping and SSH into all of our inventory items.

Take note how the yaml syntax is written in the playbook.
Yaml syntax is tab-oriented and also uses these dashes to list our items.

We will write playbooks now, follow along in 04_01 folder.

#### Create a Playbook

1. In VScode, create a new folder named playbooks.

2. Within playbooks folder, create a new file named ping.yml.

3. Start it off with comment

`# ping.yml`

4. Use our three dashes to define a playbook

`---`

5. Define the host we want to run this playbook on by tabbing

`   - hosts: all`

6. Tab over again and do tasks

`       tasks:`

7. The tasks we want to include are ping all our instances

`           -name: Ping all servers`

8. Write an action for ping

`               action: ping`

#### Run the Playbook

1. Save the playbook.

2. Run the playbook

`ansible-playbook playbooks/ping.yml`

Seeing the results for `ansible-playbook` compared to `ansible -m command`,
instead of including all our information within a single one-line adhoc command, 
we can wrap all our parameter and agruments within a playbook and use the ansible-playbook command to execute our tasks.

##### Analyze Playbook Results

The **first thing** that the `ansible-playbook` command does is know where it is running the playbook on.

The **second thing** ansible does is gather facts. Ansible logs into each instance and gather info. 
These steps are turned back into the playbook where they can be used later to determine logic
or any additional steps that need to be ran with in the playbook.
So we can inject these values or create more logic depending on the facts that were gathered.

The **third thing** ansible does is ready to run the ping task and that's what happens in the results.
We can see the name of the task we specified within the platbook. 
You can also see that it ran for eachh one of our hosts so our control host, two app servers, and load balancer.
Since we specify all in our hosts parameter, it runs on all of our inventory items.
An important note is that, we no longer see the Pong result.
So it's important to remember that using Ansible paradigms, we really don't care about the standard output.
But what we really care about is wheather it was successful or failure, or whether we had changes made.
Since the return code was not an exit code, then our result was deemed successful.

The **forth step** that anisble returns is a play recap, which gives an idea on everything that happened within execution.
We can see things like what host did this execution run on, how many steps were made, and how many changes were made.
This will also give us a recap of all the successful responses and all the failed responses.
And for some reason, if we were not able to SSH into one of our hosts, then the unreachable would be set.

#### Run Task for Show Command

1. In playbooks folder, create a new file named uname.yml

2. Write a comment

`# uname.yml`

3. Start with our three dashes for our playbook

`---`

4. For hosts we are going to run this on our webservers and load balancers
`   -hosts: webservers:loadbalancers`

5. Define a task 

`       tasks:`

6. Give the tasks a name of Get OS type

`           - name: Get OS type`

7. The module we want to run is shell module and pass in uname

`               shell: uname`

8. Save the file and run the playbook

`ansible-playbook playbooks/uname.yml`

##### Review Results for uname.yml

We see ansible detected changes for each of the instances when really no changes were made.
Ansible is pretty good about detecting whether changes are made.
But when using certain modules like the shell module, it will detect changes because there was some output associated with running this command.
For instance, running the uname command returns the OS type.
There are ways to suppress these changes, but it's really up to us to know when real logical changes are made and how to handle them.
We will take a look at this more as we continue writing playbooks and executing them.

So now that we know what playbooks are and how to run them, let's think about how to build out our system using Ansible.
In the next section, we will talk about playbooks in action and use of Ansible for orchestration and configuration to build out our system.

### Playbooks in Action

What does it take to construct a system using playbooks?

We will break our system down in three parts: package management, configuration, and service handlers.

1. **Package Management**
What packages will our system need? 
Install all packages needed to run our system.
- patching
- package management

e.g. Example Playbook
```yml
---
    - hosts: loadbalancers
        tasks:
            - name: Install apache
                yum: name-httpd state=latest
```

What ansible will do is use the yum module to install latest version of Apache to loadbalancers.

You can set state to different variables.
You can set it to present or installed which will simply ensure that the desired package is installed. 
You can use latest, which will update specific packages if it is not installed to the latest version.
You can even specify the state to absent or remove, which will uninstall it from the system.
State is not the only parameter you can set on the yum module.
There are many other parameters you can set when executing modules. 
Yum isn't the only package manager we can use with ansible, there is more like apt for ubuntu

2. **Configure Intrastructure**
Configure our system with necessary application files or configuration files that are needed to configure environment.
- copy files
- edit configuration files
e.g. Example playbook
```yml
---
    - hosts: loadbalancers
        tasks:
            - name: Copy config file
                copy: src=./config.cfg dest=/etc/config.cfg
    - hosts: wevservers
        tasks: 
            - name: Synchronize folers
                synchronize: src=./app dest=/var/www/html/app

```

The first task we run is a simple copy module that copies a config file from our local Ansible setup to our load balancer in the etc diretory.
So in another example, instead of just copying over a single file, you can copy over the entire contents of a folder by using the sychronize module.
This will basically sync the two folders from your control machine to your inventory machine. 
So as you are building out your own system, think about all the configuration changes that need to happen before your system is able to run. 
This usually consists of copying some configuration file or maybe adding a single line to a configuration file, maybe just editing a property within a configuration file, but all of it can be taken care of using different modules ansibles offers to help configure your system. 

So once we have our packages installed and once our sytem is configured, then we can start thinking about how we wanna start, stop, or restart our system when changes are made.
Depending on what type of configuration changes you are making or what type of application builds you are applying, you may need to start, stop, or restart your system.

3. **Service Handlers**
Create service handlers to start, stop, our restart out system when changes are made.
- command
- service
- handlers
e.g. Example Playbook
```yml
---
    - hosts: loadbalancers
        tasks:
            - name: Configure port number
                lineinfile: path=/etc/config.cfg regexp='^port' line='port=80'
                    notify: Restart apache
            handlers: 
                - name: Restart apache
                    service: name=httpd status=restarted
```
We are making a simple configuration change to our config file here. 
We use the lineinfile module which is great for just changing a single line in a file, to change the port number from whatever it is to port 80.
And after those changes are made, we will use service handlers to notify to restart Apache.
Using handlers in this way will allow us to restart our services only if changes are made.
We could just write another task under the lineinfile module to always restart Apache.
But in some cases, we don't wanna have to restart Apache.
So if instance, if the configuration file already had port 80 in it, there would not be any changes made, therefore we would not have to restart Apache.
Since Ansible knows how to keep up with changes, we can use service handlers to restart services, appropriately.
Since we named our handler restart Apache, then we can just call notify: Restart apache within the lineinfile task.

### Constructing System

#### Playbooks in Action

1. **Package Management**
- apache
- php

2. Configure Infrastructure
- upload index.php
- configure php.ini
- configure Load Balancer

3. Service Handlers
- restart services

You can follow along in 04_03

#### Run Yum-update.yml Playbook

1. In VScode, create a playbook to update all of our inventory items.
We will name it yum-update.yml

2. Start off with comment

`# yum-update.yml`

3. Write our three dashes for playbook

4. List out our hosts

`  - hosts: webservers:loadbalancers`

5. Tasks we will run is yum-update

`       tasks:`
`           - name: Updating yum packages `

6. For the module, we will use the yum module and have it install all packages and the latest packages

`        yum: name=* state=latest`

7. Save and run the playbook in terminal

`ansible-playbook playbooks/yum-update.yml`\

There was error because we need to be root to perform an update
Luckly Ansible allows us to esclate our permisions on the system by using the become directive.
We can also use the become user if we want to set a specific user with desired privileges.

8. Under hosts and the become property as true

`   become:true`

This basically tells Ansible that when running this playbook, run as sudo.

9. Save and run the playbook.
Now all three inventory items are up to date.

When running earlier, and it failed. It created a retry file that says how many of our hosts where tried and failed.

#### Change ansible.cfg to Delete Retry files

1. In ansible.cfg, tell it to not create retry files.

`retry_files_enabled = False`

#### Rerun playbook

1. Run the playbook again and we see that the status is ok but nothing has changed because Ansible knows it is up to date.

#### Create a Playbook to Install Services

1. Create a new playbook called install-services.yml

2. Create a comment on top and follow by the dashes
```yml
# install-services.yml
                       
---
```

3. Start off by targeting our loadbalancers first with hosts
`   - hosts: loadbalancers`

4. Write a task to install apache

5. Use the yum module to install apache

`               yum: name=httpd state=present`

Setting state to present means that if Apache is not present, install it.
If present, don't install it.

6. Set hosts as webservers and name of installing services. We can install httpd and php using same name.
```yml
    -hosts: webservers
        tasks:
            - name: Installing services
                yum:
                    name:
                        - httpd
                        - php
                    state: present

```

7. Add become = true to run as root.

8. Save and run the playbook.

9. Now we want our playbook to start apache and make sure it starts up on reboot for both webservers and loadbalancer
We can do this by making another tasks that starts apache
Name it ensure apache starts and give the state as started and enabled as yes
```yml
            -name: Ensure apache starts
                service: name=httpd state=started enabled=yes
```

10. Save the file and run the playbook

#### Create a PHP file to have Apache serve it up

1. Create a PHP file by creating a new file in the root of the directory and name it

2. Within the file do an open and close php tag
```php
<?php

?>
```

3. Within the tag, echo a simple html element.

`echo "<h1>Hello, World! This is my Ansible page.<h1>";`

4. Save the page and get this file from our local system to our webservers.

5. Create another playbook called setup-app.yml. Write a comment for name of file and add dashes

6. Name the hosts as webserver and name the task to copy our source file to our destination then give it permissions on the file

```yml
# setup-app.yml

---
    - hosts: webservers
        tasks:
            - name: Upload application file
                copy:
                    src: ../index.php
                    dest: /var/www/html
                    mode: 0755

```

7. Save it and run the playbook.
We are not able to because we have to elevate our privileges by using become.

8. Save the file and rerun the playbook.

#### Check to see if File is being Served

1. Go to a web browser and navigate to both of the ip addresses in the hosts-dev file.

It works!

#### Edit Single Line in a File

We can do this by the module lineinfile.
The file we will do this to is the php.ini file.
We also want to make changes to allow for short open tags when writing php pages
**NOTE** PHP documentation for tags doesn't recommend but we are doing it for demo for lineinfile module.

1. In index.php, remove just the php and reupload it to the webserver

2. Save and run the setup-app.yml playbook.

3. Check to see changes in web browser for webservers. 
See that the PHP complier doesn't know how to compile files that have an open php tag.

4. To fix this we can set a property in the php.ini file that allows for short open tags.
In setup-app.yml create another task named configure php.ini file

`           - name: Configure php.ini file`

5. Use the lineinfile modulevand specify a path to php.ini and use the parameter to find the lineinfile we want to change by using a regular expression, we do this by setting regex parameters with regexp
```yml
                lineinfile:
                    path: /etc/php.ini
                    regexp: ^short_open_tag
```

6. Specify now what changes we want to make on this line.
Do this using the line parameter to set short_open_tag to on

`                   line: 'short_open_tag=On`

7. Once we make these changes in the configuration file we need to restart apache so the changes will take place.
Create another task to restart apache
```yml
      - name: Restart apache
        service: name=httpd state=restarted 
```

8. Save and run the playbook.

9. Go to web browser and check and it is now pretty again. 

10. Now check to see what our loadbalancer looks like now. This is default page for loadbalancer because we haven't configured it yet.

### Configuring Load Balancer

What have we already accomplished?

1. **Package Management**
- [x] apache
- [x] php

2. **Configure Infrastructure**
- [x] upload index.php
- [x] configure php.ini
- [ ] configure Load Balancer

3. **Service Handlers**
- [ ] restart services

Load Balancer Config File

```yml
...
<Proxy>
    BalancerMember http://<IP_ADDRESS>
    BalancerMember http://<IP_ADDRESS>
    ...
    BalancerMember http://<IP_ADDRESS>
</Proxy>
...
```

We could create the file above and put it to our loadbalancer but its very static and not dynamic at all.
What if we added more webservers all the time, we would have to keep adding them as balancermembers
Instead we can loop through each one of our inventory items, webservers, and have them populate the config file before we upload it.
Luckly, Ansible gives us templates to take care of use-cases like this.

Load Balancer Config File (template)
```yml
...
<Proxy>
    {% loop through inventory}
        BalancerMember http://{{ ip_address }}
    {% endfor %}
</Proxy>
...
```
With this template we can loop through each one of our inventory items and have it populate the BalancerMember with the IP address.
This makes our configuration much more dynamic so we are able to add more items to our webservers group and know that when we upload our configuration file to our load balancer, the new items will be propagated in configuration file for loadbalancer.

The templating engine used is Jinja 2, now it is Jinja 3. Basically it's a templater to store variables as objects and looping.
Using the Jinja syntax throughout your playbooks is going to create a more dynamic setup that allows you to use variables and make your playbooks more modular. 

We will make a template file to accomplish this. Using the Load Balancer Template found in the README.md of this repo will make it easier. 

#### Create template for Load Balancer Config

1. Create a folder in the root directory of our repo and name it config.

2. Create a new file within the config folder named lb-config.j2

3. Copy and paste code from Github repository int lb-config.j2

4. Add the webservers IP to balance members.

5. Now this can work but once we get more webservers we have to manually add so lets make it more modular with a loop
Before Balancer memember line make a code snippet
`{% for hosts in Groups ['webservers'] %}`

6. End the loop after Balancers with

`{% endof%}`

7. Replace the IP address with double curly braces to define an expression in our template.Inside the braces is where you are going to evaluate the IP address for our web servers.
We can find this by looking in the hostvars variable that Ansible creates during the gathering facts steps. 

The `hostvars` holds a dictionary about our inventory. 
Within the `hostvars` we can specify which web servers that we want to pull the IP address from.
We can do that by saying get the element of `[hosts]` off of `hostvars`

Then we can pull the variable `['ansible_host']` from the item of `[hosts]`.
`['ansible_host']` will evaluate to the IP address associated with our web servers.


`BalancerMember http://{{ hostvars[hosts]['ansible_host']}}`

8. Now copy the option code into lb-config.j2 then save it.
We are ready to upload `lb-config.j2` into our load balancer.

9. To upload `lb-config.j2` into our load balancer, create a new playbook called `setup-lb.yml`

10. Start with a comment

`# setup-lb.yml`

11. Call our hosts as loadbalancer

`  - hosts: loadbalancers`

12. Define our tasks

`    tasks:`

13. Give our first task a name

`      - name: Creating Template`

14. Call in the template module

`        template:`

15. Define the template parameters for src as the template file of the lb we just created

`          src: ../config/lb-config.j2`

16. Put the dest and then name the file lb.config
`          dest: etc/httpd/conf.d/lb.conf`

17. Set up the rest of the parameters to make sure the file has the correct permissions. 
```yml
          owner: bin
          group: wheel
          mode: 064
```

18. Now once that file is uploaded we want to have a task that restarts apache.
```yml
      - name: Restart Apache
        service: name=httpd state=restarted
```

19. Make sure to be sudo to run these tasks. 
`   become: true`

20. Save and run the playbook
`ansible-playbook playbook/setup-lb.yml`

21. Now when going to the load balancer we are sent to our webservers
We can also see info about our load balancer by going in the url to /balancer-manager

### Service Handlers

In this lesson we will use a service handler to restart services.

In our current setup, whenever we upload a config file then apache restarts.
This is okay if our config files change but what if our config files don't change?

Service handlers detect if changes happened to the config file, if so it restarts apache.
If it doesn't detect changes then the subsequent tasks do not run.

e.g.
Using Service Handlers

```yml
...
    tasks: 
        - name: Configure php.ini file
            lineinfile:
                path: /etc/php.ini
                regexp: ^short_open_tag
                line: 'short_open_tag=On'
            notify: restart apache
    handlers:
        - name: restart apache
            service: name=httpd state=restarted

```
Handlers in this example is only going to run if the notify parameters is set. 

#### Set up Service Handlers

1. In setup-app.yml we see with our current code that we are restarting apache no matter what.

```yml
      - name: Restart apache
        service: name=httpd state=restarted 
```
Really we only want apache to restart when there is changes to the php.ini file

2. Add a notify parameter to the lineinfile task

`notify: Restart apache`

3. Add handler to the Restart apache task.

`    handlers:`

4. Save and rerun the playbook.

5. If we make changes to php.ini such as setting `'short_open_tag=On'` to `'short_open_tag=Off'` then apache will restart.
Make the changes and run the playbook to see that apache restarts

6. Change it back to `'short_open_tag=On'` and run the playbook.

7. Make the changes in setup-lb.yml to use a handler. Run the playbook and apache is not restarted becasue we didn't make any change to Creating template.

We now have everything we need to construct our system from start to finish.

### Congregate Playbooks

We are going to congregate all of the playbooks we created into a single playbook. 

What we have finished for playbooks in action

1. **Package Management**
- [x] apache
- [x] php

2. **Configure Infrastructure**
- [x] upload index.php
- [x] configure php.ini
- [x] configure Load Balancer

3. **Service Handlers**
- [x] restart services

Example playbook of what we will accomplish

```yml
# all-playbooks.yml
---
    - import_playbook: yum-update.yml
    - import_playbook: install-services.yml
    - import_playbook: setup-app.yml
    - import_playbook: setup-lb.yml
```

Running the above on our instances would not have changes but think if we had a blank instance with no changes, this would build out our system from scratch.

#### Create all-playbooks.yml

1. In playbooks, create a file named all-playbooks.yml

2. Write a comment with the name and then the three lines to reference a playbook.

`# all-playbooks.yml`

3. Use the import_playbook feature to run each of our playbooks and start with yum.update playbook.

`- import_playbook: yum.update.yml`

4. Import the install services playbook that installs all of our Apache and PHP dependencies

`- import_playbook: install-services.yml`

5. Use our playbook that sets up our web server application file and configures it.

`- import_playbook: setup-app.yml`

6. Use our playbook that will set up our load balancer and configure it.

`- import_playbook: setup-lb.yml`

7. Save the playbook and run it.

#### Create check-status.yml

1. Create a new playbook that will check the status of apache.
When it is run and returns a 0 then we know that Apache is up and running.
Create a new playbook and name it check-status.yml

2. Start with a comment and the 3 dashes

`# check-status.yml`

3. Run this against all of our web servers and load balancers

`  - hosts: webservers:loadbalancers`

4. Create a task that calls the command module to check the service off httpd status

```yml
    tasks:
      - name: Check status of apache
        command: service httpd status

```

5. Elevate your privileges to root

`    become: true`

6. Save and run the playbook.

7. Take down apache on our loadbalancer and see what happens when we run check-status.yml.
Run a simple adhoc command that stops apache on our load balancer

`ansible -m service -a "name=httpd state=stopped" --become loadbalancers`

Explanation of above command:
-m means module and we call the service module.
-a means that we are passing arguments of name and state to stop apache.
--become to run this command as root.
Then we are running the command against our loadbalancers.

8. Run the playbook and we get a non-zero return code

9. Run the adhoc command to start it then run the playbook again.

`ansible -m service -a "name=httpd state=started" --become loadbalancers`

10. Add the check.status.yml playbook to all playbooks.

`  - import_playbook: check-status.yml`

11. Run to make sure it runs as expected. 
