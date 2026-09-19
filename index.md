# Automated Website Deployment with Ansible

## Project Overview

This beginner-friendly lab demonstrates how Ansible can automatically install and configure an Apache web server on multiple Linux machines.

Instead of configuring every server manually, an administrator writes one set of instructions called a **playbook**. Ansible then applies those instructions to every selected server.

---

## What Is Ansible?

Ansible is an automation tool that helps administrators manage multiple computers from one central control machine.

Think of Ansible like a teacher giving the same assignment to an entire class. The teacher writes the instructions once, and every student receives the same instructions.

In this lab:

- The Ansible control machine sends the instructions.
- The managed servers receive the instructions.
- The YAML playbook describes the work that must be completed.

---

## Lab Environment

| System | Purpose | Documentation Address |
|---|---|---|
| `ansible-control` | Runs Ansible and stores the project files | `192.0.2.20` |
| `rhel-web1` | Managed Apache web server | `192.0.2.21` |
| `rhel-file1` | Second managed Apache web server | `192.0.2.22` |

> These addresses are examples reserved for documentation. Replace them with the addresses assigned to your own lab machines.

---

## How the Lab Works

1. The administrator writes the automation instructions in a YAML playbook.
2. The inventory file identifies the managed servers.
3. The Ansible control machine connects to the servers through SSH.
4. Ansible installs Apache and firewalld.
5. Ansible creates a customized homepage on each server.
6. Ansible starts and enables the required services.
7. Ansible permits HTTP traffic through the firewall.
8. The administrator opens both websites in a browser to verify the deployment.

---

## Project Files

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

### What Each File Does

| File | Purpose |
|---|---|
| `README.md` | Provides a summary on the main GitHub repository page |
| `index.md` | Becomes the main GitHub Pages tutorial |
| `inventory.ini.example` | Shows how to define managed servers safely |
| `playbooks/install_web.yml` | Contains the web-server automation instructions |
| `images/deployment-proof.png` | Provides visual proof that the lab worked |

---

## Example Inventory

The inventory tells Ansible which machines it should manage.

```ini
[rhel_servers]
rhel-web1 ansible_host=192.0.2.21
rhel-file1 ansible_host=192.0.2.22

[rhel_servers:vars]
ansible_user=administrator
```

Users who download this project should copy `inventory.ini.example` to `inventory.ini` and replace the example values with their own server addresses and SSH username.

---

## What the Playbook Does

The `install_web.yml` playbook performs the following tasks:

1. Connects to every server in the `rhel_servers` inventory group.
2. Uses administrator privileges when required.
3. Collects information about each server.
4. Installs Apache and firewalld.
5. Creates a customized homepage.
6. Starts the firewall.
7. Configures the firewall to start automatically.
8. permits HTTP web traffic.
9. Starts Apache.
10. Configures Apache to start automatically after a reboot.

---

## Important Playbook Variables

The customized webpage uses information collected from each server:

```yaml
{{ inventory_hostname }}
{{ ansible_hostname }}
{{ ansible_distribution }}
{{ ansible_distribution_version }}
```

These variables allow Ansible to display the correct server name and operating-system version on each webpage.

---

## Test Ansible Connectivity

Before running the playbook, test whether Ansible can communicate with the managed servers:

```bash
ansible rhel_servers -i inventory.ini -m ansible.builtin.ping
```

A successful result contains:

```text
SUCCESS
"ping": "pong"
```

Ansible's ping module tests the Ansible and SSH connection. It is different from the regular network `ping` command.

---

## Check the Playbook Syntax

Check the YAML file for formatting errors before running it:

```bash
ansible-playbook -i inventory.ini playbooks/install_web.yml --syntax-check
```

A successful check should display the playbook name without reporting a syntax error.

---

## Run the Playbook

Run the automation from the Ansible control machine:

```bash
ansible-playbook -i inventory.ini playbooks/install_web.yml --ask-become-pass
```

The option `--ask-become-pass` asks for the remote user's `sudo` password. The password is used during execution and is not stored in the playbook.

---

## Understand the Results

At the end of the run, Ansible displays a **PLAY RECAP**.

Example:

```text
PLAY RECAP
rhel-web1  : ok=6  changed=4  unreachable=0  failed=0
rhel-file1 : ok=6  changed=4  unreachable=0  failed=0
```

The most important results are:

| Result | Meaning |
|---|---|
| `ok` | A task completed successfully |
| `changed` | Ansible changed something on the server |
| `unreachable` | Ansible could not connect to the server |
| `failed` | A task encountered an error |

A successful deployment should show:

```text
unreachable=0
failed=0
```

---

## Proof of Success

The following screenshot shows the completed website deployment:

![Ansible deployment proof](images/deployment-proof.png)

Each managed server displays its own hostname and operating-system information.

The websites can be tested by opening the managed server addresses in a browser:

```text
http://192.0.2.21
http://192.0.2.22
```

Replace these example addresses with the addresses used in your own lab.

---

## What This Project Accomplishes

This project uses one central control machine to configure multiple Linux servers automatically.

The completed automation:

- Installs the Apache web-server software
- Installs and starts the firewall service
- Allows web traffic through the firewall
- Creates a webpage
- Starts the Apache service
- Enables services to start automatically
- Applies a consistent configuration to multiple servers
- Provides a repeatable deployment process

---

## Why Ansible Is Valuable

Without Ansible, an administrator must log in to every server and repeat the same commands manually.

This becomes difficult when a company manages tens, hundreds or thousands of servers.

Ansible allows the administrator to:

- Write the instructions once
- Apply them to many servers
- Reduce repetitive work
- Reduce configuration mistakes
- Keep systems consistent
- Repeat the deployment when new servers are added
- Document exactly how systems were configured

---

## Corporate Example

Imagine that a company needs to deploy the same internal webpage to 100 Linux servers.

Without automation, administrators may need to configure all 100 servers individually. Different administrators may also configure the servers differently.

With Ansible, the company can:

1. Add the servers to an inventory.
2. Write one approved playbook.
3. Test the playbook in a lab.
4. Run it against the production servers.
5. Review the results.
6. Run the same playbook again when new servers are added.

This saves time and creates a consistent configuration across the company.

---

## Skills Demonstrated

This project demonstrates:

- Linux server administration
- Ansible inventory management
- YAML playbook development
- SSH remote administration
- Apache web-server deployment
- Linux firewall configuration
- Service management
- Automation testing
- Technical documentation
- GitHub and GitHub Pages

---

## Security Notice

This repository contains demonstration information only.

Never upload:

- Passwords
- Private SSH keys
- API keys
- Access tokens
- Ansible Vault password files
- Confidential hostnames
- Customer information
- Real company network information

Use example addresses and sanitized screenshots in public documentation.

---

## Download the Complete Guide

hoose the format that works best for you:

- [View or download the PDF guide](documents/Ansible_Beginner_Website_Deployment_Guide.pdf)
- [Download the Microsoft Word guide](documents/Ansible_Beginner_Website_Deployment_Guide.docx)


## Final Result

This project proves that one administrator can use Ansible to deploy and configure multiple Linux web servers from one control machine.

**Write the instructions once. Apply them consistently to many servers.**