# Playbook Tests

``` bash

docker run -ti --rm -v $PWD:/workdir --workdir /workdir nofusscomputing/ansible-ee:dev bash

clear; ansible-playbook -i inventory/hosts.yaml playbooks/test/unit_test.yaml

clear; ansible-playbook playbooks/test/test.yaml -vvv --tags unit

```


## Test setup

- create the directory structure for a project

    - remove dirs not part of playbooks. i.e. the exclude dirs

- copy playbooks in

- copy mock inventory in

- read all playbooks looking for the following tags so as to gather role names

    - role

    - include_role

    - import_role

- copy mock roles in

    - update meta/main/role_name to match role name


## Unit testing


Tests the structure and that our default items are present

- syntax check

- ensure correct objects in playbooks

- each object in the playbook is the correct type. i.e. string, dict list etc

- test for keys that should not exist

    - role



requires:

- mock roles

- mock inventory


## Functional tests

test to ensure that any playbook called completes. the "run test"

- test all playbooks where all mock_roles pass
- test all playbooks where all mock_roles fail
- all playbooks set the artifact variable(s)
- test var types for ansibleunsafe test. (also what is this, find out)


requires:

- mock roles

- mock inventory