# role-aap-object-credential
Ansible role to build AAP workflow objects

# Ref
https://console.redhat.com/ansible/automation-hub/repo/published/ansible/controller/content/module/schedule/

# How to use

Step 1: Install the role in your environment.
   - You could have roles/requirements.yml if running on AAP.
   - Or simple install on your environment.

Step 2: Define your variables in the structure below

build: true/false # Bool value to switch role on off.

in_list_name:
   - name
   - image
   - description
   - credential
   - validate_certs
   - pull

Step 3: Call the role from your playbook.

# Example

## varible definition in group_vars/*.yml

private_quay_namespace: aap-exec-env
private_quay_host: "{{ lookup('env', 'PRIVATE_QUAY_HOST') }}"
private_quay_root_name: "{{ private_quay_host }}/{{ private_quay_namespace }}"
private_quay_credential_name: "{{ private_quay_credential[0] }}"
validate_certs: false
pull_state: "always"

f5_exec_env_name: f5-exec-env
custom_network_exec_env_name: custom-network-exec-env


f5:
  - "{{ f5_exec_env_name }}"
  - "{{ private_quay_root_name }}/{{ f5_exec_env_name }}"
  - "{{ common_description }}"
  - "{{ private_quay_credential_name }}"
  - "{{ validate_certs }}"
  - "{{ pull_state }}"

custom_network:
  - "{{ custom_network_exec_env_name }}"
  - "{{ private_quay_root_name }}/{{ custom_network_exec_env_name }}"
  - "{{ common_description }}"
  - "{{ private_quay_credential_name }}"
  - "{{ validate_certs }}"
  - "{{ pull_state }}"
  
## playbook

---
- name: Playbook to configure AAP
  hosts: localhost
  gather_facts: false
 
  pre_tasks:
    - name: Include specific project variables
      ansible.builtin.include_vars:
        dir: group_vars

  tasks:
    !
    !
    - name: Import role-aap-object-exec-env
      ansible.builtin.include_role:
        name: role-aap-object-exec-env
      vars:
        build: true
        in_list: "{{ item }}"
      loop:
        - "{{ f5 }}"
        - "{{ custom_network }}"
        - "{{ cins_network }}"
        - "{{ fortios }}"
        - "{{ junos }}"
        - "{{ mcn }}"
    !
    !
