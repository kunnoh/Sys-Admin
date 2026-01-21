# Ansible

## Installation
Check python version
```sh
python --version
pip --version
```

Install ansible as python module
```sh
python3 -m pip install --user ansible
```

Upgrading ansible
```sh
python3 -m pip install --upgrade --user ansible
```

Verify
```sh
ansible --version
```

## Configuration

## Inventory
Example
```yml
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

## Playbooks
Provide a repeatable, reusable, simple config management & multimachine deployment system [1](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_intro.html#ansible-playbooks).  
Uses [YAML sysntax](https://docs.ansible.com/projects/ansible/latest/reference_appendices/YAMLSyntax.html#yaml-syntax).  

Example:  
```yml
---
- name: create blog archive
  hosts: stapp01, stapp02, stapp03
  become: yes
  tasks:
    - name: create archive file and set the owner
      archive:
        path: /usr/src/data/
        dest: /opt/data/blog.tar.gz
        format: gz
        force_archive: true
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
```

### Check mode
Check mode allows you you to execute a playbook without applying any alterations to the systems [2](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_intro.html#running-playbooks-in-check-mode).  
```sh
ansible-playbook --check playbook.yaml
```

### Apply
Run without playbook file.  
```sh
ansible all -a "ls -lah /opt/data/" -i inventory
```

Using playbook.yml.  
```sh
ansible-playbook -i inventory playbook.yml
```

## blockinfile

The Nautilus DevOps team wants to install and set up a simple httpd web server on all app servers in Stratos DC. Additionally, they want to deploy a sample web page for now using Ansible only. Therefore, write the required playbook to complete this task. Find more details about the task below.


We already have an inventory file under /home/thor/ansible directory on jump host. Create a playbook.yml under /home/thor/ansible directory on jump host itself.

    Using the playbook, install httpd web server on all app servers. Additionally, make sure its service should up and running.

    Using blockinfile Ansible module add some content in /var/www/html/index.html file. Below is the content:

    Welcome to XfusionCorp!

    This is  Nautilus sample file, created using Ansible!

    Please do not modify this file manually!

    The /var/www/html/index.html file's user and group owner should be apache on all app servers.

    The /var/www/html/index.html file's permissions should be 0755 on all app servers.

```yml
---
- name: Install and configure httpd web server
  hosts: all
  become: yes
  tasks:
    - name: Install httpd package
      ansible.builtin.yum:
        name: httpd
        state: present
        update_cache: yes

    - name: Ensure httpd service is started and enabled
      ansible.builtin.service:
        name: httpd
        state: started
        enabled: yes

    - name: Add sample content to /var/www/html/index.html
      ansible.builtin.blockinfile:
        path: /var/www/html/index.html
        create: true
        owner: apache
        group: apache
        mode: '0755'
        block: |
          Welcome to XfusionCorp!
          This is  Nautilus sample file, created using Ansible!
          Please do not modify this file manually!
```


## Reference
1. [Ansible docs](https://docs.ansible.com/ansible/latest/)


