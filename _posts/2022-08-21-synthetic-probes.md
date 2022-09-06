---
title: Monitoring cluster state through remote synthetic probes with CloudProber
author: clarkezone
date: 2022-08-21 00:34:00 +0800
categories: [Kubernetes, CloudProber, Prometheus]
tags: [manifests]
---
# Draft - work in progress
## Situation
Uptime is important for services.  And when running services for yourself, whilst you aren't tied to any SLA's or SLO's it's important to reliably know the state of your systems. For this kind of scenario synthetic probes are often used to provide fake traffic that simulates realworld use.  In order for the fake traffic to have enough realism to actually test the scenario, the obviously the requests need to be originating from outside the network hosting the services.

For homelab scenario 

## Prerequisits to follow along
k8s cluster (k3s)
kubectl
kubectl autocompletion
kubectl plugin system
klean plugin
k9s

For more detail on the above, see [[Kubernetes tools I used in 2022]] post

## Approach
Deploy a digital ocean k3s single node cluster using ansible and flux 
(temporary) manually deploy cert manager needs to move to flux
(temporary) manually deploy traefik with web front end needs to move to flux

Challenges access denied on ping.. turned out to be access to raw sockets.

## Steps

0. Reset
```
Delete DoNode
Erase tailscale
wipe known hosts

```bash
kubectl konfig export -k ~/.kube/config k3s-c1 k3s-c2 clarkecluster3-admin > ~/baseconfig
mv ~/.kube/config ~/.kube/backup-8-31
mv ~/baseconfig ~/.kube/config
```

1. Create k3s host in digital ocean
 
```bash
ansible-playbook k3sbox.yml --extra-vars=@vars/k3sflux-dohost.yml --extra-vars=@vars/k3sflux-user.yml --extra-vars=@vars/k3sflux-k3s.yml -K
```

2. Get config and merge

```bash
kubectl konfig export -k ~/.kube/config k3s-c1 k3s-c2 clarkecluster3-admin > ~/baseconfig

scp james@homelabmonitoring-do:/etc/rancher/k3s/k3s.yaml ~/monitoringkubeconfig.yml

./renamecontext.sh -i ~/monitoringkubeconfig.yml -c monitoringcluster -u monitoringuser -t monitoringcontext

kubectl konfig merge ~/baseconfig ~/monitoringkubeconfig.yml > ~/merged
```

Temp steps need automating:
Apply certmanager dir. Ensure cluster issuer is present before moving on

./scripts/install.sh traefik

Apply Traefik dir

When certmanager up, restart traefik daemonset

Update Tailscale DNS

dashboard should work

3. Flux

create known_hosts on the remotemonitoring

ansible-playbook k3sbox-configflux.yml --extra-vars=@vars/k3sflux-dohost.yml --extra-vars=@vars/k3sflux-user.yml -K
 
4. Firewall
 
Ansible-playbook k3sbox-firewall.yml --extra-vars=@vars/k3sflux-dohost.yml -K
