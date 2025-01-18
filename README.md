# AZ DNS Forwarder

source project: https://github.com/whiteducksoftware/az-dns-forwarder

## Project Overview
This project provides a containerized DNS server that forwards queries to Azure's internal DNS servers so that hostnames in the virtual network can be resolved from outside the network. This is helpful, for example, when you need to resolve Private Link-enabled resources from your on-premises networks connected via Side-to-Side VPN or ExpressRoute.

This Container can be deployed and exposed internally with Azure Kubernetes Service as well as Azure Container Instances.

![image](https://github.com/smoonlee/az-dns-forwarder/assets/22956963/0684b48b-b0d8-4cda-9097-a86a1fd5a1d6)

### Azure Kubernetes Service

You need to make sure that all needed private Azure DNS zones are linked to the virtual network used for AKS. Without this, the DNS forwarder will not be able to resolve them.

```
kubectl apply -f https://github.com/smoonlee/az-dns-forwarder/master/deployAzDNSFwd.yaml
```

This will deploy the Azure DNS Forwarder container as Deployment with 3 replicas. It also creates a LoadBalancer service using an internal Azure Loadbalancer to expose the DNS forwarder internally. 

### Azure Container Instances

You can also run the DNS Forwarder as a serverless instance with ACI. Once again, you will need to make sure to expose ACI internally and make sure that all needed Azure private DNS zones are linked to the used virtual network.

```
az container create \
  --resource-group <your-rg> \
  --name dns-forwarder \
  --image ghcr.io/smoonlee/az-dns-forwarder/az-dns-forwarder:<version> \
  --cpu 1 \
  --memory 0.5 \
  --restart-policy always \
  --vnet <your-vnet> \
  --subnet <your-subnet> \
  --ip-address private \
  --location <your-location> \
  --os-type Linux \
  --port 53 \
  --protocol UDP
```

## the alternative - Azure Private DNS Resolver
Azure DNS Private Resolver requires an Azure Virtual Network. When you create an Azure DNS Private Resolver inside a virtual network, one or more inbound endpoints are established that can be used as the destination for DNS queries. The resolver's outbound endpoint processes DNS queries based on a DNS forwarding ruleset that you configure. DNS queries that are initiated in networks linked to a ruleset can be sent to other DNS servers.

https://learn.microsoft.com/en-us/azure/dns/dns-private-resolver-overview

## Azure Private DNS Resolver Pricing 
![image](https://github.com/smoonlee/az-dns-forwarder/assets/22956963/5e68470f-ac5b-4415-b04f-ab098607f5e3)


https://azure.microsoft.com/en-in/pricing/details/dns/
