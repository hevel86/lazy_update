# Lazy Update Ansible Playbooks

This repository hosts a collection of Ansible playbooks for automating server maintenance, Docker container management, and specific service updates. These playbooks are optimized for **Debian/Ubuntu** systems and designed for integration with **Semaphore UI**.

## 📦 Docker Management

### **Deploy & Update Containers** (`docker.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/docker.yml)
*   **Purpose:** Idempotent update of Docker Compose stacks.
*   **Actions:** Ensures the Python Docker SDK is installed, pulls the latest images, and restarts containers only if changes are detected using the `docker_compose_v2` module.
*   **Variables:** `target_hosts`, `compose_dirs` (list of directories containing `docker-compose.yml`).

### **Install Docker** (`docker-install.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/docker-install.yml)
*   **Purpose:** Robust Docker installation.
*   **Actions:** Checks for existing installation, uses the official Docker script if needed, and installs the `python3-docker` SDK required for other playbooks.

## 🛠 System Maintenance

### **System Updates** (`site.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/site.yml)
*   **Purpose:** General server maintenance.
*   **Actions:** Updates APT cache, performs `dist-upgrade`, cleans up unused packages (`autoremove`/`autoclean`), and notifies if a reboot is required.

### **SSH Key Deployment** (`key-deploy.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/key-deploy.yml)
*   **Purpose:** User access management.
*   **Actions:** Adds a specific public SSH key to the target user's `authorized_keys`.

## ⚡ Service Specific

### **NVIDIA Drivers** (`nvidia.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/nvidia.yml)
*   **Purpose:** Comprehensive GPU management.
*   **Actions:** Detects NVIDIA hardware, installs recommended drivers via PPA, configures **NVIDIA Container Toolkit** for Docker, and applies compute optimizations like Persistence Mode and NVENC profiling.
*   **Variables:** `target_hosts`, `var_environment` (test/production), `var_reboot` (true/false).

### **Pi-hole Update** (`pihole.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/pihole.yml)
*   **Purpose:** Pi-hole maintenance.
*   **Actions:** Runs `pihole -up` to update core installation (optimized for Pi-hole v6+).

## 📋 Requirements
Ensure the necessary collections are installed:
```bash
ansible-galaxy collection install -r collections/requirements.yml
```

---
*Note: These playbooks rely on dynamic variables (like `target_hosts`) passed via Semaphore UI or Extra Vars.*
