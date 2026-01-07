# Lazy Update Ansible Playbooks

This repository hosts a collection of Ansible playbooks for automating server maintenance, Docker container management, and specific service updates. These playbooks are designed to be flexible and are optimized for use with **Semaphore UI** on Debian/Ubuntu systems.

## 📦 Docker Management

### **Deploy & Update Containers** (`docker.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/docker.yml)
*   **Purpose:** Updates running Docker Compose stacks.
*   **Actions:** Installs the Python Docker SDK, pulls the latest images, and restarts containers only if changes are detected (idempotent).
*   **Variables:** `target_hosts`, `compose_dirs` (list of directories containing `docker-compose.yml`).

### **Install Docker** (`docker-install.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/docker-install.yml)
*   **Purpose:** Fresh installation of Docker.
*   **Actions:** Uses the official `get.docker.com` script to install Docker and the Compose plugin.

## 🛠 System Maintenance

### **System Updates** (`site.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/site.yml)
*   **Purpose:** General server maintenance.
*   **Actions:** Updates APT cache, performs `dist-upgrade`, cleans up unused packages, and notifies if a reboot is required.

### **SSH Key Deployment** (`key-deploy.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/key-deploy.yml)
*   **Purpose:** User access management.
*   **Actions:** Adds a specific public SSH key to the target user's `authorized_keys`.

## ⚡ Service Specific

### **NVIDIA Drivers** (`nvidia.yml`)
[View Raw](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/nvidia.yml)
*   **Purpose:** GPU Driver management.
*   **Actions:** Checks against recommended versions, updates drivers, and reboots if necessary.

---
*Note: These playbooks rely on dynamic variables (like `target_hosts`) passed via the inventory or CI/CD environment (Semaphore).*