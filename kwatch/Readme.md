# 🔥 kwatch - Real-Time Kubernetes Watchdog

**kwatch** is your Kubernetes watchdog that instantly alerts you when pods crash or enter unhealthy states. Never miss a critical failure again!

## 🌟 Features

- 🚨 **Real-time alerts** for crashes (`OOMKilled`, `CrashLoopBackOff`, etc.)
- 📢 **Multi-channel notifications**: Slack, Discord, Teams, Email, Webhook , Telegram
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

Now Deploy Kwatch : 

```sh 
kubectl apply -f https://raw.githubusercontent.com/abahmed/kwatch/v0.10.1/deploy/deploy.yaml
```
