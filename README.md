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
example-vars.yml
  
## playbook

example-playbook.yml

