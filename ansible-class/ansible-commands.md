# Ansible Class

24 Jan 2024

## Ansible Navigator

ansible-navigator - a Text-based User Interface (TUI) for running and developing Ansible content.

View configured Execution Environments
`ansible-navigator images`

Examining the ansible-navigator configuration
`cat ~/.ansible-navigator.yml`

## Inventory 

To use the ansible-navigator command for host management, you need to provide an inventory file which defines a list of hosts to be managed from the control node

To view your inventory with ansible-navigator
`ansible-navigator inventory --list -m stdout`
**NOTE: -m is short for --mode which allows for the mode to be switched to standard output instead of using the text-based user interface (TUI).**

For a less detailed view with a visual representation of groupings.
`ansible-navigator inventory --graph -m stdout`

Other commands to see inventory

```
ansible-navigator inventory --graph web -m stdout
[student@ansible-1 ~]$ ansible-navigator inventory --graph control -m stdout
[student@ansible-1 ~]$ ansible-navigator inventory --host node1 -m stdout
```

## Enter Ansible-navigator interface

`ansible-navigator`

## Tasks

- Present: Install package don't care if latest
- Latest: Install the latest package
### Colors of Playbooks

- Green: Task executed and no changes
- Yellow: Task executed, making a change
- Red: Task failed execution

## Run Playbooks

`ansible-navigor run playbook <playbook-name>.yml`

Run playbook with verbose
`ansible-navigor run playbook <playbook-name>.yml -m`

## Navigator subcommands

- collections
- config
- doc
- images
- inventory

## Facts

Like variables but coming from host itself
Check them out with the setup module

```
- name: facts playbook
    hosts: localhost
    
    tasks: 
        - name: 
            setup:
            gather_subset:
                - 'all'

```

## Conditionals

conditionals via vars
Task only runs if `when: ` is matched

`when: `

There is also a `when: ` statement that is `register: ` that if something is

## Handler

Advanced version of `when` using notifer, `notify: `

if either task notifies a changed resultm the handler will be notified ONCE

If two notify, handler is only ran once.

## Loop

Not have to run multiple command (i.e create user on all systmes)

```
tasks:
- name:
    user:
    name: "{{item}}"
    state: present
loop: 
    =devkf
    -dkjg
    -fjkieo
```

