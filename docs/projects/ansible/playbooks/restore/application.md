---
title: Application Restoration
description: Restoring Application backups using No Fuss Computings Ansible playbook restore.yaml
date: 2024-01-13
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This Playbook is used to restore backups from our [backup](../backup/index.md) playbook. This includes restoring different application types, `host direct`, `Docker` and `kubernetes`.


- job tag `application`


## Playbook AWX / Ansible Automation Platform Template Import

This playbook includes the AWX feature where it imports the playbook as job templates in to AWX / Ansible Automation Platform. The following job templates that will be created:

- **Restore/Application** Restore an Application


#### Database

To restore a database a database list item must be added at path `nfc_pb_restore.applications[x].databases.<database_type>`

=== "MariaDB / MySQL"

    !!! tip
        For both MariaDB and MySQL the same code is executed and each name only exists for ease of reading. The reality is that either `mariadb` or `MySQL` keys are interchangeable.

    ``` yaml
    - name: vaultwarden                     # Mandatory, String. Backup database filename.
      host:                                 # Database connection host
        name: mariadb-galera.mariadb.svc    # Mandatory. String. DNS or IP of database host to connect
        port:                               # Optional, Integer. port of database host to connect to.
        socket:                             # Optional. String. Path to the MySQL socket to use for connection.
                                            #                   If used host.name is not required
      destination: vaultwarden              # Optional, string. default={use name key} database name to restore to.
    ```

=== "Postgres"

    To be developed
