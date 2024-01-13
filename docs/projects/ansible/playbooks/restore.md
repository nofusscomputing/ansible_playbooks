---
title: Restore Backup Playbook
description: No Fuss Computings Ansible playbook restore backup
date: 2023-10-30
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

!!! Note
    Docs Still under development

This Playbook is designed to restore backups to various systems. It uses the configuration as defined in the Ansible Inventory. The specific backup details are from our [backup playbook](backup/index.md). Currently the following restoration of backups is supported:

- Files


## Files Restoration Task

The file restoration tasks within the playbook will restore backups created by our [backup playbook](backup/index.md).


### Playbook Variables

The following is the minimum required defined variables in inventory

``` yaml

restore:
  backup_storage_directory: /backup
  files:
    - name: ""        # Mandatory, String. {backup_file_name}-{inventory_hostname}
      root_directory: '' # Optional, string. root directory where to restore. default=/

backup:
  encryption_algorithm: aes256

```

during the running of this playbook, you must specify additional variables `backup_storage_directory` and `restore_backup_date`. for example

``` bash
clear; ansible-playbook --limit {the ansible host to restore the backup to} \
  -i inventory/production \
  /playbooks/restore.yaml \
  --vault-id recovery@recovery_vault \
  --extra-vars "restore_backup_date=2023-10-24-13:14:08-+0000" \
  --extra-vars "backup_storage_directory=/backup" \
  --tags files
```
