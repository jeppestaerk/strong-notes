---
title: Create a HA K3s kubernetes cluster
description: Setup a HA K3s kubernetes cluster with proxmomx, ansible and terraform
---

## Requirements

!!! summary "Preperations"

    * [x] A running [Proxmox](/metal/proxmox) instance
    * [x] [Ansible](/metal/asible) installed on client machine
    * [x] [Terraform](/metal/terraform) installed on client machine

## Architecture

```mermaid
flowchart LR
    subgraph servers ["K3s Servers"]
    direction TB
    server2("K3s-Server2")
    server1("K3s-Server1")
    server3("K3s-Server3")
    server2 --> server1
    server1 --> server2
    server3 --> server1
    server1 --> server3
    end
    subgraph dbs ["Databases"]
    direction TB
    db1[("PostgreSql
    Master")]
    db2[("PostgreSql
    Replica")]
    db1 --> db2
    end
    servers --> dbs
    subgraph agents ["K3s Agents"]
    direction TB
    agent1("K3s-Agent1")
    agent2("K3s-Agent2")
    agent3("K3s-Agent3")
    end
    servers ---> agent1 & agent2 & agent3
```
