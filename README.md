# IaC on DigitalOcean — Terraform Droplet
 
This project is a solution to the [IaC on DigitalOcean](https://roadmap.sh/projects/iac-digitalocean) challenge. It uses Terraform to provision a DigitalOcean Droplet with a public IP address and SSH access.
 
## What this does
 
- Provisions a **DigitalOcean Droplet** (Ubuntu 22.04)
- Uploads a local SSH public key to DigitalOcean and attaches it to the Droplet
- Outputs the Droplet's public IP address after creation
- Enables SSH login to the Droplet using the corresponding private key
## Project structure
 
```
.
├── main.tf           # Provider config + Droplet & SSH key resources
├── variables.tf       # Input variable definitions
├── outputs.tf          # Output values (Droplet IP)
├── terraform.tfvars    # Actual variable values (NOT committed — see .gitignore)
└── .gitignore
```
 
## Prerequisites
 
- A [DigitalOcean](https://www.digitalocean.com) account and an API token (Read + Write scope)
- [Terraform](https://developer.hashicorp.com/terraform/downloads) installed locally
- An SSH key pair on your machine (`~/.ssh/id_rsa` / `~/.ssh/id_rsa.pub` or similar)
## Setup & usage
 
1. Clone this repository:
```bash
   git clone <this-repo-url>
   cd <repo-folder>
```
 
2. Create a `terraform.tfvars` file with your DigitalOcean API token:
```hcl
   do_token         = "your_digitalocean_api_token"
   ssh_pub_key_path = "~/.ssh/id_rsa.pub"
```
 
3. Initialize Terraform (downloads the DigitalOcean provider):
```bash
   terraform init
```
 
4. Preview the resources that will be created:
```bash
   terraform plan
```
 
5. Create the Droplet:
```bash
   terraform apply
```
   Type `yes` when prompted.
 
6. Once complete, Terraform will print the Droplet's public IP. You can also retrieve it any time with:
```bash
   terraform output droplet_ip
```
 
7. SSH into the Droplet:
```bash
   ssh root@$(terraform output -raw droplet_ip)
```
 
## Tear down
 
To avoid ongoing charges, destroy the Droplet once you're done:
```bash
terraform destroy
```
Type `yes` when prompted.
 
## Notes
 
- `terraform.tfvars` contains the API token and is excluded from version control via `.gitignore`. Never commit real credentials.
- Default region is `blr1` (Bangalore) and default size is `s-1vcpu-1gb` — both configurable via variables.

https://roadmap.sh/projects/iac-digitalocean 
