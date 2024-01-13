---
title: Backup Playbook
description: No Fuss Computings Ansible playbook
date: 2023-10-23
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This playbook provides an easy way to backup the data that forms part of your infrastructure. The playbook has been designed in such a way that the backups are per application and contain all of the data pertinent to that application.


## Features

The following items can be backed up with this playbook:

- Application _(Planned)_

    _All Data pertinent the the application_

- [Containers](application.md)

    _Container paths and database dumps_

- Files

    _directory or group of directories_

- MariaDB / MySQL

    _Database dump_

- PostgresDB

    _Database dump_


## Using the Playooks

Running of these playbooks is no different to any other. However there are a fuew requirements that must be met first:

- the user running the playbook must be a member of the `backup` group

- Postgres actions require `psql` to be installed

    !!! tip
        `psql` can be installed with `apt install postgresql-client-<version>` on a Debian system

- MariaDB / MySQL actions require `mysql` to be installed

    !!! tip
        `mysql` can be installed with `apt install mariadb-client-core-<version>` on a Debian system.

### Play workflow

The basic workflow for all backup tasks is as follows:

1. Creates temp directories for backup task

1. Conduct backup

1. encrypts the data within a tar archive

1. copies the backup file locally

To view the workflow of the type of backup, refer to thir documentation pages.


## Commonalities

The backup tasks within this playbook do share some things that are common to all backup tasks, they are:

- file owner user set to `root`

- file owner group set to `backup`

- file mode set to `0770`