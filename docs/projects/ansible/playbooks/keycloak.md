---
title: Keycloak
description: No Fuss Computings Ansible playbook Keycloak
date: 2023-12-31
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This playbook is designed to run Automations against Keycloak.


## Playbook AWX / Ansible Automation Platform Template Import

This playbook includes the [AWX feature](awx.md) where it imports the playbook as job templates in to AWX / Ansible Automation Platform. The following job templates that will be created:

- **Keycloak/Configure/From Inventory** Configure a Keycloak instance from inventory.

On import to AWX / Ansible Automation Platform a credential type will also be created, `playbook/keycloak/api` that can be used to supply the required secrets and Keycloak host.


## Configure Keycloak from Inventory

- Job tag not required

This task configures a keycloak instance from config stored within your inventory.

This play utilizes the following variables.

``` yaml
nfc_pb_keycloak_auth_keycloak_url: https://keycloak.local  # Mandatory, string. Keycloak URL.
nfc_pb_keycloak_auth_realm: master                         # Mandatory, string. Keycloak Auth realm
nfc_pb_keycloak_auth_username: admin                       # Mandatory, string. Keycloak user name
nfc_pb_keycloak_auth_password: admin                       # Mandatory, string. Keycloak password

keycloak_configuration: []                                 # Mandatory, list. Keycloak configuration
```

Prior to the play completing the following artifact/stats are set:

``` json
{
  
}

```


## Playbook Definition

``` yaml title="keycloak.yaml" linenums="1"

--8<-- "keycloak.yaml"

```