# Velero Backup for Kubernetes Cluster

## Overview
Velero is a tool for backing up and restoring Kubernetes clusters. This guide provides instructions for installing, configuring, and using Velero to perform backups and restores.

## Prerequisites
- A running Kubernetes cluster (v1.20+ recommended)
- Access to a storage provider (AWS S3, GCP, Azure, MinIO, etc.)
- Kubernetes CLI (`kubectl`) installed
- Velero CLI installed

## Installation
### 1. Install Velero CLI
Download and install the Velero CLI from the [official documentation](https://velero.io/docs/).

```sh
# Linux/macOS
curl -fsSL https://github.com/vmware-tanzu/velero/releases/latest/download/velero-linux-amd64 -o velero
chmod +x velero
sudo mv velero /usr/local/bin/
```

### 2. Install Velero in the Kubernetes Cluster
Create a storage bucket for backups and configure credentials for your provider.

#### Example: AWS S3 Setup
```sh
export BUCKET=<your-bucket-name>
export REGION=<your-region>
export AWS_ACCESS_KEY_ID=<your-access-key>
export AWS_SECRET_ACCESS_KEY=<your-secret-key>

velero install \
    --provider aws \
    --plugins velero/velero-plugin-for-aws:v1.6.0 \
    --bucket $BUCKET \
    --backup-location-config region=$REGION \
    --secret-file ./credentials-velero \
    --use-volume-snapshots=false \
    --wait
```

## Backup Operations
### 1. Create a Backup
```sh
velero backup create my-cluster-backup --include-namespaces my-namespace
```

### 2. Check Backup Status
```sh
velero backup get
```

### 3. List Backup Details
```sh
velero backup describe my-cluster-backup --details
```

## Restore Operations
### 1. Restore from Backup
```sh
velero restore create --from-backup my-cluster-backup
```

### 2. Check Restore Status
```sh
velero restore get
```

## Scheduled Backups
To schedule backups every 6 hours:
```sh
velero schedule create my-scheduled-backup --schedule "@every 6h"
```

## Troubleshooting
### Check Logs
```sh
kubectl logs deployment/velero -n velero
```

### Check Velero Events
```sh
velero backup logs my-cluster-backup
```

### Delete Old Backups
```sh
velero backup delete my-cluster-backup --confirm
```

## Conclusion
Velero is a powerful tool for Kubernetes backup and disaster recovery. Ensure backups are tested periodically to verify restoration works as expected.

## References
- [Velero Official Documentation](https://velero.io/docs/)


