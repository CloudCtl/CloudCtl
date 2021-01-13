![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/cloudctl/koffer/koffer/main?style=plastic) ![Docker Image Size (latest by date)](https://img.shields.io/docker/image-size/cloudctl/koffer?style=plastic) ![Docker Pulls](https://img.shields.io/docker/pulls/cloudctl/koffer?style=plastic)
    
# Koffer | Artifact Rake & Bundle Appliance
## About
Koffer is an automation runtime for raking in various artifacts required to
deploy Kubernetes Infrastructure, Pipelines, and applications into airgaped 
environments. Koffer is strictly an ansible consumer and requires run against
external repo volume cloned or mounted inside the container at `/root/koffer`.

## Creates
Koffer produces a standardized tarball of all artifacts ready for transport

## Supports
Compatibile Artifact Types:
  - git repos
  - terraform codebases 
    - performs terraform init at time of capture
  - docker images
    - Pulls & hydrates built in docker registry service to persistent local path
    - high side is served with generic docker registry:2 container
  - capability to add more artifact types with custom collector plugins

## How To
### 1. Create Koffer Bundle Directory
  - Example: [collector-ocp](https://github.com/CodeSparta/collector-ocp) plugin
```
mkdir -p ~/bundle
```
### 2.a Run Koffer
```
sudo podman run -it --rm --pull always \
    --volume $${HOME}/bundle:/root/bundle:z \
  docker.io/cloudctl/koffer bundle \
        --config https://git.io/JIY6k
```
### 2.b Run Koffer with nested container build support
  - Example: [collector-operators](https://github.com/CodeSparta/collector-operators) plugin
```
podman run -it --rm \
    --env BUNDLE=false \
    --publish 10.88.0.1:5000:5000 \
    --volume /tmp/koffer/.ssh:/root/.ssh:z \
    --env OPERATORS=kubevirt-hyperconverged \
  quay.io/cloudctl/koffer:latest bundle \
    --plugin collector-operators
```
### 3. Verify Bundle
```
 sudo du -sh ~/bundle/*
```
