
# Helm Overview

Helm is a package manager for Kubernetes, similar to `apt`, `yum`, or `brew` for traditional operating systems. It is designed to simplify the deployment and management of applications on Kubernetes clusters. Helm packages, known as charts, provide a collection of files that describe related Kubernetes resources. These charts are easy to create, version, share, and publish.

## Installing Helm

Helm can be installed on a variety of operating systems. Below are the installation instructions for some of the most popular ones.

### Linux
1. **From Script (Easiest method):**
   ```bash
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```
2. **From package manager:**
   - For Debian/Ubuntu:
     ```bash
     sudo apt-get install helm
     ```
   - For Fedora/Red Hat:
     ```bash
     sudo yum install helm
     ```

### macOS
1. **Using Homebrew:**
   ```bash
   brew install helm
   ```
2. **From Script:**
   ```bash
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```

### Windows
1. **Using Chocolatey:**
   ```bash
   choco install kubernetes-helm
   ```
2. **From Script:**
   ```powershell
   $script = (New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3')
   Invoke-Expression $script
   ```

## Helm Cheat Sheet

A quick reference to commonly used Helm commands:

- **Create a new chart:**
  ```bash
  helm create my-chart
  ```
- **Install a chart:**
  ```bash
  helm install my-release my-chart/
  ```
- **List releases:**
  ```bash
  helm list
  ```
- **Upgrade a release:**
  ```bash
  helm upgrade my-release my-chart/
  ```
- **Uninstall a release:**
  ```bash
  helm uninstall my-release
  ```
- **Rollback a release:**
  ```bash
  helm rollback my-release [REVISION]
  ```
- **Search for charts:**
  ```bash
  helm search repo [KEYWORD]
  ```
- **Update the chart repository:**
  ```bash
  helm repo update
  ```
- **View chart information:**
  ```bash
  helm show chart my-repo/my-chart
  ```
- **Debugging an install:**
  ```bash
  helm install --debug --dry-run my-release my-chart/
  ```
