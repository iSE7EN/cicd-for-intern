# cicd-for-intern

inventory/lab.ini
```ini
lab01		ansible_host=10.0.0.1
lab02		ansible_host=10.0.0.2
lab03		ansible_host=10.0.0.3
lab04		ansible_host=10.0.0.4
lab05		ansible_host=10.0.0.5
```

sudo.yml
```yml
---
#file: sudo.yml
- hosts: all 
  gather_facts: no
  remote_user: intern
  become: true
  tasks:
  - name: Allow 'intern' group to have passwordless sudo
    lineinfile:
      dest: /etc/sudoers.d/intern
      state: present
      regexp: '^intern'
      line: '%intern ALL=NOPASSWD: ALL'
      validate: 'visudo -cf %s'
```

install-package.yml
```yml
---
#file: install-package.yml
- hosts: all 
  become: true
  remote_user: intern
  tasks:
  - name: "Install the package net-tools"
    ansible.builtin.apt:
      name: net-tools
```

### Example
```
ansible-inventory -i inventory/intern.ini --list
ansible all -m ping -u sshuser -i inventory/intern.ini
ansible-playbook -i inventory/intern.ini sudo.yml --limit lab03
```
