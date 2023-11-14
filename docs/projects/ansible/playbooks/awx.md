---
title: AWX / Tower / Automation Platform Playbook
description: No Fuss Computings Ansible playbook
date: 2023-10-23
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This playbook configures an AWX / Tower / Automation Platform instance(s) using configuration from within your inventory.

Items supported for configuration:

- Credential Types

- Organization

- Project

- Inventory

- Inventory Sources

- Roles for access to Inventories, organizations and projects

- Job Templates from playbooks

!!! note Info
    This playbook adds items ONLY. maybe in the future it will remove items no longer within the project. Merge requests welcome.


## Playbook AWX / Tower / Automation Platform Template import

As part of our playbooks we have defined a method wherein you can add a yaml variable to your playbook `yaml` files and this playbook will import them into Ansible AWX / Tower / Automation Platform. This provides the opportunity for job templates from AWX to be stored as code with a full history offered by SCM. 

Example Playbook with the import template variable `vars.nfc_pb_awx_tower_template`:

``` yaml
- name: AWX / Tower Setup
  hosts: localhost
  become: false
  gather_facts: true

  tasks: []


  roles: []


  vars:
    nfc_pb_awx_tower_template:
      - name: "AWX / Tower Configure"
        ask_tags_on_launch: false
        ask_inventory_on_launch: true
        ask_credential_on_launch: true
        description: Configure AWX from Inventory
        execution_environment: "No Fuss Computing EE"
        job_type: "run"
        labels:
          - awx
          - configure
        verbosity: 2
        use_fact_cache: true
        survey_enabled: true
        survey_spec:
        credential_types:
            - name:
              #{rest of credential type fileds here}


```

for a description of the fields under list item `vars.nfc_pb_awx_tower_template` see [awx.awx.job_template module documentation](https://docs.ansible.com/ansible/latest/collections/awx/awx/job_template_module.html). `credential_types` fields under list item `vars.nfc_pb_awx_tower_template[0].credential_types` see [awx.awx.credential_type module documentation](https://docs.ansible.com/ansible/latest/collections/awx/awx/credential_type_module.html).


## To Do

- add details on how job template creation works