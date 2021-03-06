# CLOUD AZURE SignalFx integrations

## How to use this module

Enable SignalFX integration:

```hcl
module "signalfx-integrations-cloud-azure" {
  source                 = "github.com/claranet/terraform-signalfx-integrations.git//cloud/azure/sfx?ref={revision}"

  azure_tenant_id        = var.azure_tenant_id
  azure_subscription_ids = [var.azure_subscription_id]

  azure_sp_application_id    = azuread_application.signalfx_integration.application_id
  azure_sp_application_token = azuread_service_principal_password.signalfx_integration_sp_pwd.value
}
```

## Providers

| Name | Version |
|------|---------|
| signalfx | >= 4.26.4 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| azure\_sp\_application\_id | Azure Service Principal application ID | `string` | n/a | yes |
| azure\_sp\_application\_token | Azure Service Principal application token (or password) | `string` | n/a | yes |
| azure\_subscription\_ids | List of Azure Subscription IDs to monitor | `list(string)` | n/a | yes |
| azure\_tenant\_id | Azure Tenant ID/Directory ID | `string` | n/a | yes |
| enabled | Whether the Azure integration is enabled | `bool` | `true` | no |
| poll\_rate | Azure poll rate in seconds (One of 60 or 300) | `number` | `300` | no |
| services | Azure service metrics to import. Empty list imports all services | `list` | `[]` | no |
| suffix | Optional suffix to identify and avoid duplication of unique resources | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| signalfx\_integration\_name | The SignalFx integration name for Azure |
| signalfx\_integration\_poll\_rate | The Azure poll rate configured in the SignalFx integration |
| signalfx\_integration\_services | The list of Azure services configured in SignalFx integration |
| signalfx\_integration\_subscriptions | The Azure subscriptions ids configured in the SignalFx integration |
| signalfx\_integration\_tenant | The Azure tenant id configured in the SignalFx integration |

## Related documentation

[Official documentation](https://docs.signalfx.com/en/latest/integrations/azure-info.html#connect-to-microsoft-azure)

## Requirements

You need to configure your SignalFx providers.
Credentials could be set in your `terraform.tfvars`.

```
variable "sfx_token" {
  description = "User API token from an admin on SignalFx organization"
  type        = string
}

provider "signalfx" {
  auth_token = var.sfx_token                  # admin temporary session token
  api_url    = "https://api.eu0.signalfx.com" # change for your realm
}
```

## Notes

* As for any integration configuration you need a [**session**](https://docs.signalfx.com/en/latest/admin-guide/tokens.html#user-api-access-tokens) token from an admin.
* You need to have an Azure Service Principal created with `Reader` Role granted over the SignalFX scope (ie: all monitored Azure Subscriptions).
