<div align="center">
  <img src="https://raw.githubusercontent.com/kubescape/kubescape/master/website/static/img/logo.png" alt="Kubescape Logo" width="300"/>
</div>
# Kubescape

Kubescape is an open-source Kubernetes security platform designed to help you secure your Kubernetes clusters throughout the development and deployment lifecycle. It provides comprehensive scanning, risk analysis, and compliance checks to ensure your cluster adheres to security best practices and frameworks like the NSA-CISA Kubernetes Hardening Guidelines, MITRE ATT&CK, and more.

## Why Use Kubescape?

Kubernetes clusters are complex, and misconfigurations or vulnerabilities can expose your infrastructure to significant risks. Kubescape helps by:

- **Scanning for vulnerabilities**: Identify security risks in workloads, images, and configurations.
- **Enforcing compliance**: Validate compliance with industry standards and organizational policies.
- **Risk analysis**: Prioritize risks based on severity and potential impact.
- **Shift-left security**: Integrate security early in the CI/CD pipeline to catch issues before deployment.
- **Continuous monitoring**: Run scans periodically or in-cluster to detect drift from secure baselines.

## Installation

### Prerequisites
- Kubernetes cluster (v1.18 or later)
- `kubectl` configured to access your cluster
- Helm (for in-cluster installation)

### Install the Kubescape CLI

**Quick install (Linux/macOS):**
```bash
curl -sSL https://raw.githubusercontent.com/kubescape/kubescape/master/install.sh | /bin/bash
```
### Install from source 
```bash
git clone https://github.com/kubescape/kubescape.git
cd kubescape
make install
```
### Install Kubescape in Your Cluster (Helm)
Deploy Kubescape for continuous in-cluster scanning and monitoring:
1. Add the Kubescape Helm repository:
```bash
helm repo add kubescape https://kubescape.github.io/helm-charts/
helm repo update
```
2.Install the Kubescape components:
```bash
helm install kubescape kubescape/kubescape-cloud-operator \
  --namespace kubescape \  # Namespace for Kubescape components
  --create-namespace \
  --set accountID=<YOUR_ACCOUNT_ID>  # Required for reporting (sign up at https://cloud.armosec.io)
```
### usage
Basic CLI Scanning
Scan your cluster for vulnerabilities and misconfigurations:
```bash
kubescape scan --submit  # Submit results to the Kubescape Cloud dashboard
```
Scan a specific Kubernetes manifest file:
```bash
kubescape scan "path/to/manifest.yaml"
```
Generate a JSON/PDF report:
```bash
kubescape scan --format json --output results.json
```
## Scan NSA framework
Scan a running Kubernetes cluster with the NSA framework:
```bash
kubescape scan framework nsa
```
output: 
![image](https://github.com/user-attachments/assets/e25af3a6-76a0-485b-ba0d-bdb4690e6282)

## Scan MITRE framework
Scan a running Kubernetes cluster with the MITRE ATT&CK® framework:
```bash
kubescape scan framework mitre
```
## Scan specific namespaces:
```bash
kubescape scan --include-namespaces development,staging,production
```
## Scan local YAML files
```bash
kubescape scan /path/to/directory-or-directory
```




