# azr-tf-module-subnet

[Terraform](https://www.terraform.io) Module to create **[NAME]** in Azure

<!-- BEGIN_TF_DOCS -->
## Prerequisites

- [Terraform](https://releases.hashicorp.com/terraform/) v0.12+

## Providers

| Name | Version |
|------|---------|
| <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm) | >=2.36.0 |

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >=0.14.9 |
| <a name="requirement_azurerm"></a> [azurerm](#requirement\_azurerm) | >=2.36.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [azurerm_subnet.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/subnet) | resource |
| [azurerm_virtual_network.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/virtual_network) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_resource_group_name"></a> [resource\_group\_name](#input\_resource\_group\_name) | (Required) The name of the resource group in which to get the data of the the virtual network. | `string` | n/a | yes |
| <a name="input_subnets"></a> [subnets](#input\_subnets) | (Required) Manages a subnet. Subnets represent network segments within the IP space defined by the virtual networ (https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/subnet). | <pre>list(object({<br>    name                                           = string<br>    address_prefixes                               = list(string)<br>    enforce_private_link_endpoint_network_policies = bool<br>    enforce_private_link_service_network_policies  = bool<br>    service_endpoints                              = list(string)<br>    deligation = object({<br>      name = string<br>      service_delegation = object({<br>        actions = list(string)<br>        name    = string<br>      })<br>    })<br>  }))</pre> | n/a | yes |
| <a name="input_virtual_network_name"></a> [virtual\_network\_name](#input\_virtual\_network\_name) | (Required) The name of the virtual network. Changing this forces a new resource to be created. | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_address_prefixes"></a> [address\_prefixes](#output\_address\_prefixes) | Contains a list of the address prefixes of the subnets |
| <a name="output_ids"></a> [ids](#output\_ids) | Contains a list of the resource id of the subnets |
| <a name="output_names"></a> [names](#output\_names) | Contains a list of the resource name of the subnets |
| <a name="output_names_services"></a> [names\_services](#output\_names\_services) | Contains a list of the resource name of the azuer reserved subnets |

## Example

```hcl
resource "azurerm_resource_group" "this" {
  name     = uuid()
  location = "westeurope"
}

module "subnet" {
  source               = "./module"
  virtual_network_name = "vn-example"
  resource_group_name  = azurerm_resource_group.this.name
  subnets = [
    {
      name : "AzureBastionSubnet"
      address_prefixes : ["10.255.0.0/26"]
      enforce_private_link_endpoint_network_policies : true
      enforce_private_link_service_network_policies : false
      service_endpoints : [
        "Microsoft.AzureActiveDirectory"
      ]
      deligation : {
        name : null
        service_delegation : {
          actions : null
          name : null
        }
      }
    },
    {
      name : "sn-example-endpoints"
      address_prefixes : ["10.255.0.64/26"]
      enforce_private_link_endpoint_network_policies : false
      enforce_private_link_service_network_policies : false
      service_endpoints : [
        "Microsoft.AzureActiveDirectory",
        "Microsoft.Storage",
      ]
      deligation : {
        name : null
        service_delegation : {
          actions : null
          name : null
        }
      }
    }
  ]
  tags = {
    Created : "Terraform",
    Project : "Simple Example Virtual Network"
  }
}
```


<!-- END_TF_DOCS -->
## Authors

Originally created by [Patrick Hayo](http://github.com/patrickhayo)

## License

[MIT](LICENSE) License - Copyright (c) 2022 by the Author.
