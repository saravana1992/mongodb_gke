Installing necessary tools
In addition to terraform, this example relies on gcloud and kubectl and helm tools to manage the cluster.

This means that your system needs to be configured to be able to find terraform, gcloud, kubectl and helm client utilities on the system PATH. Here are the installation guides for each tool:

gcloud
kubectl
terraform
helm (Minimum version v3.0)
Make sure the binaries are discoverable in your PATH variable. See this Stack Overflow post for instructions on setting up your PATH on Unix, and this post for instructions on Windows.

Apply the Terraform Code
Now that all the prerequisite tools are installed, we are ready to deploy the GKE cluster!

If you haven't already, clone this repo:
git clone https://github.com/gruntwork-io/terraform-google-gke.git
Make sure you are in the gke-basic-helm example folder:
cd examples/gke-basic-helm
Fill in the required variables in variables.tf based on your needs
Authenticate to GCP:
gcloud auth login
gcloud auth application-default login
Initialize terraform:
terraform init
Check the terraform plan:
terraform plan
Apply the terraform code:
terraform apply
At the end of the terraform apply, you should now have a working GKE cluster and kubectl context configured. So let's verify that in the next step!

Verify the Deployed Chart
The example configures your kubectl context, so you can use kubectl and helm commands without further configuration.

To see the created resources, run the following commands:

❯ kubectl get deployments -n default
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           92m

❯ kubectl get service -n default
NAME         TYPE           CLUSTER-IP   EXTERNAL-IP     PORT(S)                      AGE
kubernetes   ClusterIP      10.2.0.1     <none>          443/TCP                      113m
nginx        LoadBalancer   10.2.5.84    34.77.188.186   80:31588/TCP,443:31332/TCP   99m

❯ kubectl get pods -n default
NAME                     READY   STATUS    RESTARTS   AGE
nginx-57454964b8-l4w9w   1/1     Running   0          99m
If you wish to access the deployed service, you use the kubectl port-forward command to forward a local port to the deployed service:

❯ kubectl port-forward deployment/nginx 8080:8080 -n default
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
You can now access the deployed service by opening your web browser to http://localhost:8080.

Destroy the deployed resources
To destroy all resources created by the example, just run terraform destroy
