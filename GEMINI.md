# Gemini Context: Lazy Update Ansible Project

This document provides context for the `lazy_update` project, a collection of optimized Ansible playbooks for automated system maintenance, Docker container management, and GPU/Service updates.

## Project Overview

*   **Type:** Infrastructure as Code (Ansible)
*   **Target OS:** Debian / Ubuntu / Raspberry Pi OS
*   **Environment:** Proxmox clusters (3 nodes each at Home and Work), Raspberry Pi hardware, and Debian-based VMs/CTs.
*   **Orchestration:** Optimized for **Semaphore UI** integration.
*   **Key Features:**
    *   System-wide APT maintenance with automatic cleanup and reboot notifications.
    *   Idempotent Docker container lifecycle management via `docker_compose_v2`.
    *   Automated NVIDIA GPU deployment including drivers, Container Toolkit, and compute optimizations.
    *   Streamlined Pi-hole v6 updates.
    *   Secure SSH key deployment.

## Key Files & Playbooks

### Core Playbooks

*   **`site.yml`**: General maintenance.
    *   **Actions:** APT update, `dist-upgrade`, `autoremove`, `autoclean`.
    *   **Note:** Notifies if reboot is required; does not reboot automatically.

*   **`docker.yml`**: Docker stack management.
    *   **Actions:** Ensures `python3-docker` SDK is present, pulls latest images, and manages container state.
    *   **Variables:** `target_hosts`, `compose_dirs`.

*   **`docker-install.yml`**: Host bootstrapping.
    *   **Actions:** Idempotent installation of Docker engine and necessary Python dependencies.

*   **`nvidia.yml`**: Advanced GPU management.
    *   **Actions:** Hardware detection, driver PPA management, `nvidia-container-toolkit` setup, and Persistence Mode activation.
    *   **Variables:** `target_hosts`, `var_environment` (production/test), `var_reboot`.

*   **`pihole.yml`**: Pi-hole v6 maintenance.
    *   **Actions:** Runs `pihole -up` with change detection logic.

*   **`key-deploy.yml`**: Access management.
    *   **Actions:** Authorized keys deployment for `{{ ansible_user }}`.

### Configuration

*   **`collections/requirements.yml`**:
    *   **Dependency:** `community.docker` (pinned to `>=3.4.0`).

## Building and Running

### Prerequisites
```bash
ansible-galaxy collection install -r collections/requirements.yml
```

### Execution
Designed to receive `target_hosts` and other variables via Semaphore UI. Manual execution requires passing these as extra vars:
```bash
ansible-playbook -i inventory.ini site.yml -e "target_hosts=all"
```

## Development Conventions
*   **Idempotency:** Playbooks use modules (e.g., `docker_compose_v2`) or `changed_when` logic to ensure accurate reporting.
*   **Safety:** Modifying tasks in critical playbooks (like NVIDIA) are often guarded by `var_environment == "production"`.
*   **Privilege:** All playbooks assume `become: true`.