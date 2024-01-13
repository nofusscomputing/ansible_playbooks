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
