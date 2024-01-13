---
title: Application Backup
description: Conducting container backups using No Fuss Computings Ansible playbooks
date: 2024-01-13
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

Application backups are a simplified backup that has been normalized for restoration across multiple application run methods. This includes a direct install on a host, ran with docker and ran with kubernetes. This play will create a backup of the specified application to a single encrypted tar file and copy locally.

- job tag `application`


## Playbook AWX / Ansible Automation Platform Template Import

This playbook includes the AWX feature where it imports the playbook as job templates in to AWX / Ansible Automation Platform. The following job templates that will be created:

- **Backup/Application** Backup an application with the specified `extra_vars`

- **Backup/Inventory** Conduct a backup using variables from inventory to select what to backup

On import to AWX / Ansible Automation Platform a credential type will also be created, `playbook/mariadb/login` that can be used to supply the required secrets to logon to the MariaDB Database.


## Play workflow

The playbook follows the following flow when conducting a container backup.

1. Creates temp directory to store the backup data

1. Backs up each path to the temp location in a sub-directory named the same as the final directory in the path

1. Conducts backup of all databases to sub-directory `database-dump` in the temp location, using database as filename.

1. encrypts the data within a tar archive

1. copies the backup file locally

Backing up multiple items will have the workflow above start again from step two.

## Variables

Required variables.

``` yaml
backup:
  # directory: /opt/backup              # To Be Removed
  encryption_algorithm: aes256          # Encryption algorithm
  applications: []                      # Mandatory, List of dict. List of applications to backup

```

Options for the application list are detailed below.

There are different varaible requirements depending on the container engine. Select the appropriate tab.

=== "Docker"

    Application variable required to backup a Docker container.

    ``` yaml

    - name: glpi                             # Mandatory, String. Name of the Application
      type: docker                           # Mandatory. choice=docker|kube. Container engine type
      container: glpi                        # Mandatory. String. Name of the container to backup.
      path:                                  # Mandatory. List of String. Path within the container to backup
        - /var/www/html/config
        - /var/www/html/files
        - /var/log
        - /var/www/html/marketplace
        - /var/www/html/plugins
      databases: []                          # Mandatory*. List of dict. Required only if backing up database
    ```

=== "Kubernetes"

    Application variable required to backup a Kubernetes container.

    ``` yaml

    - name: glpi                             # Mandatory, String. Name of the Application
      type: kube                             # Mandatory. choice=docker|kube. Container engine type
      pod: postgres-0                        # Mandatory, String. the name of the pod to backup
      container: backup                      # Mandatory. String. Name of the container to connect to.
      namespace: postgres                    # Mandatory. String. Namespace name where the pod belongs
      path:                                  # Mandatory. List of String. Path within the container to backup
        - /opt/backup
      databases: []                          # Mandatory*. List of dict. Required only if backing up database
    ```

=== "Application"

  To Be Developed

Database Backup dict

``` yaml

- names:                                 # Mandatory. List of String. name(s) of database to backup
    - glpi
  type: mariadb                          # Mandatory, choice=mariadb|mysql
  method: docker                         # Optional, choice=docker|direct. default=direct specify the method to backup the database
  encrypt: false                         # Optional, Boolean. default=true. Encrypt the database backup
  socket: /var/run/mysqld/mysqld.sock    # Mandatory. String. Path to the MySQL socket.
  # address:         # 
```

A part of the backup archive a metadata file is created `metadata.yaml`. This is a yaml file containing the details of the files within the backup. The metadata file is a yaml formated list of dicts.

metadata file contents example

``` yaml
- checksum:
    algo: "sha256"
    value: "c545cb6a1682beee488ac6b03b7356e459812515939e564cd0c79d3ddf2e2286"
  gid: 100
  mode: "0644"
  name: "plugins/glpiinventory/lib/lazy.js-0.5.1/.travis.yml"
  uid: 0
```
