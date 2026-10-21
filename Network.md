
## Introduction 
This proof of concept shows how an organisation can move RADIUS authentication for its wired and wireless network to the cloud. The goal is to replace a local RADIUS server with a centrally hosted PacketFence instance that performs 802.1X network access control (NAC) for devices connecting through an Omada switch and access point.

### Topology
```mermaid
architecture-beta
    service bletchley(internet)[Bletchley network]

    group nuc(server)[NUC]
    group vnet(cloud)[VNET] in nuc
    group school(server)[SCHOOL] in vnet

    service pf(server)[PacketFence RADIUS] in vnet
    service opn(server)[OPNsense VPN tunnel to PF] in school
    service omada(server)[Omada controller] in school

    service sw(server)[Switch]
    service ap(internet)[AP]

    bletchley:R -- L:opn
    pf:B -- T:opn
    opn:R -- L:sw
    omada:T -- B:opn
    omada:R -- B:sw
    sw:R -- L:ap
```
