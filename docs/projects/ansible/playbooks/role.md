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

- **Playbook/Role/Ansible** Setup a hosts configured role

- **Playbook/Role/Helm Chart** Deploy a Helm Chart

- **Playbook/Role/Kubernetes Manifest** Deploy a Kubernetes Manifest


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

This playbook is broken up into different role types, they are:

- Ansible Role

- Helm Chart

- Kubernetes Manifest


### Ansible Role

- job tag `ansible_role`

This playbook requires the following variables be set.

``` yaml

role_map:                                              # Mandatory, Dict.
  kubernetes_node:                                     # Mandatory, String. Netbox device/Virtual Machine role slug
    name: nofusscomputing.kubernetes.nfc_kubernetes    # Mandatory, String. Name of the Ansible Role that will be run
    tasks_from: main                                   # Optional, String. Name of the task file within the role that will be used.

```

Environmental variables `NETBOX_API` and `NETBOX_TOKEN`, must be set for the url (with protocol) and token to access NetBox.

The remaining required variables that must be set are those that are required by the Ansible Role. These variables must be part of the device/virtual machine rendered configuration.


### Helm Chart

On the Ansible Controller, helm must be installed as must the PyYaml Python module.

- job tag `helm_chart`

This playbook requires the following variables be set.

``` yaml

role_map:                                                # Mandatory, Dict.
  nginx_ingress:                                         # Mandatory, String. Chart Name
    name: nginx                                          # Mandatory, String. Helm deployment name
    repo: 
      name: nginx                                        # Mandatory, String. Name to give the repository
      url: https://kubernetes.github.io/ingress-nginx    # Mandatory, String. Helm Chart repository URL
    chart: ingress-nginx                                 # Mandatory, String. Name of the chart withing the helm repo.
    version: '4.8.2'                                     # Mandatory, String. Chart version to deploy
    namespace: ingress                                   # Optional, String. Kubernetes namespace to deploy chart to.
    create_namespace: true                               # Optional, String. Create Namespoace?
    release_values:                                      # Optional, Dict. Chart Values.
    # Optional, String. Template filename for chart values
    template_file: "{{ inventory_dir + '/../../templates/helm-chart-values/nginx.yaml.j2'}}"
```

!!! tip
    AS a helm repository can contain multiple helm charts, keeping the `repo` dictionary the same across different helm role is recommended so that you don't end up with multiple helm repositories pointing to the same content.

The following environmental variables must be set so that the ansible controller can connect to the kubernetes host:

- `K8S_AUTH_HOST`, `K8S_AUTH_API_KEY`, `K8S_AUTH_SSL_CA_CERT` and optionally `K8S_AUTH_VERIFY_SSL`

or

- `K8S_AUTH_KUBECONFIG`

The remaining required variables that must be set are those that are required by the template file if specified. These variables must be part of the device/virtual machine rendered configuration or included in the Ansible Inventory.


### Kubernetes Manifest

On the Ansible Controller, kubectl must be installed as must the PyYaml and jsonpatch Python module.

- job tag `kubernetes_manifest`

This playbook requires the following variables be set.

``` yaml

role_map:                                                # Mandatory, Dict.
  ingress_my_website:                                    # Mandatory, String. Chart Name
    name: The ingress for my website                     # Mandatory, String. Arbitrary name.
    state: present                                       # Optional, String. present or absent
    # Mandatory, String. Template filename containing the kubernetes manifest.
    template: "{{ inventory_dir + '/../../templates/kubernetes/my_website.yaml.j2'}}"
```

The following environmental variables must be set so that the ansible controller can connect to the kubernetes host:

- `K8S_AUTH_HOST`, `K8S_AUTH_API_KEY`, `K8S_AUTH_SSL_CA_CERT` and optionally `K8S_AUTH_VERIFY_SSL`

or

- `K8S_AUTH_KUBECONFIG`

The remaining required variables that must be set are those that are required by the template file if specified. These variables must be part of the device/virtual machine rendered configuration or included in the Ansible Inventory.


### Workflow

This playbook has the following workflow:

1. Confirm environmental variables `NETBOX_API` and `NETBOX_TOKEN` are set

1. Fetching of the host (Device / Virtual Machine) ID from NetBox using `inventory_hostname` and `serial` as the filter

1. Fetching of the hosts rendered config

1. Saves the rendered config to a tmp file

1. Load tmp file (load variables into `hostvars`)

1. Removes tmp file

1. Variable Validation

    - _Role `kubernetes_node`_ Confirms required variables are set

1. Runs the role as specified in `role_map`


## Playbook Definition

``` yaml title="role.yaml" linenums="1"

--8<-- "role.yaml"

```