# adopt_hybrid_terraform

Example steps for an aka.ms/adopt/hybrid deployment using Terraform including OpenID Connect for the pipeline.

The service principal will have far reaching permissions so the top level management group will be manually created by someone with Global Admin permissions.

## Prereqs

1. Bash environment, e.g. Ubuntu on WSL2
1. Azure CLI
1. GitHub CLI

Check that GitHub is authenticated. Check the Azure CLI is authenticated and in the right context.

## Initial steps

1. Elevate permission to Owner at the tenant root group

    Requires Global Administrator.

    ```bash
    my_object_id=$(az ad signed-in-user show --query id --output tsv)
    root_tenant_group_scope="/$(az account show --query tenantId --output tsv)"
    az role assignment create --role Owner --assignee $my_object_id --scope $root_tenant_group_scope
    ```

NOT WORKING!

**The request did not have a subscription or a valid tenant level resource provider.**

1. Clone this repo

    ````bash
    git clone https://github.com/richeney/adopt_hybrid_terraform && cd adopt_hybrid_terraform
    gitHubUser=$(git remote get-url origin | cut -f4 -d/)
    gitHubRepo=$(git remote get-url origin | cut -f5 -d/)
    ```

1. Set default variables

    These values will be set locally in `.azure/config`.

    ```bash
    az config set --local defaults.group=adopt_hybrid_terraform
    az config set --local defaults.location="UK South"
    ```

    > Feel free to change the values from the defaults shown.

1. Create a service principal, resource group, storage account

    ```bash
    az group create --name "$(az config get --local defaults.group --query value --output tsv)"




## References

* [Terraform ALZ module wiki](https://aka.ms/alz/tf/wiki)
* Level 100 | [Deploy Default Configuration](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BExamples%5D-Deploy-Default-Configuration)
