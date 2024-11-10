# Allora Validator Node Playbook

This repository contains an Ansible playbook designed to install, register, and update an Allora Validator Node. The playbook is structured with multiple tasks organized within roles for installation, validation registration, and node updates.

## Structure

### Installation

The installation process consists of several tasks to prepare and set up the system, as well as initialize the Allora node:

- **Prepare Environment**: Prepares the necessary prerequisites for installation.
  
  ```yaml
  - name: Run prepare tasks
    include_tasks: install/prepare.yml
    when: install
    ```


### Execution Commands

Ensure to edit your inventory file before proceeding, specifically setting the correct moniker for each host. 
Example inventory:
  ```yaml
  all:
      hosts:
        111.111.111.111:
          ansible_user: root
          moniker: uniq)name  # Set moniker name here
      vars:
        ansible_python_interpreter: /usr/bin/python3
   ```

### Run Full Playbook

```bash
ansible-playbook -i inventory/hosts.yml main_playbook.yml
```

### Run Specific Tasks with Extra Vars:
- **Installation Only:** 
    ```bash
    ansible-playbook -i inventory/hosts.yml main_playbook.yml -e "install=true register_validator=false update=false"
    ```
- **Registration Only:** 
    ```bash
    ansible-playbook -i iinventory/hosts.yml main_playbook.yml -e "install=false register_validator=true update=false"
    ```
- **Update Node:** 
    ```bash
    ansible-playbook -i inventory/hosts.yml main_playbook.yml -e "install=false register_validator=false update=true"
    ```

### Vars
Configuration Variables

Key configuration variables used across the playbook:

    Installation Control:
        install: Whether to run installation tasks (false by default; set true for initial setup).

    Registration Control:
        register_validator: Controls execution of registration tasks (true by default).

    Update Control:
        update: Whether to update the node (false by default).

    User Settings:
        username: Username for the setup (default: "allora").
        user_password: Password for the user (recommend using Ansible Vault).

    System Directories:
        data_dir: Base directory for data storage (/home/{{ username }}/.allora).
        allora_home: Home directory for Allora (/data).

    Repository Settings:
        repo_url: Allora repository URL.
        repo_branch: Branch of the repo to use.

    Network Configuration:
        amount: Allocation amount for the validator.
        network: Network identifier (e.g., allora-testnet-1).

    Miscellaneous:
        allora_image_version: Docker image version for Allora.


### Important Notes
- **Moniker Configuration:** Ensure each host within your inventory file has a unique moniker assigned as it is crucial for the registration of the validator.
- **Security Practices:** Use Ansible Vault for storing sensitive information like user_password and root_password. Refer to the Ansible Vault documentation for guidance.

