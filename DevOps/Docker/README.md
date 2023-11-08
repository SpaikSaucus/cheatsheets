# CheatSheets Docker

## Table of Contents
- [Sidecar](#sidecar)
- [Playground](#playground)


## Sidecar
A sidecar is a separate container that runs alongside an application container in a Kubernetes pod – a helper application of sorts. Typically, the sidecar is responsible for offloading functions required by all apps within a service mesh – SSL/mTLS, traffic routing, high availability, and so on – from the apps themselves, and implementing deployment testing patterns such as circuit breaker, canary, and blue‑green. Sidecars are sometimes used to aggregate and format log messages from multiple app instances into a single file.

The sidecar container shares the same lifecycle, resources, and network namespace as the main container but has its own file system and process space. This means that the sidecar container can access the same ports, volumes, and environment variables as the main container but cannot interfere with its execution.

## Playground
* https://labs.play-with-docker.com/