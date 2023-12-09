---
title: Common Playbook
description: No Fuss Computings Ansible playbook
date: 2023-12-09
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This playbook is designed to run the nfc_common ansible role. Documentation for the role is [available here.](../roles/common/index.md)


## Playbook AWX / Ansible Automation Platform Template Import

This playbook includes the [AWX feature](awx.md) where it imports the playbook as job templates in to AWX / Ansible Automation Platform. There are two job templates that will be created `Host Stats Collection` and `Common`

- **Common** is designed to run the nfc_common role and will use the configuration from your inventory

- **Host Stats Collection** This task collects the stats of your hosts within the inventory.