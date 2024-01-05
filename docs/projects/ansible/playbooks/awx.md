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

- Workflow Templates from code

!!! note Info
    This playbook adds items ONLY. maybe in the future it will remove items no longer within the project. Merge requests welcome.

!!! tip
    Cloning a repository that requires credentials is possible if you put credentials in your `~/.gitconfig` file. If your using Ansible Automation Platform / AWX on playbook import a custom Git credential will be created that will setup the `~/.gitconfig` file, that you can use in job templates.

!!! danger "Security"
    As there is an expectation that git credentials will be saved to `~/.gitconfig` for use when cloning private repositories. It's strongly recommended that you assess if this is viable within your threat model. The stance taken within this role is that credentials would only be saved to `~/.gitconfig` when running the play within an execution environment container that would dissapear post the job running.


## Playbook AWX / Ansible Automation Platform Template Import

This playbook includes the AWX feature where it imports the playbook as job templates in to AWX / Ansible Automation Platform. The following job templates that will be created:

- **Configure/AWX/All** Configures all AWX items/settings available within the inventory

- **Configure/AWX/Credential Type** Configures AWX Credential Type(s)

- **Configure/AWX/Instance** Configures AWX Instance(s)

- **Configure/AWX/Instance Groups** Configures AWX Instance Group(s)

- **Configure/AWX/Job Template** Configures AWX Job Tempalte(s)

- **Configure/AWX/Organization** Configures AWX Organization(s)

- **Configure/AWX/Project** Configures AWX Project(s)

- **Configure/AWX/Settings** Configures AWX Settings

- **Configure/AWX/Workflow Template** Configures Workflow Template(s)



## Configure ALL

- Job tag is `complete`

This play utilizes the variables from each listed section below.

``` yaml
my_awx_config: {}    # Mandatory, dict. The AWX configuration
```

Prior to the play completing the following artifact/stats are set:

``` json
{}
```


## Credential Type

- Job tag is `credential_types`

This play utilizes the following variables.

``` yaml
my_awx_config:
  credential_types:       # Mandatory, list of dict.
    - name: ...
      ........
```

The dict keys are derived from Ansible module `awx.awx.credential_type`. please refer to the module [documentation](https://docs.ansible.com/ansible/latest/collections/awx/awx/credential_type_module.html).


Prior to the play completing the following artifact/stats are set:

``` json
{}
```


## Instance

- Job tag is `instances`

This play utilizes the following variables.

``` yaml
my_awx_config:
  instances:       # Mandatory, list of dict.
    - hostname: ...
      ........
```

The dict keys are derived from Ansible module `awx.awx.instance`. please refer to the module [documentation](https://docs.ansible.com/ansible/latest/collections/awx/awx/instance_module.html#ansible-collections-awx-awx-instance-module).


Prior to the play completing the following artifact/stats are set:

``` json
{}
```


## Instance Groups

- Job tag is `instance_groups`

This play utilizes the following variables.

``` yaml
my_awx_config:
  instance_groups:       # Mandatory, list of dict.
    - name: ...
      ........
```

The dict keys are derived from Ansible module `awx.awx.instance_group`. please refer to the module [documentation](https://docs.ansible.com/ansible/latest/collections/awx/awx/instance_group_module.html).


Prior to the play completing the following artifact/stats are set:

``` json
{}
```


## Job Templates

- Job tag is `job_templates`

This play utilizes the following variables.

``` yaml
my_awx_config:
  organizations:                    # Mandatory, list of dict.
    - name: ""                      # Mandatory, string. An organization that exists within AWX.

      projects:                     # Mandatory, list of dict.
        - name: ""                  # Mandatory, string. A project that exists within AWX.
          playbook_path: ""         # Mandatory, string. Project root directory relative path to location of playbooks
          scm_url: ""               # Mandatory, string. the full git clone url
          scm_branch: ""            # mandatory, string. The git branch to checkout when cloning repo

          rbac:                     # Optional, dict.
            job:                    # Optional, list of dict. RBAC Permissions for the job templates imported.
              - name: Technician
                type: team
                state: present
                role: read
      ........
```

Prior to the play completing the following artifact/stats are set:

``` json
{}
```


### Playbook import as AWX / Ansible Automation Platform Job Template

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


## Organizations

- Job tag is `organization`

This play utilizes the following variables.

``` yaml
my_awx_config:
  organizations:                    # Mandatory, list of dict.
    - name: ...
      .....
      teams:
        - name: ""
          description: ""
      
      rbac:
        # Valid Roles for org:
        # admin, read, member, execute, adhoc, update, use, approval, auditor, project_admin, inventory_admin,
        # credential_admin, workflow_admin, notification_admin, job_template_admin, execution_environment_admin
        - name: ""
          type: team
          state: present
          role: credential_admin

```

The dict keys are derived from Ansible module `awx.awx.organization`. please refer to the module [documentation](https://docs.ansible.com/ansible/latest/collections/awx/awx/organization_module.html).


Prior to the play completing the following artifact/stats are set:

``` json
{}
```


## Projects

- Job tag is `project`

This play utilizes the following variables.

``` yaml
my_awx_config:
  organizations:          # Mandatory, list of dict.
    - name: ""            # Mandatory, string. An organization that exists within AWX.

      projects:
        - name: ...
      .....

```

The dict keys are derived from Ansible module `awx.awx.project`. please refer to the module [documentation](https://docs.ansible.com/ansible/latest/collections/awx/awx/project_module.html).


Prior to the play completing the following artifact/stats are set:

``` json
{}
```


## Settings

- Job tag is `settings`

This play utilizes no variables as it uses jinja templates. To configure standard AWX Settings template `{{ project_dir }}/templates/groups/awx/config-awx-system.json.j2` and to configure SAML settings template `{{ project_dir }}/templates/groups/awx/config-awx-authentication_saml.json.j2` is used.

Prior to the play completing the following artifact/stats are set:

``` json
{}
```


## Workflow Templates

- Job tag is `workflow_templates`

This play utilizes the following variables.

``` yaml
my_awx_config:
  organizations:                    # Mandatory, list of dict.
    - name: ""                      # Mandatory, string. An organization that exists within AWX.

      projects:                     # Mandatory, list of dict.
        - name: ""                  # Mandatory, string. A project that exists within AWX.
          workflow_path: ""         # Mandatory, string. Project root directory relative path to location of playbooks
          scm_url: ""               # Mandatory, string. the full git clone url
          scm_branch: ""            # mandatory, string. The git branch to checkout when cloning repo

          rbac:                     # Optional, dict.
            job:                    # Optional, list of dict. RBAC Permissions for the job templates imported.
              - name: Technician
                type: team
                state: present
                role: read
      ........
```

Prior to the play completing the following artifact/stats are set:

``` json
{}
```






## To Do

- add details on how job template creation works