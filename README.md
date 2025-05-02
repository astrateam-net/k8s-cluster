<div align="center">

<img width="144px" height="144px" src="https://raw.githubusercontent.com/astrateam-net/home-cluster/main/docs/src/assets/logo.png"/>

## AstraTeam Kubernetes Cluster <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f680/512.gif" alt="🚀" width="32" height="32">

<div align="center">

[![Age-Days](https://img.shields.io/endpoint?url=https%3A%2F%2Fkromgo.astrateam.net%2Fcluster_age_days&style=flat-square&label=Age)](https://github.com/kashalls/kromgo)&nbsp;&nbsp;
[![Uptime-Days](https://img.shields.io/endpoint?url=https%3A%2F%2Fkromgo.astrateam.net%2Fcluster_uptime_days&style=flat-square&label=Uptime)](https://github.com/kashalls/kromgo)&nbsp;&nbsp;
[![Node-Count](https://img.shields.io/endpoint?url=https%3A%2F%2Fkromgo.astrateam.net%2Fcluster_node_count&style=flat-square&label=Nodes)](https://github.com/kashalls/kromgo)&nbsp;&nbsp;
[![Pod-Count](https://img.shields.io/endpoint?url=https%3A%2F%2Fkromgo.astrateam.net%2Fcluster_pod_count&style=flat-square&label=Pods)](https://github.com/kashalls/kromgo)&nbsp;&nbsp;
[![CPU-Usage](https://img.shields.io/endpoint?url=https%3A%2F%2Fkromgo.astrateam.net%2Fcluster_cpu_usage&style=flat-square&label=CPU)](https://github.com/kashalls/kromgo)&nbsp;&nbsp;
[![Memory-Usage](https://img.shields.io/endpoint?url=https%3A%2F%2Fkromgo.astrateam.net%2Fcluster_memory_usage&style=flat-square&label=Memory)](https://github.com/kashalls/kromgo)&nbsp;&nbsp;
[![Power-Usage](https://img.shields.io/endpoint?url=https%3A%2F%2Fkromgo.astrateam.net%2Fcluster_power_usage&style=flat-square&label=Power)](https://github.com/kashalls/kromgo)&nbsp;&nbsp;
[![Alerts](https://img.shields.io/endpoint?url=https%3A%2F%2Fkromgo.astrateam.net%2Fcluster_alert_count&style=flat-square&label=Alerts)](https://github.com/kashalls/kromgo)

</div>

... managed with Flux and Renovate <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f916/512.gif" alt="🤖" width="16" height="16">

</div>




</div>

## Overview

This repository is AstraTeam Kubernetes cluster in a declarative state. [Flux](https://github.com/fluxcd/flux2) watches the [kubernetes](./kubernetes/) folder and will make the changes to the cluster based on the YAML manifests.

### <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f4a1/512.gif" alt="💡" width="16" height="16"> Core Components

Core components that form the foundation of the cluster:

- [backube/volsync](https://github.com/backube/volsync) and [backube/snapscheduler](https://github.com/backube/snapscheduler): Backup and recovery of persistent volume claims.
- [cilium/cilium](https://github.com/cilium/cilium): Kubernetes CNI.
- [envoyproxy/envoy](https://github.com/envoyproxy/gateway): Kubernetes-based application gateway using [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io/).
- [external-secrets/external-secrets](https://github.com/external-secrets/external-secrets): Managed Kubernetes secrets using [1Password Connect](https://github.com/1Password/connect).
- [jetstack/cert-manager](https://cert-manager.io/docs/): Creates SSL certificates for services in my Kubernetes cluster.
- [kubernetes-sigs/external-dns](https://github.com/kubernetes-sigs/external-dns): Automatically manages DNS records from my cluster in CloudFlare.
- [rancher/system-upgrade-controller](https://github.com/rancher/system-upgrade-controller): Handles Kubernetes and Talos upgrades automatically.
- [rook/rook](https://github.com/rook/rook): Distributed block storage for peristent storage.
- [siderolabs/talos](https://www.talos.dev/): The Kubernetes Operating System.

### <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f6a8/512.gif" alt="🚨" width="16" height="16"> Observability

For observability and monitoring of the cluster the following software is used:

- [fluent/fluent-bit](https://github.com/fluent/fluent-bit): Log processor.
- [grafana/grafana](https://github.com/grafana/grafana): Data visualization platform.
- [prometheus/alertmanager](https://github.com/prometheus/alertmanager): Handles processing and sending alerts.
- [pushover](https://pushover.net): Handles receiving alerts on my devices.
- [TwiN/gatus](https://github.com/TwiN/gatus): High level status dashboard.
- [VictoriaMetrics/VictoriaLogs](https://docs.victoriametrics.com/victorialogs/): Database for logs.
- [VictoriaMetrics/VictoriaMetrics](https://github.com/VictoriaMetrics/VictoriaMetrics): Time series database, drop-in replacement for Prometheus.

### <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f916/512.gif" alt="🤖" width="16" height="16"> Automation

- [Github Actions](https://docs.github.com/en/actions) for checking code formatting and running periodic jobs
- [Renovate](https://github.com/renovatebot/renovate) keeps the application charts and container images up-to-date

### <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f32a_fe0f/512.gif" alt="🌪" width="16" height="16"> Cloud Dependencies

- [1Password](https://1password.com) for managing secrets via external-secrets.
- [Cloudflare](https://cloudflare.com) tunnels for exposing services & creating certificates & managing domains.
- [Pushover](https://pushover.net/) for sending alerts.


### <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f35d/512.gif" alt="🍝" width="16" height="16"> Directories

This Git repository contains the following directories.


```sh
📁 kubernetes
├── 📁 apps       # applications
├── 📁 components # re-useable kustomize components
└── 📁 flux       # flux system configuration
```


## <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/2699_fe0f/512.gif" alt="⚙" width="16" height="16"> Hardware

<details>
  <summary>Checkout my rack</summary>

  <img src="https://raw.githubusercontent.com/astrateam-net/k8s-cluster/main/" align="center" alt="rack" height="900px"/>
</details>


| Device                                                | Count | OS Disk Size  | Data Disk Size       | Ram     | Operating System | Purpose           |
|-------------------------------------------------------|-------|---------------|----------------------|---------|------------------|-------------------|
| CCR2004-1G-12S+2XS                                    | 1     | -             | -                    | -       | Mikrotik         | Router            |
| CRS504-4XQ-IN                                         | 1     | -             | -                    | -       | Mikrotik         | Switch            |
| CRS326-24S+2Q+RM                                      | 1     | -             | -                    | -       | Mikrotik         | Switch            |
| CRS310-8G+2S+IN                                       | 2     | -             | -                    | -       | Mikrotik         | Swtich            |
| CRS328-24P-4S+RM                                      | 2     | -             | -                    | -       | Mikrotik         | PoE Swtich        ||
| AM-KVM801 HDMI KVM Switch 8 Ports                     | 1     | -             | -                    | -       | -                | KVM Switch        |
| POWERCOM Macan MRT-3000SE                             | 1     | -             | -                    |         | -                | UPS               |
| Synology DS1821+                                      | 1     | -             | 4x16TB               | 20GB    | DSM              | NAS               |
| MS-01 i9-13900H                                       | 1     | 1TB           | 2TB                  | 96GB    | Talos            | Control Plane     |

---

## <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f64f/512.gif" alt="🙏" width="16" height="16"> Graditude and Thanks

Thanks to all the people who donate their time to the [Home Operations](https://discord.gg/home-operations) community.

This repository was built off the [onedr0p/cluster-template](https://github.com/onedr0p/cluster-template) repository.

## <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/2728/512.gif" alt="✨" width="16" height="16"> Star History

[![Star History Chart](https://api.star-history.com/svg?repos=astrateam-net/k8s-cluter&type=Date)](https://star-history.com/#astrateam-net/k8s-cluster&Date)

## <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/270f_fe0f/512.gif" alt="✏" width="16" height="16"> License

See [LICENSE](./LICENSE)
