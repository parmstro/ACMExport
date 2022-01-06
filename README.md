# README.md
## ACM Export
This project assumes an advanced knowledge of Ansible Automation Platform 2.1+ and Red Hat Satellite 6.9+. Without the experience, you will problably just get frustrated...

This project contains the code and the export for the Automating Content Management Blog. It is expected that you have read the blog and / or reviewed the code for the ACM project. The code in this project is used to import and export the configuration of the ACM Credentials, Projects, Inventories and Templates. Edit the variables mentioned below and run the ImportConfig.yml playbook to create your required assets in Ansible Automation Platform. Once you have done that, you can make any changes that you want using the AAP Web UI or commands and backup your configuration with the ExportConfig.yml playbook.

### How to use:

Requirements:
- a functioning Ansible Automation Platform 2.1 Controller
- administrative privilege to the above controller

Assumptions:
- you are using a git based SCM tool. This is not a requirement, but if you are using something else, you will have to make more changes to the configuration than are listed here. You can ping me as needed.
- it bears repeating, you have read the blog or reviewed the code for the Automating Content Management project. :-)

Basic steps:
- clone the ACM repo
- clone the ACMExport repo
- set the variables in the vault file
- update the project URLs
- run the Import playbook

In the group_vars directory are the all.yml and vault_all.yml files. The vault_all.yml file contains the sensitive variables you must replace this file with your own. In the repo, this file contains no values and looks something like:
```
---
vault_tower_username: ""
vault_tower_password: ""
vault_default_credential_become_method: ""
vault_default_credential_become_username: ""
vault_default_credential_username: ""
vault_default_credential_password: ""
vault_towergithub_credential_username: "" 
vault_towergithub_credential_password: ""
vault_ACM_vault_credential_vault_id: ""
vault_ACM_vault_credential_vault_password: ""
```
Before running the import playbook, set the value of these variables.

You all know how to use ansible-vault encrypt/decrypt and know how to call you playbook and specify your secret files when running the above playbooks. You could also configure this project in AAP as well... getting very circular though...

Also, in the all.yml change the variable below to reflect your cloned ACM repo
```
acm_project_url: 
```
Review the files and make changes to suit your environment. 

If you have any questions or issues, please reach out or use the available github tools.

