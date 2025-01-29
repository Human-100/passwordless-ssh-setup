# Passwordless SSH Setup

## Overview
This repository demonstrates how to set up a **passwordless SSH connection** between hosts for seamless connectivity and an efficient workflow. This setup is particularly useful for automation tools like **Ansible**, allowing secure and uninterrupted access between machines.

## Prerequisites
- Two machines: **Host machine** (control node) and **Server machine** (target node)
- SSH installed on both machines
- User accounts with appropriate permissions

## Steps to Setup Passwordless SSH

### 1. Generate SSH Key Pair on the Host Machine
First, generate an SSH key pair (public and private key) on the **Host machine**.

```bash
ssh-keygen
```
- When prompted, press **Enter** to accept the default location (`/home/$USER/.ssh/id_rsa`).
- If needed, set a passphrase or leave it empty for passwordless authentication.

### 2. Copy the Public Key to the Server Machine
Find your public key in the following location:

```bash
cat ~/.ssh/id_rsa.pub
```
Copy the output and save it for the next step.

### 3. Configure SSH on the Server Machine
#### a) Generate SSH Key Pair on the Server Machine (Optional)
If the server does not already have an SSH key, generate one:

```bash
ssh-keygen
```

#### b) Add the Public Key from the Host Machine
Open the `authorized_keys` file in the **Server machine** and paste the public key copied from the **Host machine**:

```bash
echo "PASTE_PUBLIC_KEY_HERE" >> ~/.ssh/authorized_keys
```
Alternatively, use `ssh-copy-id` to automate the process:

```bash
ssh-copy-id username@server-ip
```

### 4. Set Correct Permissions
Ensure the correct permissions for SSH keys:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

### 5. Test the SSH Connection
Now, from the **Host machine**, try connecting to the **Server machine** without a password:

```bash
ssh username@server-ip
```
If it logs in without asking for a password, the setup is successful!

## Troubleshooting
- If SSH still prompts for a password, ensure:
  - The `authorized_keys` file exists and has the correct permissions.
  - The SSH service is running (`sudo systemctl restart ssh` on Linux).
  - The SSH configuration allows public key authentication (`PubkeyAuthentication yes` in `/etc/ssh/sshd_config`).
  - SELinux/AppArmor is not blocking SSH (check logs via `journalctl -u ssh` or `/var/log/auth.log`).


##  /\_/\  
## ( o.o ) 
##  > ^ <

