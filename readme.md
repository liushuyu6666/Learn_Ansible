- [Overview](#overview)
  - [Keywords](#keywords)
- [Raspberry Pi Setup Guide](#raspberry-pi-setup-guide)
  - [Preparing the Control Node (MacBook):](#preparing-the-control-node-macbook)
  - [Preparing the Raspberry Pi Ubuntu Server:](#preparing-the-raspberry-pi-ubuntu-server)
  - [Verifying the SSH Connection:](#verifying-the-ssh-connection)
  - [Configuring `~/.ssh/authorized_keys` on Raspberry Pi](#configuring-sshauthorized_keys-on-raspberry-pi)
  - [Completing SSH Public-Key Authentication](#completing-ssh-public-key-authentication)
- [Troubleshooting](#troubleshooting)
  - [SSH connection](#ssh-connection)

# Overview
This project is to learn Ansible in practice, for effective learning and practice of Ansible, 3 Raspberry Pis powered by Ubuntu Server (64-bit) serve as managed nodes, while a MacBook with Ansible installed acts as the control node. All devices, including the **Raspberry Pis (act as managed nodes)** and the **MacBook (acts as control node)**, must operate within the same network, sharing a common router for testing and practice purposes.

## Keywords
1. Ubuntu
2. Ansible
3. Raspberry Pi (ARM based)

# Raspberry Pi Setup Guide
For a streamlined Raspberry Pi environment setup, follow the provided [Raspberry Pi setup instruction](https://github.com/liushuyu6666/Knowledge/tree/master/RaspberryPi). However, in this project, all Raspberry Pi devices are configured for key-based authentication only, with password-based authentication disabled. To achieve this, the following instructions are needed.


## Preparing the Control Node (MacBook):
1. Generate an SSH key pair on the Control Node (MacBook):
    1. Copy the public key once generated.
    2. Note the location of the private key.
## Preparing the Raspberry Pi Ubuntu Server: 
1. In the Raspberry Pi Imager:
   1. Under SSH settings, select "Allow public-key authentication only" to disable password-based authentication.
   2. Paste the public key into the "Set authorized_keys" field.
   3. Set the hostname and username as "raspberrypi1", "raspberrypi2" or "raspberrypi3".
   4. Insert the SD card into the Raspberry Pi.
## Verifying the SSH Connection: 
1. Check the SSH connection from MacBook (Control Node) to the Raspberry Pi (managed node) via password-based SSH in the terminal:
   1. Ensure proper permissions on the SSH private key file on the Control Node (MacBook):
     ```bash
     chmod 600 </path/to/your/private_key>
     ```
   2. Obtain the IP address of the Raspberry Pi. [Follow this instruction](https://github.com/liushuyu6666/Knowledge/blob/master/RaspberryPi/Readme.md#scan-the-raspberry-pi-on-the-local-network).
   3. Establish a password-based SSH connection from MacBook using the command:
     ```bash
     ssh <raspberry_pi_username>@<raspberry_pi_ip>
     ```
   4. A password is required at this stage since `~/.ssh/authorized_keys` on managed nodes (Raspberry Pi) is not yet configured.
## Configuring `~/.ssh/authorized_keys` on Raspberry Pi
1. Set up the `~/.ssh/authorized_keys` on managed nodes (Raspberry Pi):
   1. Typically, `~/.ssh/authorized_keys` on the Raspberry Pi is empty.
   2. Copy the public key from the Control Node to Raspberry Pis using:
    ```bash
    ssh-copy-id -i </path/to/your/public_key.pub> <raspberry_pi_username>@<raspberry_pi_ip>
    ```
   3. Ensure correct permissions on `.ssh` and `authorized_keys` on Raspberry Pis:
    ```bash
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys
    ```
   4. Once `~/.ssh/authorized_keys` is successfully configured, you won't need a password for SSH connections. Use:
    ```bash
    ssh -i </path/to/your/private_key> <raspberry_pi_username>@<raspberry_pi_ip>
    ```
## Completing SSH Public-Key Authentication
1. Complete SSH public-key authentication configuration on all Control and Managed Nodes. You can configure the Ansible inventory file for each project later:
   1. Ensure the IP addresses of all Raspberry Pis are added to the `~/.ssh/known_hosts` on MacBook. This should be completed automatically in step 3.
   2. Confirm that the public key is added to the `~/.ssh/authorized_keys` on the Raspberry Pi. This should be completed in step 4.

By following these steps and recording the IP addresses of your Raspberry Pis, you'll have a functional Ansible environment for learning and practical exercises in this project.

# Troubleshooting
## SSH connection
If you encounter the 'WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!' error when attempting to connect to your Raspberry Pi (the managed node) from your control node (MacBook), you can resolve this issue by taking the following steps:
1. Remove the Old Host Key Entry from the control node: To remove the old host key entry from your local `~/.ssh/known_hosts` file, use the following command:
```bash
ssh-keygen -R <your_raspberry_pi_ip>
```
2. Accept the New Key: After removing the old host key, you should accept the new key by connecting to the Raspberry Pi again using the updated key. Use the following command format:
```bash
ssh -i </path/to/your/private_key> <raspberry_pi_username>@<raspberry_pi_ip>
```
