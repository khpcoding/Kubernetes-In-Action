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

### Helm Installation (Recommended)
```sh
helm repo add kwatch https://kwatch.dev/helm-charts
helm install kwatch kwatch/kwatch -n monitoring --create-namespace
