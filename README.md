# Simple DNS forwarder for Azure VNET
This is a conatiner image of simple DNS forwarder, which works on Azure Container Instances, for Azure VNET.

## Background
As [this documentation](https://docs.microsoft.com/azure/private-link/private-endpoint-dns#on-premises-workloads-using-a-dns-forwarder) describes, it is needed to use DNS forwarder in order that client PCs which connects to the VNET via ExpressRoute / P2S VPN and so on use Azure Private DNS Zone. This is because the Private DNS Zone can be used only for name resolution query from nodes in the VNET.

## How to use
This image is based on [distroless](https://github.com/GoogleContainerTools/distroless) and contains only [`dnsmasq`](https://thekelleys.org.uk/dnsmasq/doc.html) for forwarding DNS queries.

If you want to this on Azure Container Instances(ACI) in your Virtual Network, please take care the following points on deployment.

  - Container details in `Basics`  
    This image is registered in GitHub Container Registry, so the following setting is needed.

    - Select `Docker Hub or other registry` for `Image source`
    - Select `Public` for `Image type`
    - Input `ghcr.io/horihiro/simple-dns-forwerder:<tagName>` as `Image` and replace `<tagName>` to a tag from the [list](https://github.com/horihiro/simple-dns-forwarder/pkgs/container/simple-dns-forwarder/versions)
    - Select `Linux` for `OS type`
      
    ![image](https://user-images.githubusercontent.com/4566555/135413167-94e03e07-18f8-4ec6-8391-eafc33442000.png)

  - Ports in `Networking`  
    - Set VNET and subnet you want to deploy to
    - Set port 53 and UDP for DNS protocol  
    ![image](https://user-images.githubusercontent.com/4566555/135404958-d4f3eff2-a4bd-4728-8a6b-b3a9d88058ce.png)

  - Command override in `Advanced`  
    Set following commands
    > ["dnsmasq", "--no-daemon", "--server", "[168.63.129.16](https://docs.microsoft.com/azure/virtual-network/what-is-ip-address-168-63-129-16)"]

    ![image](https://user-images.githubusercontent.com/4566555/135405110-d4bae919-94d1-4f11-b9c9-cfe172fbfbb9.png)

After the deployment, 

  1. Check the private IP address in `General` of the ACI  
    ![image](https://user-images.githubusercontent.com/4566555/135694859-6de8ccc5-bc38-4965-99a0-d6e590d8e4e4.png)
  1. Set the private IP address to the `DNS servers` of the VNET you deployed ACI in  
    ![image](https://user-images.githubusercontent.com/4566555/135694798-b96aa88c-aa2d-483a-b3bd-cdcb4b80559c.png)