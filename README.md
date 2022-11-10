# NGINX Configurations for Sample Application

This repo contains NGINX configuration files for a sample application. Configurations are stored in code, and modifications to the NGINX configuration files will result in a [GitHub Actions workflow to update NGINX for Azure](https://docs.nginx.com/nginx-for-azure/management/nginx-configuration/#nginx-configuration-automation-workflows).

## Deploy NGINX for Azure

Refer to the GitHub Repo [F5 NGINX for Azure Deployment with Demo Application in Multiple Regions](https://github.com/f5devcentral/f5-digital-customer-engagement-center/tree/main/solutions/delivery/application_delivery_controller/nginx/nginx-for-azure)

Terraform will output a GitHub actions workflow file named 'nginxGithubActions.yml'. Add the workflow file to your GitHub repository storing the NGINX configurations. The workflow file needs to be placed into the following location:

```
.github/workflows/nginxGithubActions.yml
```

## NGINX Configurations

The configuration directory has the following file and folder structure. Refer to the demo repo [CI/CD Pipeline NGINX Configurations](https://github.com/f5devcentral/f5-digital-customer-engagement-center/tree/main/solutions/delivery/application_delivery_controller/nginx/nginx-for-azure#cicd-pipeline-nginx-configurations) to learn how these files are used.


```
└── configs
    ├── nginx.conf
    └── upstreams
        ├── app1-east-vmss.conf
        ├── app1-vmss.conf
        └── app1-west-vmss.conf
```

