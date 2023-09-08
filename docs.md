# Document: Deploying Azure Landing Zones

## Introduction

This document provides a comprehensive guide on deploying Azure Landing Zones using the [Azure/terraform-azurerm-caf-enterprise-scale](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale) module. Deploying landing zones is crucial for setting up a well-structured Azure environment with proper configurations and policies. We will demonstrate how to utilize multiple module declarations and remote state management to ensure consistency and code separation within Terraform workspaces.

## Prerequisites

Before proceeding with the deployment, ensure that you have the following prerequisites in place:

1. **Terraform CLI**: You must have the Terraform CLI installed and configured with access to Azure.

2. **GitHub Account**: You need a GitHub account and a Git client installed for version control.

3. **Backend Configuration**: Choose a suitable backend for storing Terraform state files. While this example uses a local backend for simplicity, it's recommended to use a more robust backend for production scenarios.

4. **Azure Permissions**: You should have the necessary permissions to create resources in Azure as specified in the [module permissions documentation](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BUser-Guide%5D-Module-permissions).

## Overview

This example builds upon existing templates and configurations, including:

- [Deploying a single module instance with default settings](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BExamples%5D-Deploy-using-default-settings).
- [Deploying a single module instance with custom settings](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BExamples%5D-Deploy-using-custom-settings).
- [Deploying multiple module instances with orchestration](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BExamples%5D-Deploy-using-multiple-module-declarations-with-orchestration).

The key differentiator in this example is the use of multiple Terraform workspaces, one for each module instance, and the integration of remote state data sources to establish consistency across core, management, and connectivity resources.

![Architecture diagram](https://raw.githubusercontent.com/Azure/terraform-azurerm-caf-enterprise-scale/main/docs/media/l400-multi.png)

## Code Structure

The code for this example is structured as follows:

```
├── connectivity
│   ├── main.tf
│   ├── outputs.tf
│   ├── settings.connectivity.tf
│   └── variables.tf
├── core
│   ├── lib
│   │   └── archetype_definition_customer_online.json
│   ├── main.tf
│   ├── remote.tf
│   ├── settings.core.tf
│   ├── settings.identity.tf
│   └── variables.tf
└── management
    ├── main.tf
    ├── outputs.tf
    ├── settings.management.tf
    └── variables.tf
```

Each folder represents a specific scope and contains the necessary files to configure and deploy a module instance. The core folder also includes a custom archetype definition file in the lib subfolder.

- **main.tf**: This file contains the module declaration and provider configuration.

- **outputs.tf**: Here, you'll find the module outputs used to share information between module instances.

- **settings.xxxx.tf**: These files contain local variables used to customize the module configuration.

- **variables.tf**: This file defines the input variables used to pass values from the orchestration module.

- **core/remote.tf**: This file contains data sources that retrieve outputs from the remote state for connectivity and management resources.

## Deployment Steps

Due to the multiple workspaces involved, follow these deployment steps to ensure proper resource creation and dependency resolution:

1. **Deploy the Connectivity Resources**: From the connectivity module directory, execute the following commands:
   - Ensure you have a correct Azure connection configured with permissions as specified in the [module permissions documentation](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BUser-Guide%5D-Module-permissions).
   - Initialize the Terraform workspace: `terraform init`
   - Generate a plan: `terraform plan -out=tfplan`, making sure to specify required variables for the connectivity module.
   - Review the plan output (optionally as a JSON file): `terraform show -json ./tfplan`
   - Start the deployment: `terraform apply ./tfplan` and follow the prompts.

2. **Deploy the Management Resources**: Repeat the above steps in the management module directory.

3. **Deploy the Core Resources**: Finally, deploy the core resources following the same steps from the core module directory.

By following this sequence, you ensure that all resources are created in the correct order to satisfy dependency requirements.

## Review and Customization

After completing the deployment, you can review the deployed resources in the Azure Portal or by using Azure CLI commands. You can also compare this example with the ones it's based on to understand the differences.

Customization is possible by adjusting the input variables' values or by adding more resources or policies. Refer to the [module variables documentation](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BUser-Guide%5D-Module-variables) for more information on configuring the module to suit your requirements.

For additional inspiration, consider exploring other examples provided by the module's creators.

## Advanced Customization

If you have specific requirements beyond what's covered here, you can explore advanced customization options:

- **Policy Definitions**: Modify policy definitions to enforce specific compliance standards for your resources. Refer to the [Azure Policy documentation](https://docs.microsoft.com/en-us/azure/governance/policy/overview) for guidance.

- **Resource Scaling**: Adjust the module's configurations to accommodate resource scaling based on your organization's needs. Ensure you follow Azure's best practices for scalability.

- **Logging and Monitoring**: Implement comprehensive logging and monitoring solutions to keep track of resource activity and security events. Azure offers various tools and services for this purpose.

## Conclusion

Deploying Azure Landing Zones using the [Azure/terraform-azurerm-caf-enterprise-scale](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale) module provides a structured and consistent way to set up your Azure environment. By following the steps and guidelines outlined in this document, you can establish a solid foundation for your cloud infrastructure, ensuring compliance and reliability.

## References

1. [Azure/terraform-azurerm-caf-enterprise-scale](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale)

2. [Terraform CLI Downloads](https://www.terraform.io/downloads.html)

3. [GitHub](https://github.com/)

4. [Git Downloads](https://git-scm.com/downloads)

5. [Terraform Backend Configuration](https://www.terraform.io/docs/language/settings/backends/index.html)

6. [Module Permissions Documentation](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BUser-Guide%5D-Module-permissions)

7. [Deploy using Default Settings](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BExamples%5D-Deploy-using-default-settings)

8. [Deploy using Custom Settings](https://

github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BExamples%5D-Deploy-using-custom-settings)

9. [Deploy using Multiple Module Declarations with Orchestration](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BExamples%5D-Deploy-using-multiple-module-declarations-with-orchestration)

10. [Architecture Diagram](https://raw.githubusercontent.com/Azure/terraform-azurerm-caf-enterprise-scale/main/docs/media/l400-multi.png)

11. [Module Variables Documentation](https://github.com/Azure/terraform-azurerm-caf-enterprise-scale/wiki/%5BUser-Guide%5D-Module-variables)

12. [Azure Policy Documentation](https://docs.microsoft.com/en-us/azure/governance/policy/overview)
