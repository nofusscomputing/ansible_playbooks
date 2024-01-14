---
title: PostgreSQL Module Playbook
description: No Fuss Computings Ansible playbook, ansible_module_postgres.yaml
date: 2024-01-14
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This playbook provides direct access to an Ansible Module. The following Modules are supported by this playbook:

- [community.postgresql.postgresql_db](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_db_module.html)

For each of the above modules, variable `module_database` must be defined. This variable is a dictionary of the options as are used by the module. The documentation for each module can be found by click on the module name above. 


## Module definitions

Each module definition is detailed below under the tab matching the module name. This directly shows what is or is not supported.

=== "community.postgresql.postgresql_db"

    ``` yaml title="community-mysql-mysql_db.yaml" linenums="1"

    --8<-- "tasks/modules/community-postgresql-postgresql_db.yaml"

    ```
