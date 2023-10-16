- [Overview](#overview)
- [How to use](#how-to-use)

# Overview
In this project, the objective is to orchestrate the installation and removal of three essential services—`httpd`, `rabbitmq-server`, and `postgresql`—across a network of Raspberry Pis. The setup comprises three Raspberry Pis designated as managed nodes, while the control node is a MacBook. The project involves executing an Ansible playbook on the MacBook to automate the deployment and removal of these services on the Raspberry Pis, checking the disk space and sending a warning email. Gmail is used in this project.

# How to use
1. Install Ansible: Install Ansible on the control node (MacBook).
2. Prepare SSH connections:
   1. Follow the root directory's README instructions to enable SSH connections between the control node (MacBook) and all managed nodes (Raspberry Pi) using public-key authentication.
   2. Collect the IP addresses of all managed nodes.
3. Update inventory:
   1. In the inventory file, update `ansible_host` (managed node's IP address), `ansible_ssh_user` (managed node's username), and `ansible_ssh_private_key_file` (control node).
   2. Verify the connections by running the following command in the `project1` folder:
    ```bash
    ansible all -i inventory -m ping
    ```
4. Prepare Email SMTP Server (Gmail):
   1. Access your "Google Account."
   2. Enable 2-step verification to create "App passwords."
   3. Type "App passwords" in the search bar to enter.
   4. Generate a 16-character code.
   5. Copy the `config.example.yaml` and rename it to `config.yaml`, replace the `email_code` field with the 16-digit code.
5. Execute Tasks:
   1. Run the following commands in the `project1` folder to perform various tasks:
      * To install all necessary servers on 3 hosts:
        ```bash
        ansible-playbook -i inventory install.yaml -e action=verify-install
        ```
      * To check disk space:
        ```bash
        ansible-playbook -i inventory install.yaml -e action=check-disk
        ```
      * To uninstall all servers and complete Project 1:
        ```bash
        ansible-playbook -i inventory uninstall.yaml -e action=uninstall
        ```