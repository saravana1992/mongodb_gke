## Overview

In this guide we will walk through the steps necessary to get up and running with GKE and Helm. Here are the steps:

1. [Install the necessary tools](#installing-necessary-tools)
1. [Apply the Terraform code](#apply-the-terraform-code)
1. [Verify the Deployed Chart](#verify-the-deployed-chart)
1. [Destroy the Deployed Resources](#destroy-the-deployed-resources)

## Installing necessary tools

In addition to `terraform`, this example relies on `gcloud` and `kubectl` and `helm` tools to manage the cluster.

This means that your system needs to be configured to be able to find `terraform`, `gcloud`, `kubectl` and `helm`
client utilities on the system `PATH`. Here are the installation guides for each tool:

1. [`gcloud`](https://cloud.google.com/sdk/gcloud/)
1. [`kubectl`](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
1. [`terraform`](https://learn.hashicorp.com/terraform/getting-started/install.html)
1. [`helm`](https://docs.helm.sh/using_helm/#installing-helm) (Minimum version v3.0)


## Apply the Terraform Code

Now that all the prerequisite tools are installed, we are ready to deploy the GKE cluster!

1. If you haven't already, clone this repo:
   - `git clone https://github.com/saravana1992/mongodb_gke.git`
1. Fill in the required variables in `variables.tf` based on your needs
1. Authenticate to GCP:
   - `gcloud auth login`
   - `gcloud auth application-default login`
1. Initialize terraform:
   - `terraform init`
1. Check the terraform plan:
   - `terraform plan`
1. Apply the terraform code:
   - `terraform apply`

At the end of the `terraform apply`, you should now have a working GKE cluster and `kubectl` context configured.
So let's verify that in the next step!

## Verify the Deployed Chart

The example configures your `kubectl` context, so you can use `kubectl` and `helm` commands without further configuration. 

To see the created resources, run the following commands:

```
❯ kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
mongodb-5ccfc467f8-jglbc   1/1     Running   0          47m

❯ kubectl get service -n default
NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)     AGE
kubernetes   ClusterIP   10.4.16.1     <none>        443/TCP     53m
mongodb      ClusterIP   10.4.23.187   <none>        27017/TCP   47m

```
## MongoDB Password

```
> export MONGODB_ROOT_PASSWORD=$(kubectl get secret --namespace default mongodb -o jsonpath="{.data.mongodb-root-password}" | base64 --decode)
````

## To connect to Database
````
> kubectl run --namespace default my-release-mongodb-client --rm --tty -i --restart='Never' --image docker.io/bitnami/mongodb:4.2.7-debian-10-r0 --command -- mongo admin --host mongodb --authenticationDatabase admin -u root -p $MONGODB_ROOT_PASSWORD
`````

now you can execute mongodb commands
```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB

```

## Destroy the deployed resources

To destroy all resources created by the example, just run `terraform destroy`.
