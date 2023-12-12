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

- **ITSM/GLPI/Ticket/Create** Create a ticket in GLPI

- **ITSM/GLPI/Ticket/Task/Create** Create a ticket task

On import to AWX / Ansible Automation Platform a credential type will also be created, `playbook/glpi/api` that can be used to supply the required secrets and glpi FQDN.


## Ticket Template From ITIL Category

- Job tag is `ticket_template_from_itil_category`

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

Prior to the play completing the following artifact/stats are set:

``` json
{
  "nfc_pb_glpi": {
    "ticket":{
      "template": {},         // dict. generated ticket template in the same format as would be used to send to the GLPI API 
      "mandatory_fields": []  // list. field names that are set as required from the ticket template.
    }
  }
}
```


## Create Ticket

- Job tag is `ticket_create`

!!! tip
    Aditionally you can specify tag `ticket_template_from_itil_category` to collect the ticket template from the ITIL category which will be used to create the ticket.

This play utilizes the following variables.

``` yaml
nfc_pb_glpi_host: glpi.hostname.tld      # Mandatory, string. FQDN that forms part of the url. Don't specify http|https.
nfc_pb_glpi_app_token: ''                # Mandatory, string. application token as generated from GLPI.
nfc_pb_glpi_user_token: ''               # Mandatory, string. user token as generated from GLPI.
nfc_pb_glpi_ticket_create:               # Mandatory, dict. The ticket body in API Format.
  name: ""                               # Mandatory, string. Title of the ticket. If using ticket template will be appended to existing.
  content: ""                            # Mandatory, string. The ticket description. If using ticket template will be appended to existing.
  entities_id: 0                         # Mandatory, integer. entities ID for ticket to be created in.
  type: 1                                # Mandatory, choice. 1=incident|2=Request
  itilcategories_id: 0                   # Mandatory*, integer. the ticket category. ONLY mandatory for create
nfc_pb_glpi_no_log_sensitive_data: true  # Optional, boolean. used to turn `no_log` on/off for logging sensitive data
                                         # NOTE: Sensitive data will be logged. i.e. user and app token.
```

Prior to the play completing the following artifact/stats are set:

``` json
{
  "nfc_pb_glpi": {
    "ticket":{
      "assembled": {},   // dict. Assembled ticket in API format
      "create": {},      // dict. contents of 'nfc_pb_glpi_ticket_create' variable
      "items_id": []     // list. List of items to be assosiated to a tickeck on creation in API format
    }
  }
}
```


## Create Ticket Task

- Job tag is `ticket_task_create`

!!! tip
    Aditionally you can specify tags `ticket_template_from_itil_category` and `ticket_create`to collect the ticket template from the ITIL category which will be used to create the ticket, with the ticket task being created as the last step.

This play utilizes the following variables.

``` yaml
nfc_pb_glpi_host: glpi.hostname.tld      # Mandatory, string. FQDN that forms part of the url. Don't specify http|https.
nfc_pb_glpi_app_token: ''                # Mandatory, string. application token as generated from GLPI.
nfc_pb_glpi_user_token: ''               # Mandatory, string. user token as generated from GLPI.
nfc_pb_glpi_ticket_task_create:          # Mandatory, dict. The ticket task body in API Format.
  id: 1                                  # Optional, integer. if specified will update the task. NOTE: the tickets_id must be specified
  taskcategories_id: 1                   # Mandatory, integer. task ITIL Category (ONLY Mandatory for create)
  tickets_id: 64                         # Optional, integer. Only required if not creating a ticket first. Mandatory if 'id' specified
  content": ""                           # Mandatory, string. The content of the ticket task.
  state: 1                               # Optional, choice. 0=Information|1=todo|2=Done.
  actiontime: 0                          # Optional, integer. time in seconds for task duration
  is_private: 0                          # Optional, choice 0=public|1=private
  users_id_tech: 0                       # Optional, integer. the user id of the person to assign the task.
nfc_pb_glpi_no_log_sensitive_data: true  # Optional, boolean. used to turn `no_log` on/off for logging sensitive data
                                         # NOTE: Sensitive data will be logged. i.e. user and app token.
```

Prior to the play completing the following artifact/stats are set:

``` json
{
  "nfc_pb_glpi": {
    "ticket":{
      "task": {}    // dict. ticket task in API format
    }
  }
}
```


## Playbook Definition

``` yaml title="common.yaml" linenums="1"

--8<-- "glpi.yaml"

```