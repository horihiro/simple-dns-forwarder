# Simple DNS forwarder
This is a conatiner image of simple DNS forwarder which works on Azure Container Instances.

## Backgrond
As [this documentation](https://docs.microsoft.com/azure/private-link/private-endpoint-dns#on-premises-workloads-using-a-dns-forwarder) describes, it is needed to use DNS forwarder in order that client PCs which connects to the VNET via ExpressRoute / P2S VPN and so on use Azure Private DNS Zone. This is because the Private DNS Zone can be used only for name resolution query from nodes in the VNET.

## How to use
This image is based on [distroless](https://github.com/GoogleContainerTools/distroless) and contains only [`dnsmasq`](https://thekelleys.org.uk/dnsmasq/doc.html) for forwarding DNS queries.

If you want to this on Azure Container Instances(ACI) in your Virtual Network, please take care the following points.

  - Container details  
    This image is registered in GitHub Container Registry, so the following setting is needed.

    - Select `Docker Hub or other registry` for `Image source`
    - Select `Public` for `Image type`
    - Input `ghcr.io/horihiro/simple-dns-forwerder:<tagName>` as `Image` and replace `<tagName>` to a tag from the [list](https://github.com/horihiro/simple-dns-forwarder/pkgs/container/simple-dns-forwarder/versions)
    - Select `Linux` for `OS type`
      
    ![image](https://user-images.githubusercontent.com/4566555/135413167-94e03e07-18f8-4ec6-8391-eafc33442000.png)

  - Port and Port Protocol  
    Set port 53 and UDP for DNS protocol  
    ![image](https://user-images.githubusercontent.com/4566555/135404958-d4f3eff2-a4bd-4728-8a6b-b3a9d88058ce.png)

  - Command override  
    Set following commands
    > ["dnsmasq", "--no-daemon", "--server", "[168.63.129.16](https://docs.microsoft.com/azure/virtual-network/what-is-ip-address-168-63-129-16)"]

    ![image](https://user-images.githubusercontent.com/4566555/135405110-d4bae919-94d1-4f11-b9c9-cfe172fbfbb9.png)

