# DAMAP

This repo is based on the [Docker Compose DAMAP documentation](https://github.com/tuwien-csd/damap-backend/tree/next/docker "Docker Compose based DAMAP") and contains configuration files needed for creating deployment pipeline based on GitHub actions that deploys DAMAP Docker images as DAMAP stack to ACDH-CH Kubernetes environment. 

Due to specific ACDH-CH environment that is using centralized PostgresDB, dedicated PostgresDB and Keycloak Docker images are not used with this setup.

Environment variables needed for the Directus stack:

|Name|Required|Type|Level|Description|
|----|:------:|----|:---:|-----------|
|KUBE_CONFIG|:white_check_mark:|Secret|Org|base64 encoded K8s config file. Usually set at the Org level and shared by all (public) repositories. |
|C2_KUBE_CONFIG|:white_check_mark:|Secret|Org|If you deploy using the workflow for the second cluster the C2_ variant is used. |
|KUBE_NAMESPACE|:white_check_mark:|Variable|Repo/Env|The K8s namespace the deployment should be installed to. |
|PROXY_SERVICE_ID|:white_check_mark:|Variable|Env|A K8s label ID is attached to the workload/deployment with this value (usually a number) |
|FITS_SERVICE_ID|:white_check_mark:|Variable|Env|A K8s label ID is attached to the workload/deployment with this value (usually a number) |
|API_MOCK_SERVICE_ID|:white_check_mark:|Variable|Env|A K8s label ID is attached to the workload/deployment with this value (usually a number) |
|DAMAP_BE_SERVICE_ID|:white_check_mark:|Variable|Env|A K8s label ID is attached to the workload/deployment with this value (usually a number) |
|DAMAP_FE_SERVICE_ID|:white_check_mark:|Variable|Env|A K8s label ID is attached to the workload/deployment with this value (usually a number) |
|K8S_SECRET_PUBLIC_URL|:white_check_mark:|Variable|Env|The URI with https:// that should be configured for access to the service. |


### How to deploy new Directus instance

1. Create Kubernetes namespace.
2. Create PostgresDB database with postgis extension.
3. Create domain for the service and point it to the cluster.
4. Create new GitHub environment for the service that should have the same name as new GitHub branch that will be used for the new Directus instance.
5. Add GitHub environment variables and secrets described in the table above.
6. Create a new branch in this repo that should have tha same name as the Kubernetes namespace created in the first step.
8. The newly created GitHub branch will trigger the Github pipeline that will deploy new Directus stack to the Kubernetes Cluster.