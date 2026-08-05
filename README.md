# Local DevOps Workstation Setup & Verification

## Overview
This document outlines the package management strategy, environment configuration steps, and troubleshooting resolutions applied while building a production-grade local DevOps workstation on Windows 10 utilizing WSL2.

---

## Package Managers Used
1. **Winget (Windows Package Manager):** Used for initial bulk installation of core desktop utilities and Windows binaries (`git`, `docker`, `vscode`, `nodejs`, `aws-cli`, `azure-cli`, `jq`, `terraform`, `minikube`).
2. **APT (Advanced Package Tool - Ubuntu/WSL2):** Used inside the WSL2 Linux environment to install native Linux packages including `ansible` and its Python dependencies.
3. **NPM (Node Package Manager):** Used for installing global CLI tools (`@google/gemini-cli`).

---

## Workstation Architecture & Configuration

1. **Host Environment & Virtualization:**
   - Enabled WSL2 on Windows 10 and configured Ubuntu 26.04 LTS as the default Linux distribution.
   - Configured Docker Desktop with WSL2 engine integration.

2. **Kubernetes Environment:**
   - Installed `minikube`, `kubectl`, and `helm`.
   - Provisioned a local Kubernetes cluster backed by the Docker driver (`minikube start --driver=docker`).

3. **Environment Persistence (`.bashrc`):**
   - Appended standard Windows binary directories (`nodejs`, `AWSCLIV2`, `Azure/CLI2`, and global `npm`) to the Git Bash `PATH` environment variable inside `~/.bashrc`.

---

## Troubleshooting & Engineering Resolutions

### Issue 1: `bash: command not found` for newly installed CLI binaries
* **Cause:** Winget installed packages to non-standard system directories (`/c/Program Files/...`) that were not present in the default Git Bash `PATH`.
* **Resolution:** Appended the target binary paths permanently to `~/.bashrc` and reloaded the shell (`source ~/.bashrc`).

### Issue 2: Ansible Execution on Windows Host
* **Cause:** Ansible lacks native execution support on Windows environments.
* **Resolution:** Installed Ansible within the WSL2 Ubuntu distribution (`sudo apt install ansible`) and invoked it from Git Bash via `wsl ansible`.

### Issue 3: Missing Global `npm` Binaries
* **Cause:** Global npm packages were saved in `%AppData%\npm`, which was not linked in Git Bash.
* **Resolution:** Registered `/c/Users/Hp/AppData/Roaming/npm` in `~/.bashrc`.## Verification Status: Complete
