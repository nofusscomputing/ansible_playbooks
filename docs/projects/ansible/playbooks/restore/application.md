---
title: Application Restoration
description: Restoring Application backups using No Fuss Computings Ansible playbook restore.yaml
date: 2024-01-13
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This Playbook is used to restore application backups from our [backup](../backup/index.md) playbook. This includes restoring different application types, `host direct`, `Docker` and `kubernetes`.


- job tag `application`

!!! warning "Important"
    The use of this playbook requires that `--limit` be specified as this is the host that the restoration will occur to.


## Playbook AWX / Ansible Automation Platform Template Import

This playbook includes the AWX feature where it imports the playbook as job templates in to AWX / Ansible Automation Platform. The following job templates that will be created:

- **Restore/Application** Restore an Application


## Play workflow

1. copy backup file to remote host

1. decrypt backup file

1. Restore path(s) to the container

1. Restore database(s)

1. remove backup file from the host.

!!! info
    On error during any stage of the workflow, prior to the play stopping all files related to the workflow, included any created (i.e. extracting contents of archive) will be removed so there are no traces on the remote host.


## Variables

This playbook requires variables be set which instruct what is to occur. The parent key for the variables is `nfc_pb_restore` which is a yaml object that when completed determins what backup(s) to use and how to restore the backup(s).

Restoration Yaml Object.

``` yaml
nfc_pb_restore:
  restore_directory_backups:    # Mandatory, String. Name of backup file to restore without file extension. .enc.tar is assumed
  applications: []              # Mandatory, List of Dict. The applications to restore
```

### Restoring an Application

To restore an application, an application list item must be added to `nfc_pb_restore` at path `nfc_pb_restore.applications` Application list items are broken down into three different types: `application`, `docker` and `kubernetes`. Each type has it's own requirements.

!!! tip
    Different Application type backups can be restored to a different application type. This allows a migration for example where the deployment of said application was originally with docker, that now is being deployed with kubernetes. 

=== "Kubernetes"

    ``` yaml
    - name: vaultwarden         # Mandatory, String. Name of the Application
      file_restore:                 # Mandatory, String. Name of file without extension to restore
      type: kube                # Mandatory, string. Kubernetes container engine
      path: []                  # Mandatory*, List of Dict. Required to restore files to path
      pod: vaultwarden-0
      namespace: vaultwarden
      databases:                # Mandatory*, dict, choice=mariadb|mysql. Only required if database restoration to be done.
        mariadb: []             # Mandatory, string. MariaDB/MySQL databases to restore
        mysql: []               # Mandatory, string. MariaDB/MySQL databases to restore
        postgres: []            # Mandatory, string. Postgres databases to restore
    ```

=== "Docker"

    To be developed

=== "Application"

    To Be Developed

!!! tip
    If you wish to restore backups for multiple applications this is possible by adding additional application list items.


#### Files

Restoring files requires that there be a path list item added at path `nfc_pb_restore.applications[x].path`

``` yaml

- name: data            # Mandatory. String. sub-directory name inside the archive
  path: /data           # Mandatory, String. Path within the container to restore to

```


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

    ``` yaml
    - name: vaultwarden                                 # Mandatory, String. Backup database filename.
      host:                                             # Database connection host
        name: main-postgresql-ha-pgpool.postgres.svc    # Mandatory. String. DNS or IP of database host to connect
        port:                                           # Optional, Integer. port of database host to connect to.
        socket:                                         # Optional. String. Path to the MySQL socket to use for connection.
                                                        #                   If used host.name is not required
      destination: vaultwarden                          # Optional, string. default={use name key} database name to restore to.
   
