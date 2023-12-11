---
title: GLPI
description: No Fuss Computings Ansible playbook GLPI
date: 2023-12-11
template: project.html
about: https://gitlab.com/nofusscomputing/projects/ansible/ansible_playbooks
---

This playbook is designed to run Automations against GLPI.


## Playbook AWX / Ansible Automation Platform Template Import

This playbook includes the [AWX feature](awx.md) where it imports the playbook as job templates in to AWX / Ansible Automation Platform. The following job templates that will be created:

- **ITSM/GLPI/Ticket/Template/From_ITIL_Category** Provide an ITIL Category and this play will fetch and generate the ticket template.

On import to AWX / Ansible Automation Platform a credential type will also be created, `playbook/glpi/api` that can be used to supply the required secrets and glpi FQDN.


## Ticket Template From ITIL Category

This play utilizes the following variables.

``` yaml
nfc_pb_glpi_host: glpi.hostname.tld      # Mandatory, string. FQDN that forms part of the url. Don't specify http|https.
nfc_pb_glpi_app_token: ''                # Mandatory, string. application token as generated from GLPI.
nfc_pb_glpi_user_token: ''               # Mandatory, string. user token as generated from GLPI.
nfc_pb_glpi_itil_category: 4             # Mandatory, integer. The ITIL Category to use from GLPI.
nfc_pb_glpi_ticket_type: request         # Mandatory, string. Choice incident|request
nfc_pb_glpi_no_log_sensitive_data: true  # Optional, boolean. used to turn `no_log` on/off for logging sensitive data
                                         # NOTE: Sensitive data will be logged. i.e. user and app token.
```

Prior to the play completing the following artifact/stat is set:

``` json
{
  "nfc_pb_glpi": {
    "ticket":{
      "template": {},
      "mandatory_fields": []
    }

  }
}
```
> `template` is the generated ticket template in the same format as would be used to send to the GLPI API  
> `mandatory_fields` is a list of field names that are set as required from the ticket template.

## Playbook Definition

``` yaml title="common.yaml" linenums="1"

--8<-- "glpi.yaml"

```