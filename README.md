# ocp_auth

## Description

Ansible role to generate manage Openshift Auth token.
It allow you to create/revoke a token.

By default when calling role a token is created. Add ```ocp_auth_revoke: true``` to revoke the generated token.

# Usage

Create playbook:

```
cat << EOF >> auth_playbook.yml
---
- hosts: localhost
  connection: local
  gather_facts: false
  become: false
  module_defaults:
    - redhat.openshift.openshift_auth:
        host: "{{ ocp_auth_api }}"
        validate_certs: "{{ ocp_auth_validate_certs }}"
    - kubernetes.core.k8s_info:
        host: "{{ ocp_auth_api }}"
        validate_certs: "{{ ocp_auth_validate_certs }}"
        api_key: "{{ ocp_auth_api_key }}"
    - kubernetes.core.k8s:
        host: "{{ ocp_auth_api }}"
        validate_certs: "{{ ocp_auth_validate_certs }}"
        api_key: "{{ ocp_auth_api_key }}"
    - kubernetes.core.k8s_exec:
        host: "{{ ocp_auth_api }}"
        validate_certs: "{{ ocp_auth_validate_certs }}"
        api_key: "{{ ocp_auth_api_key }}"
  vars:
    ocp_auth_api_key: ""
  tasks:
    - block:
        - name: Get Auth Token
          include_role: ocp_auth
      always:
        - name: Authentification OCP
          ansible.builtin.include_role: ocp_auth
          vars:
            - ocp_auth_revoke: true
EOF
```

Execute:

```
ansible-playbook -i inventory auth_playbook.yml --ask-vault-pass
```

By default when calling role a token is created. Add ```ocp_auth_revoke: true``` to revoke the generated token.
