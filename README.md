# Allora Validator Node Playbook

This repository contains an Ansible playbook designed to install, register, and update an Allora Validator Node. The playbook is structured with multiple tasks organized within roles for installation, validation registration, and node updates.

## Structure

### Before installation
- Ensure you have python3 installed.
- Install ansible: `pip3 install ansible`

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

### Run Specific Tasks with Extra Vars:
- **Stage 1 - Install and sync full node:** 
    ```bash
    ansible-playbook -i inventory/hosts.yml main_playbook.yml -e "install=true register_validator=false update=false"
    ```
During this step, node credentials will be downloaded to the /keys folder from the node. 
You will need to top up your wallets with amount specified in variable `amount: "20000000uallo"
` (file /roles/allora/defaults/main.yml).
As soon as you top up your wallets, you can proceed to Stage 2

- **Stage 2 - Registration:** 
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
- **Security Practices:** Use Ansible Vault for storing sensitive information like `user_password`. Refer to the Ansible Vault documentation for guidance.

### Community & Support

- 📱 Telegram: Suseki Capital https://t.me/susekicapital - Follow for updates, guides, and support
- 🐛 Issues: Feel free to submit issues and enhancement requests
- 💡 Contributing: Contributions are welcome! Please check our contributing guidelines

### Support the Project 🚀
If this playbook saved you some time or helped automate your validator setup, consider buying me a coffee:
- SOL: `7ZeiFgCAeSc8QoAjm32fCzNqdPkF6h57YECcCvAdKdW3`
- ETH/ERC20: `0x88a81527a7437edC969Da7CDD5F8f5a9d9cdCF9A`
- TRX/TRC20: `TPTrsTDjUPSpvN4Qn1PMuA2kT13FMFWz1t`

Your support helps maintain and improve this tool. Plus, caffeinated developers write better code! ☕️