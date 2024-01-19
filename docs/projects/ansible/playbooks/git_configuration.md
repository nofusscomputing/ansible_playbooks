---
title: Git Configuration
description: How to use No Fuss Computings Ansible Playbook git_configuration
date: 2023-05-27
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

!!! Note
    Docs Still under development

This playbook is intended to run our ansible role [git_configuration](../../git_configuration/index.md).


## AWX / Ansible Automation Platform

This playbook is setup to tak advantage of configuring AWX via the [awx playbook / Job Template](awx.md). The following job templates will be created:

- Configure Gitlab / Github Repositories

    > Configure Gitlab / Github repositories from code

- Webhook/gitlab/Create Artifact

    > After receiving a Gitlab webhook, Create an Artifact with the webhook data.


## Docs ToDo

- specify cli with tags and variables

- "See it in action at" list

- link to git_configuration docs including specifically the variables/tags for usage.


## Playbook Definition

``` yaml title="git_configuration.yaml" linenums="1"

--8<-- "git_configuration.yaml"

```
