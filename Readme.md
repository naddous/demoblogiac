# Infrastructre information

React SPA application is bundled into a Docker image. As shipped npm server is meant for devlopemnt, application was layerd on top 
of an Nginx web server (same docker image).
https://hub.docker.com/repository/docker/naddous/demoblog

Testing:
     test case was added to demoblog repo. The test is invoked during Docker build steps.

Build 
    Docker image is built using CI/CD pipeline described in Jenkins file in demoblog repo.


Application is delpoyed to a Kubernets cluster (3 nodes cluster ) running on GPC.

Envionments 

a Stage enviroment was stood up. This was done via a modular Terrforrm configuration which would easily addition of other enviroments.
(see this repo)




# Environment Initial Setup


## GKE Setup

Below are initial setup steps for the GKE environment. Subsequent changes will be done via Terraform and CI/CD pipeline automation

1-Create a project structure to organize infrastructure artifacts.

ProductX

 

2-Setup a service account (used by Terraform)

[https://console.cloud.google.com/iam-admin/serviceaccounts](https://console.cloud.google.com/iam-admin/serviceaccounts)

	user terraform


        Grant this account these three roles:
        Kubernetes Cluster Admin
        Storage Admin
        Create Service Account

3-Enable API access and download API key (in json format), save file as : "&lt;BASDIR>/terraform/cred/account.json"

 


## CI/CD Setup 

Follow the following to set up CI/CD environment if needed. For simplification, CI/CD env has only one host and uses local files for configuration and state management. 

Procedure was tested on an Ubuntu 18.x workstation.

Install Tools:
Install the following to &lt;$BASDIR>/tools/
Install Docker (used for image builds)
Install the Google Cloud SDK (user path) ([https://cloud.google.com/sdk/docs/downloads-interactive](https://cloud.google.com/sdk/docs/downloads-interactive))
Install kubectl (in user path) 
&lt;$BASDIR>/tools/jenkins
&lt;$BASDIR>/tools/terraform

wget [https://releases.hashicorp.com/terraform/0.12.24/terraform_0.12.24_linux_amd64.zip](https://releases.hashicorp.com/terraform/0.12.24/terraform_0.12.24_linux_amd64.zip)

unzip  terraform_0.12.24_linux_amd64.zip


### Jenkins Setup

Plugins

[https://plugins.jenkins.io/google-kubernetes-engine/](https://plugins.jenkins.io/google-kubernetes-engine/)


### Terraform setup

Terraform is used to provision the environment infrastructure initially as well as when infrastructure changes are required 
cd &lt;BASDIR>/terraform/
git &lt;iac repo git URL>
cd environments/stage
&lt;Initialize Google Cloud SDK if ot done already>
gcloud init

export GOOGLE_APPLICATION_CREDENTIALS="&lt;BASDIR>/terraform/cred/account.json"

terraform get 
terraform init
terraform plan
terraform apply


# Local Testing Setup

You may test docker image for the project locally as follows.


#### Prerequisite 



*   Install Docker 

    [Container runtimes - Kubernetes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)

*   Add your user to docker group (optional), alternatively append sudo to run various Docker commands

docker pull naddous:demoblog
docker run -p 8888:80 demoblog