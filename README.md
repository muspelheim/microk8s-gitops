<img src="https://camo.githubusercontent.com/bd0df216af51c1525f14e62155608e448562cb4033554e001a0ac2009e545aec/68747470733a2f2f726173706265726e657465732e6769746875622e696f2f696d672f6c6f676f2e737667" align="left" width="144px" height="144px"/>

## microk8s-gitops - Home Cloud via Flux v2 | GitOps Toolkit
> GitOps state for my home lab cluster using [flux2](https://github.com/fluxcd/flux2)

[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white&style=flat-square)](https://github.com/pre-commit/pre-commit)
[![renovate](https://github.com/angelnu/k8s-gitops/workflows/renovate/badge.svg)](https://github.com/angelnu/k8s-gitop/workflows/renovate-annotations-schedule/actions)
[![update-flux](https://github.com/angelnu/k8s-gitops/workflows/update-flux/badge.svg)](https://github.com/angelnu/k8s-gitop/workflows/flux-update-schedule/actions)
<br />
Leverage Flux2 to automate cluster state using code residing in this repo.

---

## :open_file_folder:&nbsp; Repository structure

The Git repository contains the following directories under `cluster` and are ordered below by how Flux will apply them.

- **base** directory is the entrypoint to Flux
- **crds** directory contains custom resource definitions (CRDs) that need to exist globally in your cluster before anything else exists
- **core** directory (depends on **crds**) are important infrastructure applications (grouped by namespace) that should never be pruned by Flux
- **apps** directory (depends on **core**) is where your common applications (grouped by namespace) could be placed, Flux will prune resources here if they are not tracked by Git anymore

```
.
├── cluster
│   ├── apps
│   │   ├── default
│   │   │   └── hajimari
│   │   ├── home
│   │   │   ├── adguard-home
│   │   │   ├── esphome
│   │   │   ├── focalboard
│   │   │   ├── home-assistant
│   │   │   ├── mosquitto
│   │   │   └── zigbee2mqtt
│   │   └── networking
│   │       ├── authelia
│   │       └── linkerd
│   ├── base
│   │   └── flux-system
│   ├── core
│   │   ├── cert-manager
│   │   ├── kubed
│   │   ├── metallb-system
│   │   └── namespaces
│   └── crds
│       └── cert-manager
│       └── metallb
└── standalone
    └── adguard
```

---

## :gear:&nbsp; Hardware

Dell Optiplex 7090 Micro as a core server with the following specs:
* Intel Core i5-10500T
* 32GB RAM
* 512 NVMe SSD
* 1TB SSD
* Zigbee USB stick (CC2538)

x3 Raspberry Pi 3B+ as edge devices with the following specs:
* 1GB RAM
* 32GB SD Card

Bunch of Zigbee and BLE devices.

---

## :wrench:&nbsp; Tools

Below are some of the tools I find useful

| Tool                                                   | Purpose                                               |
|--------------------------------------------------------|-------------------------------------------------------|
| [sops](https://github.com/mozilla/sops)                | Simple and flexible tool for managing secrets         |
| [pre-commit](https://github.com/pre-commit/pre-commit) | Ensure the YAML and shell script in my repo are consistent |
| [task](https://taskfile.dev/)                          | Task is a task runner / build tool|                  |

---

## :handshake:&nbsp; Thanks!

A lot of inspiration for my cluster came from the people that have shared their clusters over at [awesome-home-kubernetes](https://github.com/k8s-at-home/awesome-home-kubernetes)

---
