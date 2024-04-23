---
title: Roles
description: No Fuss Computings Ansible playbook Roles
date: 2024-04-24
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This playbook is designed to fetch a device's/virtual machine's rendered configuration from NetBox and run an ansible role that is mapped to the NetBox device/virtual machine role.


## Playbook AWX / Ansible Automation Platform Template Import

This playbook includes the [AWX feature](awx.md) where it imports the playbook as job templates in to AWX / Ansible Automation Platform. The following job templates that will be created:

- **Playbook/Role** Setup a hosts configured role


## Requirements

- The inventory must contain the following variables: `role` and `serial` which the value of `serial` must match those within NetBox.

    !!! tip
        The `netbox.netbox.nb_inventory` inventory plugin does contain the required variables.

- NetBox has been setup with the required custom fields. 

    !!! info
        You can use our Ansible Role `nofusscomputing.kubernetes.kubernetes_netbox` to setup NetBox with the required fields. See [Documentation for more info](../collection/kubernetes/roles/kubernetes_netbox/index.md).

- Rendered configuration contains the required variables for the Ansible Role being used.

    !!! info
        We provide [publicly accessible templates](https://gitlab.com/nofusscomputing/infrastructure/configuration-management/netbox/-/tree/development/templates) for our roles, should you wish not to create your own.


## Usage

This playbook requires the following variables be set.

``` yaml

role_map:                                              # Mandatory, Dict.
  kubernetes_node:                                     # Mandatory, String. Netbox device/Virtual Machine role slug
    name: nofusscomputing.kubernetes.nfc_kubernetes    # Mandatory, String. Name of the Ansible Role that will be run
    tasks_from: main                                   # Optional, String. Name of the task file within the role that will be used.

```

Environmental variables `NETBOX_API` and `NETBOX_TOKEN`, must be set for the url (with protocol) and token to access NetBox.

The remaining required variables that must be set are those that are required by the Ansible Role. These variables must be part of the device/virtual machine rendered configuration.


### Workflow

This playbook has the following workflow:

1. Confirm environmental variables `NETBOX_API` and `NETBOX_TOKEN` are set

1. Fetching of the host (Device / Virtual Machine) ID from NetBox using `inventory_hostname` and `serial` as the filter

1. Fetching of the hosts rendered config

1. Saves the rendered config to a tmp file

1. Load tmp file (load variables into `hostvars`)

1. Removes tmp file

1. _Role `kubernetes_node` ONLY_ Confirms required variables are set

1. Runs the role as specified in `role_map`


## Playbook Definition

``` yaml title="role.yaml" linenums="1"

--8<-- "role.yaml"

```