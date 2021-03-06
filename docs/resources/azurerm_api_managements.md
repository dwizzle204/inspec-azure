---
title: About the azurerm_api_managements Resource
platform: azure
---

# azurerm\_api\_managements

Use the `azurerm_api_managements` InSpec audit resource to test properties and configuration of Azure Api Management Service.
<br />

## Azure REST API version

This resource interacts with version `2019-12-01` of the Azure Management API. For more
information see the [Official Azure Documentation](https://docs.microsoft.com/en-us/rest/api/apimanagement/2019-12-01/apimanagementservice/get).

At the moment, there doesn't appear to be a way to select the version of the
Azure API docs. If you notice a newer version being referenced in the official
documentation please open an issue or submit a pull request using the updated
version.

## Availability

### Installation

This resource is available in the `inspec-azure` [resource
pack](https://www.inspec.io/docs/reference/glossary/#resource-pack). To use it, add the
following to your `inspec.yml` in your top-level profile:

    depends:
      inspec-azure:
        git: https://github.com/inspec/inspec-azure.git

You'll also need to setup your Azure credentials; see the resource pack
[README](https://github.com/inspec/inspec-azure#inspec-for-azure).

## Syntax

An `azurerm_api_managements` resource block returns all Azure Api Management Services, either within a Resource Group (if provided), or within an entire Subscription.

    describe azurerm_api_managements do
      ...
    end

  or

    describe azurerm_api_managements(resource_group: 'my-rg') do
      ...
    end

<br />

## Examples

The following examples show how to use this InSpec audit resource.

### Check Api Management Services  are present

    describe azurerm_api_managements do
      it            { should exist }
      its('names')  { should include 'my-apim' }
    end
<br />

## Filter Criteria

* `name`

### name

Filters the results to include only those Application Gateways which match the given name. This is a string value.

    describe azurerm_api_managements.where{ name.eql?('production-apim-01') } do
      it { should exist }
    end

* `location`

### location

Filters the results to include only those Application Gateways which reside in a given location. This is a string value.

    describe azurerm_api_managements.where{ location.eql?('eastus') } do
      it { should exist }
    end

## Attributes

- `ids`
- `names
- `locations`
- `properties`
- `tags`
- `types`

### ids
Azure resource ID.

### names
Api Management Service name, e.g. `my-apim`.

    its('names') { should include 'my-apim' }


### locations
Resource location, e.g. `eastus`.

    its('locations') { should_not include 'eastus' }

### properties
A collection of additional configuration properties related to the API Management Service, e.g. `publisherEmail`.

### tags
Resource tags applied to the Api Management Service.

### types
The type of Resource, typically `Microsoft.ApiManagement/service`.

## Matchers

This InSpec audit resource has the following special matchers. For a full list of available matchers,
please visit our [Universal Matchers page](https://www.inspec.io/docs/reference/matchers/).

### exists

The control will pass if the filter returns at least one result. Use
`should_not` if you expect zero matches.

    describe azurerm_api_managements do
      it { should exist }
    end

## Azure Permissions

Your [Service
Principal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal)
must be setup with a `contributor` role on the subscription you wish to test.
