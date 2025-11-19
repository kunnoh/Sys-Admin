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
```sh
ansible-playbook -i inventory playbook.yml
```

## Reference
1. [Ansible docs](https://docs.ansible.com/ansible/latest/)


