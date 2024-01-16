---
title: Backup Playbook
description: No Fuss Computings Ansible playbook
date: 2023-10-23
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This playbook provides an easy way to backup the data that forms part of your infrastructure. The playbook has been designed in such a way that each backup is a discrete unit, known as an application backup. Each [application](application.md) contains all data related to that application: files and database dumps. Each backup unit is then stored as an encrypted tar archive ready for [restoration](../restore/index.md).


## Features

- Backup of:

    - Files

        _directory or group of directories_

    - **Planned** Kubernetes Secrets 

        _manifests of the secret_

    - MariaDB / MySQL

        _Database dump_

    - PostgresDB

        _Database dump_

- Encrypted backups

- Restoration of an application from one application type to the other


## Using the Playooks

Running of these playbooks is no different to any other. However there are a few requirements that must be met first:

- the user running the playbook must be a member of the `backup` group

- Postgres actions require `psql` to be installed and pip module `psycopg2`

    !!! tip
        Install with `apt install postgresql-client` on a Debian system

- MariaDB / MySQL actions require `mysqldump`and `mysql` to be installed and pip module `PyMySQL`.

    !!! tip
        Install with `apt install mariadb-client` on a Debian system.

- Kubernetes containers that are having their files backed-up, require tar to be instlled in the contianer.

    !!! tip
        If tar is not installed within the container, add a sidecar container that contains tar. `debian:bookworm-slim` is one such container and use `bash -c sleep 9999999999999999999` as the container command so it doesn't stop.


!!! danger "Alert"
    When a backup path contains a symlink, these are not followed. As a consequence, the symlink not the data is backed up. You are advised to add the symlink to `excludes` and set the directory where the symlinked file resides as a path to backup. As nessacary, add additional excludes to exclude files that you dont wish to backup that reside in the symlinked files directory.


### Play workflow

The basic workflow for all backup tasks is as follows and is grouped per application:

1. Creates temp directories for backup task

1. Conduct backup

1. encrypts the data within a tar archive

1. copies the backup file locally

To view the workflow of the type of backup, refer to their [documentation pages](application.md).


## Commonalities

The backup tasks within this playbook do share some things that are common to all backup tasks, they are:

- file owner user set to `root`

- file owner group set to `backup`

- file mode set to `0770`