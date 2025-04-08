# 🔥 kwatch - Real-Time Kubernetes Watchdog

[![GitHub release](https://img.shields.io/github/v/release/kwatch-dev/kwatch)](https://github.com/kwatch-dev/kwatch/releases)
[![Helm Chart](https://img.shields.io/badge/Helm-ArtifactHub-informational)](https://artifacthub.io/packages/helm/kwatch/kwatch)
[![Go Report Card](https://goreportcard.com/badge/github.com/kwatch-dev/kwatch)](https://goreportcard.com/report/github.com/kwatch-dev/kwatch)

**kwatch** is your Kubernetes watchdog that instantly alerts you when pods crash or enter unhealthy states. Never miss a critical failure again!

## 🌟 Features

- 🚨 **Real-time alerts** for crashes (`OOMKilled`, `CrashLoopBackOff`, etc.)
- 📢 **Multi-channel notifications**: Slack, Discord, Teams, Email, Webhook
- 📝 **Crash context** with pod logs and events
- 🕵️ **Smart filtering** to reduce alert fatigue
- ⚡ **Lightweight** (~10MB RAM per node)
- 🔄 **Supports all workloads**: Deployments, StatefulSets, DaemonSets, CronJobs

## 🚀 Quick Start

### Installation

here with this config map you can install kwatch : 

```sh
#config.yml
apiVersion: v1
kind: Namespace
metadata:
  name: kwatch
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: kwatch
  namespace: kwatch
data:
  config.yaml: |
    alert:
      telegram:
        token: TOKEN #====> PUT IT IN DUBLE QOUTE
        chatId: CHAT_ID  #====> PUT IT IN DUBLE QOUTE
```

After set your Telegram Token and chatID you need to run this command to deploy it on k8s cluster :

```sh
kubectl apply -f config.yml
```
