# Create a new VPC and Public Subnet with Terraform

## Prerequisites
- GCP Project
- VM Instance with Terrform installed

## Create a Service Account
From Google Cloud console's main navigation, choose IAM & Admin > Service Accounts.

![image](https://user-images.githubusercontent.com/35857179/78898622-af27c380-7aa6-11ea-8244-b0e270b90ec5.png)

Click Create service account.

![image](https://user-images.githubusercontent.com/35857179/78898900-265d5780-7aa7-11ea-8e7a-978432789709.png)

Give your service account the name and click create

![image](https://user-images.githubusercontent.com/35857179/78898948-3c6b1800-7aa7-11ea-8f89-3b4e59d915ae.png)

In the roles dropdown, select Project > Owner.

![image](https://user-images.githubusercontent.com/35857179/78898993-53aa0580-7aa7-11ea-8931-1e030fd982a8.png)

Click Continue and then Done.

Copy the new service account's email address and paste it into a text file, as we'll need it later.
```
wingkwong@using-terraf-156-921c61.iam.gserviceaccount.com
```
At the top of the Google Cloud console, copy the project name that appears in the top navigation bar next to Google Cloud Platform. Paste it into the same text file — we'll need it later as well.

```
using-terraf-156-921c61
```

## Ensure Terraform is installed 

From Google Cloud navigation, choose Compute Engine > VM instances.

![image](https://user-images.githubusercontent.com/35857179/78902191-0714f900-7aac-11ea-8369-ce5b58b41d7b.png)

Click SSH next to your terraform instance. This will open a terminal session in a new browser tab or window, automatically logging us in to the instance.

Call Terraform to ensure it is installed
```
terraform
``` 

![image](https://user-images.githubusercontent.com/35857179/78899691-52c5a380-7aa8-11ea-8594-d697b5ff87a7.png)

## Create a Service Account Key within the instance

Allow the SDK to communicate with GCP:
```
gcloud auth login
```

Enter ``Y`` at the prompt.

![image](https://user-images.githubusercontent.com/35857179/78900034-d7182680-7aa8-11ea-98c3-b76dd7b36a4d.png)


Click on the link in the output.

![image](https://user-images.githubusercontent.com/35857179/78900066-e39c7f00-7aa8-11ea-9b53-5778b22d70cd.png)

Select the account.

Click Allow.

Copy the code provided.

Paste the code into the terminal.

![image](https://user-images.githubusercontent.com/35857179/78900096-f1520480-7aa8-11ea-8844-66c228e73ff1.png)


Create the service account key, replacing <SERVICE_ACCOUNT_EMAIL> with the one you copied earlier in the lab:
```
gcloud iam service-accounts keys create /downloads/instance.json --iam-account <SERVICE_ACCOUNT_EMAIL>
```

![image](https://user-images.githubusercontent.com/35857179/78900333-4857d980-7aa9-11ea-97f2-e9711e86da93.png)


## Create VPC, Subnet Configuration and Execute the Plan

Create a main.tf file:
```
vi main.tf
```

To avoid adding unnecessary spaces and hashes, type ``:set paste`` and then ``i`` to enter insert mode.

Paste the following configuration, replacing ``<PROJECT_NAME>`` with the project name you copied earlier:
```tf
provider "google" {
  version = "3.5.0"
  credentials = file("/downloads/instance.json")
  project = "<PROJECT_NAME>"
  region  = "us-central1"
  zone    = "us-central1-c"
}
resource "google_compute_network" "vpc_network" {
  name = "terraform-network"
}
resource "google_compute_subnetwork" "public-subnetwork" {
  name          = "terraform-subnetwork"
  ip_cidr_range = "10.2.0.0/16"
  region        = "us-central1"
  network       = google_compute_network.vpc_network.name
}
```
Save and exit the file by pressing Escape followed by ``:wq``.

Initialize the configuration file:
```
terraform init
```

![image](https://user-images.githubusercontent.com/35857179/78900897-1dba5080-7aaa-11ea-9aa1-bc24d9bbcd17.png)

Validate the configuration file:
```
terraform validate
```

![image](https://user-images.githubusercontent.com/35857179/78900940-2ad73f80-7aaa-11ea-8ceb-71f95a5330b4.png)

Create the execution plan:
```
terraform plan
```

![image](https://user-images.githubusercontent.com/35857179/78901033-4d695880-7aaa-11ea-9488-3ba477bed008.png)

Apply the changes:
```
terraform apply
```

Enter yes at the prompt.

![image](https://user-images.githubusercontent.com/35857179/78901073-6114bf00-7aaa-11ea-975c-79ca7f3f7cdb.png)

It will take a few minutes to complete.

![image](https://user-images.githubusercontent.com/35857179/78901297-b355e000-7aaa-11ea-9de8-306793465127.png)

In the Google Cloud console, navigate to VPC network > VPC networks. 

![image](https://user-images.githubusercontent.com/35857179/78901529-062f9780-7aab-11ea-95db-9118da6cb2be.png)

We should see our ``terraform-network`` and ``terraform-subnetwork``.

![image](https://user-images.githubusercontent.com/35857179/78901764-632b4d80-7aab-11ea-82e6-e4773a941b9f.png)

## Clean up
```
terraform destroy
```
Enter yes at the prompt.

![image](https://user-images.githubusercontent.com/35857179/78902566-8a364f00-7aac-11ea-88ce-dc53baf7be18.png)

It will take a few minutes to complete.

![image](https://user-images.githubusercontent.com/35857179/78902728-c1a4fb80-7aac-11ea-9c36-0feba4cc72ae.png)
