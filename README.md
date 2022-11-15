# NGINX Configurations for Sample Application

## To Do

- n/a

## Issues

- Find an issue? Fork, clone, create branch, fix and PR. I'll review and merge into the main branch. Or submit a GitHub issue with all necessary details and logs.

## Contents

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Day 1 Infrastructure](#day-1-infrastructure)
- [Day 2 NGINX Configurations](#day-2-nginx-configurations)

## Introduction

This repo contains NGINX configuration files for a sample application. The sample NGINX configuration repo goes hand in hand with the deployment repo ["F5 NGINX for Azure Deployment with Demo Application in Multiple Regions"](https://github.com/f5devcentral/f5-digital-customer-engagement-center/tree/main/solutions/delivery/application_delivery_controller/nginx/nginx-for-azure) in which all the infrastructure components are deployed in a "Day 1" concept. The NGINX configurations are managed in a "Day 2" concept.

```
└── configs
    ├── nginx.conf
    └── upstreams
        ├── app1-east-vmss.conf
        ├── app1-vmss.conf
        └── app1-west-vmss.conf
```

**Note:** Refer to the deployment repo README section [CI/CD Pipeline NGINX Configurations](https://github.com/f5devcentral/f5-digital-customer-engagement-center/tree/main/solutions/delivery/application_delivery_controller/nginx/nginx-for-azure#cicd-pipeline-nginx-configurations) to learn how these files are used.

## Requirements

### Requirement #1 - GitHub Repository to Store NGINX Configurations

You will store and modify NGINX configurations in a GitHub repo. The easiest way is to [fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) this repo.

1. Select the **Fork** button in the upper-right
2. Choose an Owner and Repository Name for the new repo
3. Create fork
4. Your web browser will automatically navigate to the new repo
5. Copy the URL for the repo by hitting the green **Code** button (ex. https://github.com/MyOrg123XYZ/myAppConfigs.git)
    - Save URL for later

### Requirement #2 - GitHub Access to Modify Azure Environment

The GitHub Actions workflow uses the Azure login action and requires OpenID Connect to establish trust with Azure. See [GitHub Actions to Connect to Azure](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-cli%2Cwindows) for details. Here's a quick walk-through.

1. [Create an Azure Active Directory application and service principal](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-cli%2Cwindows#create-an-azure-active-directory-application-and-service-principal) with "Contributor" access to your Azure subscription
    - Copy the values for clientId, subscriptionId, and tenantId
2. [Add federated credentials](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-cli%2Cwindows#add-federated-credentials) for the NGINX configuration repo
    - Your GitHub organization/username (ex. MyOrg123XYZ)
    - Your NGINX configuration repo name (ex. myAppConfigs)
    - Branch (ex. main)
3. [Create GitHub secrets](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-cli%2Cwindows#create-github-secrets) in the NGINX configuration repo using values from the Azure Active Directory application (copied from step 1)
    - AZURE_CLIENT_ID
    - AZURE_TENANT_ID
    - AZURE_SUBSCRIPTION_ID
4. Create a workflow file using GitHub secrets (see sample workflow YAML file in this repo)

### Requirement #3 - Azure PowerShell Access to Modify GitHub Repository

The Azure Function runtime uses PowerShell and updates the NGINX configuration files in the repo. Git actions include clone, pull, add, and commit. Secure communication is accomplished with a GitHub access token retrieved from an Azure Key Vault secret. See [GitHub Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) and [Azure Key Vault Secret](https://learn.microsoft.com/en-us/azure/key-vault/secrets/quick-create-cli) for details. Here's a quick walk-through.

1. In your GitHub profile settings, create a [fine-grained GitHub access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-fine-grained-personal-access-token)
    - Name of token, expiration, Resource Owner
2. Add **Repository access**:
    - Select the new/forked NGINX configuration repo
3. Add **Repository permissions**:
    - Commit statuses = Read and write
    - Contents = Read and write
    - Metadata = Read-only
4. Generate token and copy the value
5. In your Azure subscription, [create an Azure Key Vault and secret](https://learn.microsoft.com/en-us/azure/key-vault/secrets/quick-create-cli)
    - secret value = GitHub access token (copied from step 4)
6. Copy the names of the following Azure items:
    - Key Vault name and Key Vault secret name
    - Save for later

## Day 1 Infrastructure

Refer to the deployment repo ["F5 NGINX for Azure Deployment with Demo Application in Multiple Regions"](https://github.com/f5devcentral/f5-digital-customer-engagement-center/tree/main/solutions/delivery/application_delivery_controller/nginx/nginx-for-azure) to deploy NGINX for Azure, a sample application in multiple regions, and the necessary Azure infrastructure components.

## Day 2 NGINX Configurations

Terraform will create a GitHub actions workflow file after completing the "Day 1" infrastructure deployment. The file is named 'nginxGithubActions.yml'. Add the GitHub actions workflow file to the sample NGINX configurations repo. Once the workflow file has been added to the sample NGINX configuration repo, modifications to the NGINX configuration files will result in a [GitHub Actions workflow to update NGINX for Azure](https://docs.nginx.com/nginx-for-azure/management/nginx-configuration/#nginx-configuration-automation-workflows).

**Note:** The workflow file needs to be placed into the following location:

```
.github/workflows/nginxGithubActions.yml
```
