---
title: Restore Backup Playbook
description: No Fuss Computings Ansible playbook restore backup
date: 2023-10-30
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---


This Playbook is designed to restore backups that were createdby our [backup](../backup/index.md) playbook.


## Using the Playooks

Running of this playbook is no different to any other. However there are a few requirements that must be met first:

- The user running the playbook must be a member of the `backup` group on the remote host.

- Postgres actions require `psql` and `pg_config` to be installed and python modules `psycopg2`.

    !!! tip
        install with `apt install postgresql-client-<version> postgresql-common` on a Debian system and `pip install psycopg2-binary` for the python modules

- MariaDB / MySQL actions require `mysql`, `mariadb-client-<version>` to be installed and python modules `PyMySQL`

    !!! tip
        `mysql` can be installed with `apt install mariadb-client-core-<version> mariadb-client-<version>` on a Debian system and `pip install PyMySQL` for the python modules.


## Restoration tasks

Just like out backup playbook, this playbook is broken down into sub-tasks, being:

- [Application restore](application.md). This covers application type Containers. Specifically in this case, Kubernetes containers
