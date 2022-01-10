# README.md
## ACM Export
This project assumes an advanced knowledge of Ansible Automation Platform 2.1+ and Red Hat Satellite 6.9+. Without the experience, you will problably just get frustrated...

This is a helper project contains the code for recreating the Automating Content Management Blog AAP configuration. It is expected that you have read the blog and / or reviewed the code for the [AutomatingContentManagement](https://github.com/parmstro/AutomatingContentManagement) project. The code in this project is used to import and export the configuration of the credentials, projects, inventories and templates that make up the workflow. 

**PLEASE NOTE:** These assets contain ansible extra variables related to the AutomatingContentManagement project that are specific to the blog and original project. These include hostnames, names of content views and composite content views in Satellie, etc.. It is not intended that this project makes a no-effort configuration of AAP. It is intended that this project reduces the effort to setup the blog environment and follow along. The [AutomatingContentManagment wiki](https://github.com/parmstro/AutomatingContentManagement/wiki) goes into the detail of each template and other assets that are required for making **your** environment work. You are expected to have read the blog post before using this project. 


Edit the variables mentioned below and run the ImportConfig.yml playbook to create your required assets in Ansible Automation Platform. Once you have done that, you can make any changes that you want using the AAP Web UI or commands and backup your configuration with the ExportConfig.yml playbook.

### How to use:

Requirements:
- a functioning Ansible Automation Platform 2.1 Controller
- administrative privilege to the above controller

Assumptions:
- you are using a git based SCM tool. This is not a requirement, but if you are using something else, you will have to make more changes to the configuration than are listed here. You can ping me as needed.
- it bears repeating, you have read the blog or reviewed the code for the Automating Content Management project. :-)

Basic steps:
- Fork and clone locally the [AutomatingContentManagement](https://github.com/parmstro/AutomatingContentManagement) repo
- Fork and clone locally the [ACMExport](https://github.com/parmstro/ACMExport) repo
- Rename group_vars/vault_all.yml.sample to group_vars/vault_all.yml and set the variables (see below for the variables and what they are for)
- Rename group_vars/all.yml.sample to group_vars/all.yml and set the variables (e.g. update the project URLs)
- Update the inventory file
- Run the Import playbook: ImportConfig.yml
- Edit the resulting Inventories and Job templates to match your Satellite and AAP environment
- Run the ExportConfig.yml play to save your configuration.

```
ansible-playbook -i <inventory> \
                 --vault-password-file <path> \
                 -e 'ansible_python_interpreter=/usr/bin/python3' \
                 ImportConfig.yml 
```

In the group_vars directory are the all.yml and vault_all.yml files. Both files require a bit of editing.

The all.yml file contains all the variables that are used in the project. The two main values that need to be updated are the controller_host value and the acm_project_url value. 

Where there are secrets, the variable references another variable with a vault_* prefix. These variables are defined in the vault_all.yml file. The file contains the sensitive data you don't want to share. It allows you to copy the code easily without have to hunt for secrets - simply update vault_all.yml and you should be good from a secrets perspective. In the repo, this file contains no values and looks something like:
```
---
# These are the credentials ACMExport will use to 
# connect to your controller instance to run an import or export
vault_tower_username: ""
vault_tower_password: ""

# These are the values for your default machine credential. 
# The credential the controller typically uses to connect to systems you are automating
vault_default_credential_become_method: ""
vault_default_credential_become_username: ""
vault_default_credential_username: ""
vault_default_credential_password: ""

# These are the values that the controller uses to 
# connect to your fork of AutomatingContentManagement i.e. your github login and PAT
vault_towergithub_credential_username: "" 
vault_towergithub_credential_password: ""

# These are the values that you are using to protect 
# *all* of your vault files in AutomatingContentManagement
# In the blog I mention that this was done for simplicity 
# normally we would have vault credentials managed by each project
vault_ACM_vault_credential_vault_id: ""
vault_ACM_vault_credential_vault_password: ""
```
Before running the import playbook, set the value of these variables.

You all know how to use ansible-vault encrypt/decrypt and know how to call you playbook and specify your secret files when running the above playbooks. You could also configure this project in AAP as well... getting very circular though...

Also, in the all.yml change the variable below to reflect your forked AutomatingContentManagement repo
```
acm_project_url: 
```
Review the files and make changes to suit your environment. 

If you are having trouble with a particular asset, you can try to import only a single asset type using:
```
ansible-playbook -i <inventory> \
                 --vault-password-file <path> \
                 --e path=<path_to_asset_yml> \
                 import_individual_asset.yml
```

If you have any questions or issues, please reach out or use the available github tools.

