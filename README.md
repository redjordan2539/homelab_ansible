# Ansible Project for Homelab

#### WARNING: This repo is highly opinionated and configured specifically for my environment and devices. 
The best use of this repo is as a example for setting up your own Ansible Project

## Roles
This project use several roles to manage devices based on their usage. Below a short summary of all the roles currently in the project.

| Role | Description | Key Tasks |
| :--- | :--- | :--- |
| `common` | Base config for *ALL* hosts | Installs CLI tools for all devices<br>Installs starship<br>Installs Tailscale
| `workstation` | Additional config for *workstation* devices | Install GUI programs<br>Install workstation specific CLI tools<br>Downloads and setups dotfiles
| `tailscale_server` | Configures tailscale for servers | Configures tailscale for devices that use preauth keys
| `tailscale_subnet_router` | Configures tailscale for subnet routers | Configures tailscale for devices that work as subnet routers
| `tailscale_user_device` | configures tailscale for devices for users | Configres tailscale for user devices<br>Uses OIDC login URL to login using google workspace accounts

## Usage
### Requirements
This project requires the following to run correctly
- Ansible (tested on `v2.20.1`)
- Python (tested on `3.14.2`)
- jinja (tested on `3.1.6`)
- kewlfft.aur (for AUR specific tasks. tested on `v0.13.0`)

You will also need to ssh access to all devices listed in `inventory.yaml` to make changes. 

### Secrets Management 
This project uses ansible vault to encrypt certain secret variables. In order to decrypt these variables a vault password file containing the master vault password will need to be created at `~/.vault_pass`

As this is a secret that password will not be provided in this repo. If you do decide to use ansible vaults in your own projects make sure to include them in your `.gitignore` to prevent accidentally committing them to your repo.

### Run the project
Below are steps needed to run the project. Not covered in installing ansible or any other requirements.

#### Clone the repo
```bash
git clone https://github.com/redjordan2539/homelab_ansible.git
cd homelab_ansible
```

#### Create vault_pass file
```bash
nano ~/.vault_pass
```
Make sure to put just your vault password in this file and nothing else

#### Run the playbook
```bash
# This command will target all hosts listed in inventory.yaml
ansible-playbook -i inventory.yaml ./site.yaml
# if you prefer to target just one host use the limit flag
ansible-playbook -i inventory.yaml --limit <device hostname> 
```
