# Ansible Playbooks Overview

This repository contains several Ansible playbooks designed to automate system configuration and service deployments. Below is an overview of each playbook and its purpose, with links to the raw files.

## Docker Ansible Playbook

[View `docker.yml` (Raw)](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/docker.yml)

This playbook installs Docker and Docker Compose, pulls the latest Docker images, and starts the containers for each specified Docker Compose directory.

## NVIDIA Drivers Ansible Playbook

[View `nvidia.yml` (Raw)](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/nvidia.yml)

This playbook manages NVIDIA driver installation. It checks if the recommended version is installed and updates it if necessary, with an optional reboot.

## Pi-hole Ansible Playbook

[View `pihole.yml` (Raw)](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/pihole.yml)

This playbook updates Pi-hole and runs the Gravity Sync command to synchronize Pi-hole instances.

## Site Deployment Ansible Playbook

[View `site.yml` (Raw)](https://raw.githubusercontent.com/hevel86/lazy_update/refs/heads/master/site.yml)

This playbook performs system maintenance tasks, including updating the APT package cache, upgrading all packages, and cleaning up unnecessary ones.
