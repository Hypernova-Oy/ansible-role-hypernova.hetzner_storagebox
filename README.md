# ansible-role-hypernova.hetzner_storagebox

Ansible role for mounting Hetzner Storage Box with sshfs.

Generates a new SSH key and installs it in your Storage Box for passwordless
access.

Adds an entry to /etc/fstab

# Installation

Unfortunately we do not publish our roles in ansible-galaxy. You have to clone this manually.

`git clone https:/github.com/Hypernova-Oy/ansible-role-hypernova.hetzner_storagebox hypernova.hetzner_storagebox`

# Variables

```
hetzner_storageboxes_base_dir: /hetzner_storagebox

hetzner_storageboxes:
  - name: storagebox-1
    username: u000000
    password: "{{ vault_hetzner_storagebox_myuser_password }}"
    hostname: u000000.your-storagebox.de
    fstab_owner: myuser
    fstab_group: mygroup
```

`fstab_owner` and `fstab_group` are the user and group on your host that gain
the filesystem ownership of the mounted Storage Box.

# Playbook

```
- hosts: hetzner_storagebox
  roles:
    - { role: hypernova.hetzner_storagebox, become: yes }
```

# Hetzner SSH magic

Hetzner Storage Box is using two SSH key formats, RFC4716 in port 22 and OpenSSH
format in port 23.

This role adds your key using both formats using Hetzner's `install-ssh-key` tool
on the Storage Box.

https://docs.hetzner.com/storage/storage-box/backup-space-ssh-keys/#ssh-key-authentication-for-storage-boxes
