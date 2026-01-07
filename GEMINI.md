# Gemini Context: Lazy Update Ansible Project

This document provides context for the `lazy_update` project, a collection of Ansible playbooks designed for automated system maintenance, Docker container management, and specific service updates (Pi-hole, Nvidia).

## Project Overview

*   **Type:** Infrastructure as Code (Ansible)
*   **Purpose:** Automate routine maintenance tasks across a fleet of servers. It appears to be designed for integration with **Semaphore UI**, as indicated by comments in the playbooks (e.g., `hosts: "{{ target_hosts }}"`).
*   **Key Features:**
    *   System-wide APT updates and reboots.
    *   Docker installation (via official script) and container lifecycle management (pull/up).
    *   NVIDIA driver updates.
    *   Pi-hole synchronization via Gravity Sync.
    *   SSH key deployment.

## Key Files & Playbooks

### Core Playbooks

*   **`site.yml`**: The main system maintenance playbook.
    *   **Actions:** Updates apt cache, performs `dist-upgrade`, checks for `/var/run/reboot-required`, and reboots if necessary (skipping `brainiac-local`).
    *   **Variables:** `target_hosts`

*   **`docker.yml`**: Manages existing Docker Compose stacks.
    *   **Actions:** Checks for Docker installation, pulls latest images for specified directories, and runs `docker compose up -d`.
    *   **Variables:** `target_hosts`, `compose_dirs` (list of paths to docker-compose directories).

*   **`docker-install.yml`**: Installs Docker from scratch.
    *   **Actions:** Uses the official `get.docker.com` script to install Docker and adds the user to the `docker` group.
    *   **Variables:** `target_hosts`

*   **`nvidia.yml`**: manages Nvidia drivers.
    *   **Actions:** Checks installed version vs recommended, updates drivers, and handles reboots.
    *   **Variables:** `target_hosts`

*   **`pihole.yml`**: Pi-hole maintenance.
    *   **Actions:** Runs `pihole -up` and `gravity-sync`.
    *   **Variables:** `target_hosts`

*   **`key-deploy.yml`**: SSH Key Management.
    *   **Actions:** Adds a specific public SSH key to the `authorized_keys` of the target user.
    *   **Variables:** `target_hosts`, `ssh_key`

### Configuration

*   **`collections/requirements.yml`**: Defines Ansible collection dependencies.
    *   **Dependency:** `community.docker`

## Building and Running

### Prerequisites

1.  **Ansible:** Must be installed.
2.  **Dependencies:** Install required collections:
    ```bash
    ansible-galaxy collection install -r collections/requirements.yml
    ```

### Execution

These playbooks are written to be flexible with the inventory source, relying on the `target_hosts` variable.

**Manual CLI Run:**
To run manually, you must provide the inventory and the `target_hosts` variable (and others where applicable).

```bash
# Example: Run system updates
ansible-playbook -i inventory.ini site.yml -e "target_hosts=all"

# Example: Update Docker stacks
ansible-playbook -i inventory.ini docker.yml -e "target_hosts=docker_servers" -e '{"compose_dirs": ["/opt/stack1", "/opt/stack2"]}'
```

**Semaphore UI:**
When running within Semaphore, ensure the variables `target_hosts`, `compose_dirs`, and `ssh_key` are mapped correctly in the task environment or inventory variables.

## Development Conventions

*   **Variables:** Heavy reliance on extra vars (`-e`) for dynamic targeting (`target_hosts`).
*   **Privilege Escalation:** Most playbooks use `become: true` for root privileges.
*   **Error Handling:** Some tasks utilize `ignore_errors: yes` (e.g., Docker installation checks) to proceed gracefully if a state is already satisfied or verified via a different method.
