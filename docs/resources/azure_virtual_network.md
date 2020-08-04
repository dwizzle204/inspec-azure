---
title: About the azure_virtual_network Resource
platform: azure
---

# azure_virtual_network

Use the `azure_virtual_network` InSpec audit resource to test properties related to a virtual network.

## Azure REST API version, endpoint and http client parameters

This resource interacts with api versions supported by the resource provider.
The `api_version` can be defined as a resource parameter.
If not provided, the latest version will be used.
For more information, refer to [`azure_generic_resource`](azure_generic_resource.md).

Unless defined, `azure_cloud` global endpoint, and default values for the http client will be used .
For more information, refer to the resource pack [README](../../README.md). 

## Availability

### Installation

This resource is available in the `inspec-azure` [resource pack](/inspec/glossary/#resource-pack). To use it, add the following to your `inspec.yml` in your top-level profile:
```yaml
depends:
  - name: inspec-azure
    git: https://github.com/inspec/inspec-azure.git
```
You'll also need to setup your Azure credentials; see the resource pack [README](../../README.md).

## Syntax

The `resource_group` and virtual network `name` must be given as parameters.
```ruby
describe azure_virtual_network(resource_group: 'MyResourceGroup', name: 'MyVnetName') do
  #...
end
```
## Parameters

| Name                           | Description                                                                      |
|--------------------------------|----------------------------------------------------------------------------------|
| resource_group                 | Azure resource group that the targeted resource resides in. `MyResourceGroup`     |
| name                           | Name of the virtual network to test. `MyVNetwork`                                 |
| resource_id                    | The unique resource ID. `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Network/virtualNetworks/{vnName}`                                 |

Either one of the parameter sets can be provided for a valid query:
- `resource_id`
- `resource_group` and `name`

## Properties

| Property | Description |
|----------|-------------|
| subnets       | The list of subnet names that are attached to this virtual network. `its('subnets') { should eq ["MySubnetName"] }` |
| address_space | The list of address spaces used by the virtual network. `its('address_space') { should eq ["x.x.x.x/x"] }` |
| dns_servers | The list of DNS servers configured for the virtual network.  The virtual network returns these IP addresses when virtual machines makes a DHCP request. `its('dns_servers') { should eq ["x.x.x.x", "x.x.x.x"] }` |
| vnet_peerings | A mapping of names and the virtual network ids of the virtual network peerings. `its('vnet_peerings') { should eq "MyVnetPeeringConnection"=>"PeeringConnectionID"}` |
| enable_ddos\_protection | Boolean value showing if Azure DDoS standard protection is enabled on the virtual network. `its('enable_ddos_protection') { should eq true }` |
| enable_vm_protection | Boolean value showing if the virtual network has VM protection enabled. `its('enable_vm_protection') { should eq false }` |

For parameters applicable to all resources, such as `type`, `name`, `id`, `location`, `properties`, refer to [`azure_generic_resource`](azure_generic_resource.md#parameters).

Also, refer to [Azure documentation](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/virtualnetworks/get#virtualnetwork) for other properties available. 
Any property in the response may be accessed with the key names separated by dots (`.`).

## Examples

### Ensure that the Virtual Network Exists in the East US Region
```ruby
describe azure_virtual_network(resource_group: 'resource_group', name: 'MyVnetName') do
  it               { should exist }
  its('location')  { should eq 'eastus' }
end
```
### Ensure that the Virtual Network's DNS Servers are Configured as Expected
```ruby
describe azure_virtual_network(resource_group: 'resource_group', name: 'MyVnetName') do
    its('dns_servers') { should eq ["192.168.0.6"] }
end
```
### Ensure that the Virtual Network's Address Space is Configured as Expected
```ruby
describe azure_virtual_network(resource_group: 'resource_group', name: 'MyVnetName') do
    its('address_space') { should eq ["192.168.0.0/24"] }
end
```    
## Matchers

This InSpec audit resource has the following special matchers. For a full list of available matchers, please visit our [Universal Matchers page](https://www.inspec.io/docs/reference/matchers/).

### exists
```ruby
# If a virtual network is found it will exist
describe azure_virtual_network(resource_group: 'MyResourceGroup', name: 'MyVnetName') do
  it { should exist }
end

# virtual networks that aren't found will not exist
describe azure_virtual_network(resource_group: 'MyResourceGroup', name: 'DoesNotExist') do
  it { should_not exist }
end
```
## Azure Permissions

Your [Service Principal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal) must be setup with a `contributor` role on the subscription you wish to test.
