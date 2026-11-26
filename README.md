# Phonebook Helm Chart Repository

This repository hosts the packaged Helm chart for the Kubernetes Microservice Phonebook application. The chart deploys the webserver, resultserver, and MySQL database.

## Requirements

- Kubernetes cluster
- Helm 3
- A working `kubectl` context for the target cluster

## Add the Helm repository

This chart repository is hosted in the `myHelmRepo` directory of the GitHub project:

```bash
helm repo add phonebook https://raw.githubusercontent.com/HalitCam/Kubernetes-Microservice-Phonebook-deployment/main/myHelmRepo
helm repo update
```

## Install

Install the chart using its default configuration:

```bash
helm install phonebook phonebook/phonebook
```

Check the deployment:

```bash
kubectl get pods
kubectl get services
kubectl get pvc
```

The webserver and resultserver are exposed as `NodePort` services:

- Webserver: `http://<NODE-IP>:30001`
- Resultserver: `http://<NODE-IP>:30002`

Replace `<NODE-IP>` with an address that is reachable from your machine. Your cluster's firewall or security rules must allow traffic to these ports.

## Configuration

The packaged chart is `phonebook-0.1.0.tgz`; its chart version is `0.1.0` and its application version is `1.0.0`.

The chart's `values.yaml` defines the MySQL credentials and database name, application database connection settings, and replica counts (`webserver_count` and `resultserver_count`). To customize values, download the chart, edit its `values.yaml`, and install the local package:

```bash
helm pull phonebook/phonebook --untar
# Edit phonebook/values.yaml
helm install phonebook ./phonebook
```

## Upgrade and uninstall

```bash
helm upgrade phonebook phonebook/phonebook
helm uninstall phonebook
```

## Storage and security notes

- MySQL uses a `20Gi` PersistentVolumeClaim and a `hostPath` PersistentVolume at `/mnt/data`. This storage setup is intended for demonstration and may not be suitable for multi-node or production clusters. Configure storage appropriate to your Kubernetes environment before deployment.
- The chart's default password values are Base64-encoded because they are used in a Kubernetes Secret. Base64 is not encryption. Change the defaults and do not commit real credentials to a public repository. Use a suitable secret-management solution for production.

## Repository contents

- `index.yaml` — Helm repository index
- `phonebook-0.1.0.tgz` — packaged Phonebook chart
