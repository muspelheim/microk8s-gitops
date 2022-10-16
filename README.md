# GitOps for HomeAssistant

This is a GitOps repository for HomeAssistant. It uses [flux2](https://github.com/fluxcd/flux2)
to manage the kubernetes cluster ([MicroK8S](https://microk8s.io/)) for HomeAssistant and other services.

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
│   │       ├── linkerd
│   │       └── traefik
│   ├── base
│   │   └── flux-system
│   ├── core
│   │   ├── cert-manager
│   │   ├── kubed
│   │   ├── metallb-system
│   │   └── namespaces
│   └── crds
│       ├── cert-manager
│       └── traefik
└── standalone
    └── adguard
```

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
