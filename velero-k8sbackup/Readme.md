# 🚀 Velero Backup for Kubernetes Cluster

## 📌 Overview 📝
Velero is a tool for backing up and restoring Kubernetes clusters. This guide provides instructions for installing, configuring, and using Velero to perform backups and restores.

## ✅ Prerequisites 🛠️
- 🏗️ A running Kubernetes cluster (v1.20+ recommended)
- ☁️ Access to a storage provider (AWS S3, GCP, Azure, MinIO, etc.)
- 🔧 Kubernetes CLI (`kubectl`) installed
- 📦 Velero CLI installed

## ⚙️ Installation 🚀
### 1️⃣ Install Velero 🏗️
```sh
wget https://github.com/vmware-tanzu/velero/releases/download/v1.15.2/velero-v1.15.2-linux-amd64.tar.gz && tar xvf  velero-v1.15.2-linux-amd64.tar.gz && cd velero-v1.15.2-linux-amd64 && cp velero  /usr/local/bin/
```

### 2️⃣ Install MinIO Object Storage on K8s Cluster ☁️
Here, we use MinIO for Kubernetes backup storage. You can easily deploy MinIO using the following manifest:

```sh
version: '3'
services:
  minio:
    image: quay.io/minio/minio
    container_name: minio
    command: server /data --console-address ":9001"
    environment:
      MINIO_ROOT_USER: admin
      MINIO_ROOT_PASSWORD: password
    ports:
      - "9000:9000"  # S3 API
      - "9001:9001"  # Web UI
    volumes:
      - minio-data:/data
volumes:
  minio-data:
```
🔹 Now, log in to the MinIO UI and create a bucket named, for example, `k8sbackup`.

### 3️⃣ Create Credentials 🔑
Create MinIO Bucket Credentials:
```sh
cat <<EOF > credentials-velero
[default]
aws_access_key_id=admin
aws_secret_access_key=password
EOF
```

### 4️⃣ Install Velero 🎯
It will start up the Velero pod:
```sh
velero install \
    --provider aws \
    --plugins velero/velero-plugin-for-aws:v1.8.1 \
    --bucket k8sbackup \
    --secret-file ./credentials-velero \
    --backup-location-config region=minio,s3Url=http://185.166.9.40:9000,s3ForcePathStyle="true" \
    --use-volume-snapshots=false
```

## 🔄 Backup Operations 📂
### 1️⃣ Create a Backup 🏷️
```sh
velero backup create <BACKUP_NAME> --include-namespaces my-namespace
```

### 2️⃣ Check Backup Status 📊
```sh
velero backup get
```
<p align="center">
  <img src="https://github.com/user-attachments/assets/14eb47f5-c122-44bb-b0a9-2906a1581b4d" width="600">
</p>

🔹 Now, in MinIO, the backup is also stored:

![image](https://github.com/user-attachments/assets/c6872ff5-295f-40f8-8ea8-628361981d36)

### 3️⃣ List Backup Details 📜
```sh
velero backup describe my-cluster-backup --details
```

## 🔧 Restore Operations 🔄
### 1️⃣ Restore from Backup ♻️
```sh
velero restore create --from-backup my-cluster-backup
```

### 2️⃣ Check Restore Status 🔍
```sh
velero restore get
```

## ⏳ Scheduled Backups 📆
To schedule backups every 6 hours:
```sh
velero schedule create my-scheduled-backup --schedule "@every 6h"
```

## 🛠️ Troubleshooting 🔍
### 📜 Check Logs 📄
```sh
kubectl logs deployment/velero -n velero
```

### 📢 Check Velero Events 📡
```sh
velero backup logs my-cluster-backup
```

### 🗑️ Delete Old Backups ❌
```sh
velero backup delete my-cluster-backup --confirm
```

## 🎯 Conclusion ✅
Velero is a powerful tool for Kubernetes backup and disaster recovery. Ensure backups are tested periodically to verify restoration works as expected.

