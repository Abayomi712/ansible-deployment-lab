# Ansible Web Deployment Lab

A beginner-friendly automation project that uses Ansible to install and configure Apache web servers on multiple Linux machines.

## Project Overview

In this lab, one Ansible control machine automatically configures two managed Linux servers.

Instead of signing in to each server and repeating the same commands, the administrator creates one YAML playbook. Ansible then applies those instructions to every server in the selected inventory group.

## What I Accomplished

From one Ansible control machine, I automated the following tasks on two managed Linux servers:

- Installed the Apache web server
- Installed the firewalld service
- Created a customized homepage
- Started the Apache service
- Enabled Apache to start automatically
- Started and enabled the firewall
- Allowed HTTP traffic through the firewall
- Displayed each server's information on its webpage
- Verified both websites in a web browser

## Lab Design

| Machine | Purpose | Documentation Address |
|---|---|---|
| `ansible-control` | Runs Ansible and stores the project files | `192.0.2.20` |
| `rhel-web1` | First managed Apache web server | `192.0.2.21` |
| `rhel-file1` | Second managed Apache web server | `192.0.2.22` |

> The addresses above are documentation examples. Anyone reproducing this lab should replace them with the addresses assigned to their own machines.

## How the Lab Works

1. The inventory file identifies the managed servers.
2. The YAML playbook defines the required configuration.
3. The Ansible control machine connects to both servers through SSH.
4. Ansible installs Apache and firewalld.
5. Ansible creates a customized webpage on each server.
6. Ansible starts and enables the required services.
7. Ansible permits HTTP traffic through the firewall.
8. The administrator opens both websites to verify the deployment.

## Project Structure

```text
ansible-web-deployment-lab/
├── README.md
├── index.md
├── inventory.ini.example
├── playbooks/
│   └── install_web.yml
└── images/
    └── deployment-proof.png
```

## File Descriptions

| File | Purpose |
|---|---|
| `README.md` | Introduces the project on the main GitHub repository page |
| `index.md` | Provides the complete GitHub Pages tutorial |
| `inventory.ini.example` | Provides a safe example Ansible inventory |
| `playbooks/install_web.yml` | Contains the website deployment instructions |
| `images/deployment-proof.png` | Shows proof that the deployment succeeded |

## Requirements

To reproduce this lab, you need:

- One Linux machine to act as the Ansible control node
- Two Linux machines to act as managed servers
- Ansible installed on the control machine
- Python installed on the managed machines
- SSH connectivity between the control machine and managed machines
- A user account with `sudo` access on the managed machines
- The `ansible.posix` collection for firewall management

Install the required collection with:

```bash
ansible-galaxy collection install ansible.posix
```

## Example Inventory

The repository includes `inventory.ini.example`:

```ini
[rhel_servers]
rhel-web1 ansible_host=192.0.2.21
rhel-file1 ansible_host=192.0.2.22

[rhel_servers:vars]
ansible_user=administrator
```

Copy the example file to create your working inventory:

```bash
cp inventory.ini.example inventory.ini
```

Next, replace the example IP addresses and username with the information from your own lab.

Do not upload the working `inventory.ini` file if it contains information you do not want to publish.

## Test Ansible Connectivity

Before running the playbook, test the connection to the managed servers:

```bash
ansible rhel_servers -i inventory.ini -m ansible.builtin.ping
```

Successful systems should return:

```text
SUCCESS
"ping": "pong"
```

Ansible's ping module verifies that SSH, Python and Ansible communication are working. It is not the same as the standard network `ping` command.

## Check the Playbook Syntax

Check the playbook before running it:

```bash
ansible-playbook -i inventory.ini playbooks/install_web.yml --syntax-check
```

This detects many YAML indentation and formatting mistakes before Ansible changes the servers.

## Run the Playbook

Run the deployment from the Ansible control machine:

```bash
ansible-playbook -i inventory.ini playbooks/install_web.yml --ask-become-pass
```

The `--ask-become-pass` option asks for the remote user's `sudo` password. The password is used during execution and is not stored in the repository.

## What the Playbook Does

The playbook:

1. Connects to the servers in the `rhel_servers` inventory group.
2. Collects information about each managed server.
3. Installs Apache and firewalld.
4. Creates a customized homepage.
5. Starts and enables firewalld.
6. Opens the HTTP service in the firewall.
7. Starts and enables Apache.

Because the playbook uses Ansible facts, each webpage can display the correct hostname, Linux distribution and operating-system version.

## Expected Result

A successful Ansible play recap should resemble:

```text
PLAY RECAP
rhel-web1  : ok=6  changed=4  unreachable=0  failed=0
rhel-file1 : ok=6  changed=4  unreachable=0  failed=0
```

The most important values are:

```text
unreachable=0
failed=0
```

These values mean Ansible connected to the servers and completed the tasks without failures.

## Verify the Websites

Open both managed-server addresses in a browser:

```text
http://192.0.2.21
http://192.0.2.22
```

Replace the example addresses with your actual lab addresses.

You can also test from a terminal:

```bash
curl http://192.0.2.21
curl http://192.0.2.22
```

## Proof of Success

![Ansible deployment proof](images/deployment-proof.png)

The screenshot demonstrates that the website was deployed and made accessible through the network.

## Why Ansible Is Valuable

Without automation, an administrator must manually configure every server. That approach takes more time and increases the possibility of mistakes.

Ansible allows administrators to:

- Write instructions once
- Apply the instructions to multiple servers
- Create consistent configurations
- Reduce repetitive work
- Reduce human error
- Add new servers more easily
- Reproduce a deployment
- Document infrastructure changes

## Corporate Use Example

A company might need to configure 100 web servers with the same packages, services, firewall rules and website files.

Instead of configuring each server individually, the company can test and approve one Ansible playbook. The playbook can then apply the same configuration to every selected server.

The company can also rerun the playbook later. Ansible checks the existing configuration and changes only what is necessary.

## Skills Demonstrated

This project demonstrates experience with:

- Linux server administration
- Ansible automation
- YAML playbook development
- Inventory management
- SSH connectivity
- Apache deployment
- Linux firewall configuration
- Service management
- Configuration verification
- GitHub documentation
- GitHub Pages

## Security Notice

This repository is intended for demonstration and education.

Never upload:

- Passwords
- SSH private keys
- GitHub personal access tokens
- API keys
- Ansible Vault passwords
- Private certificate keys
- Customer information
- Confidential server names
- Real company network details

Use example addresses and sanitized screenshots when publishing the project.

## Full Tutorial

The complete beginner tutorial is available through GitHub Pages:

```text
https://YOUR-USERNAME.github.io/ansible-web-deployment-lab/
```

Replace `YOUR-USERNAME` with your GitHub username after enabling GitHub Pages.

## Project Outcome

This lab demonstrates how one administrator can use a single Ansible playbook to deploy and configure websites across multiple Linux servers.

**One control machine. One set of instructions. Multiple consistently configured servers.**