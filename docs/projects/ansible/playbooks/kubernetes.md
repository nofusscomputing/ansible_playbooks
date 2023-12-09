---
title: Kubernetes
description: No Fuss Computings Ansible playbook Kubernetes
date: 2023-10-29
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

!!! Note
    Docs Still under development

flow:
1. deploy Kubernetes to prime master node

1. deploy Kubernetes to remaining master nodes

1. Copy Manifest(s) and Expand Templated Manifests to prim master nodes manifest directory.

    > These not the manifests required to setup the cluster. They are the manifests for what's deployed to the cluster.

1. deploy Kubernetes to worker nodes.


## Ansible AWX / Tower / Automation Platform

If you use Ansible  AWX / Tower / Automation Platform you can use our [AWX Playbook](awx.md) to automagically import this role for use in the web interface.

| | Detail|
|:---|:---:|
| import | yes |
| name | kubernetes |