# Troubleshoot ExpressRoute connection issues

As an Azure network engineer supporting an ExpressRoute deployment, you will have to diagnose and resolve any ExpressRoute connection issues that arise. 

ExpressRoute connectivity traditionally involves three distinct network zones, as follows:

- Customer Network

- Provider Network

- Microsoft Datacenter

  > [!Note] In the ExpressRoute direct connectivity model (offered at 10/100 Gbps bandwidth), customers can directly connect to Microsoft Enterprise Edge (MSEE) routers' port. Therefore, in the direct connectivity model, there are only customer and Microsoft network zones.

## **Verify circuit provisioning and state through the Azure portal**

Provisioning an ExpressRoute circuit establishes a redundant Layer 2 connections between CEs/PE-MSEEs (2)/(4) and MSEEs (5). 

> [!Tip] A service key uniquely identifies an ExpressRoute circuit. Should you need assistance from Microsoft or from an ExpressRoute partner to troubleshoot an ExpressRoute issue, provide the service key to readily identify the circuit.

In the Azure portal, open the ExpressRoute circuit blade. In the section of the blade, the ExpressRoute essentials are listed as shown in the following screenshot:

​	![Azure portal - view circuit status](../media/portal-circuit-status.png)

In the ExpressRoute Essentials, Circuit status indicates the status of the circuit on the Microsoft side. Provider status indicates if the circuit has been Provisioned/Not provisioned on the service-provider side.

For an ExpressRoute circuit to be operational, the Circuit status must be Enabled, and the Provider status must be Provisioned.

> [!Note] After configuring an ExpressRoute circuit, if the Circuit status is stuck in not enabled status, contact [Microsoft Support](https://portal.azure.com/?). On the other hand, if the Provider status is stuck in not provisioned status, contact your service provider.

## **Validate Peering Configuration**

After the service provider has completed the provisioning the ExpressRoute circuit, multiple eBGP based routing configurations can be created over the ExpressRoute circuit between CEs/MSEE-PEs (2)/ (4) and MSEEs (5). Each ExpressRoute circuit can have: Azure private peering (traffic to private virtual networks in Azure), and/or Microsoft peering (traffic to public endpoints of PaaS and SaaS). 

> [!Note] In IPVPN connectivity model, service providers handle the responsibility of configuring the peering (layer 3 services). In such a model, after the service provider has configured a peering and if the peering is blank in the portal, try refreshing the circuit configuration using the refresh button on the portal. This operation will pull the current routing configuration from your circuit.

In the Azure portal, status of an ExpressRoute circuit peering can be checked under the ExpressRoute circuit blade. In the overview section of the blade, the ExpressRoute peering would be listed as shown in the following screenshot:

​	![Azure portal - view peering status](../media/portal-private-peering.png)

In the preceding example, as noted Azure private peering is provisioned, whereas Azure public and Microsoft peering are not provisioned. A successfully provisioned peering context would also have the primary and secondary point-to-point subnets listed. The /30 subnets are used for the interface IP address of the MSEEs and CEs/PE-MSEEs. For the peering that are provisioned, the listing also indicates who last modified the configuration.

 

> [!Note] If enabling a peering fails, check if the primary and secondary subnets assigned match the configuration on the linked CE/PE-MSEE. Also check if the correct VlanId, AzureASN, and PeerASN are used on MSEEs and if these values map to the ones used on the linked CE/PE-MSEE. If MD5 hashing is chosen, the shared key should be same on MSEE and PE-MSEE/CE pair. Previously configured shared key would not be displayed for security reasons. Should you need to change any of these configuration on an MSEE router, refer to [Create and modify routing for an ExpressRoute circuit](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-routing-portal-resource-manager).

> [!Note] On a /30 subnet assigned for interface, Microsoft will pick the second usable IP address of the subnet for the MSEE interface. Therefore, ensure that the first usable IP address of the subnet has been assigned on the peered CE/PE-MSEE.

 

## Quiz title: Check your knowledge 



## Multiple Choice 

What property of an ExpressRoute circuit is useful when opening a support ticket with the service provider?

(X) Service key.{{Correct. A service key uniquely identifies an ExpressRoute circuit. If you need assistance from Microsoft or from an ExpressRoute partner to troubleshoot an ExpressRoute issue, provide the service key to readily identify the circuit.}} 

( ) Circuit name.{{Incorrect. The Circuit name may not be unique or easily searchable for the service provider.}} 

( ) Circuit number.{{Incorrect. There is no circuit number property.}}



## Multiple Choice 

You want to know if your service provider has made any changes that affect your circuit. Where can you check for this information?

(X) Check the **Last modified by** property of the relevant peering.{{Correct. Azure resources record information about changes, in this case, who made the last change.}} 

( ) Call the service provider and ask them.{{Incorrect. This may get you the correct answer, but it is not the quickest way to check.}} 

( ) Raise a support ticket with Microsoft.{{Incorrect. This may get you the correct answer, but it is not the quickest way to check.}}
