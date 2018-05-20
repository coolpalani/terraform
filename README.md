# HAProxy - DigitalOcean - Terraform

Deploy your Two tier Web Server with HAProxy loadbalancer on DigitalOcean using Terraform.

## Requirements

* [DigitalOcean](https://www.digitalocean.com/) account
* DigitalOcean Token [In DO's settings/tokens/new](https://cloud.digitalocean.com/settings/tokens/new)
* [Terraform](https://www.terraform.io/)

## Prepare Terraform Server

Login into cloudserver

```
1. Install unzip
	sudo apt-get install unzip -y

2. Download latest version of the terraform
	wget https://releases.hashicorp.com/terraform/0.11.7/terraform_0.11.7_linux_amd64.zip

	unzip terraform_0.11.7_linux_amd64.zip
	sudo mv terraform /usr/local/bin/
	terraform --version
```

## Generate private / public keys

```
ssh-keygen -t rsa -b 4096
```

The system will prompt you for a file path to save the key, we will go with `~/.ssh/id_rsa` in this tutorial.

## Add your public key in the DigitalOcean control panel

[Do it here](https://cloud.digitalocean.com/settings/security). Name it and paste the public key just below `Add SSH Key`.

## Add this key to your SSH agent

```bash
eval `ssh-agent -s`
ssh-add ~/.ssh/id_rsa
```

## Invoke Terraform

We put our DigitalOcean token in the file `./secrets/DO_TOKEN` (this directory is mentioned in `.gitignore`, of course, so we don't leak it)

Then we setup the environment variables (step into `this repository` root).

```bash
export TF_VAR_do_token=$(cat ./secrets/DO_TOKEN)
export TF_VAR_pub_key="~/.ssh/id_rsa.pub"
export TF_VAR_pvt_key="~/.ssh/id_rsa"
export TF_VAR_ssh_fingerprint=$(ssh-keygen -E MD5 -lf ~/.ssh/id_rsa.pub | awk '{print $2}' | sed 's/MD5://g')
```

Let's use this ```setup-ssh-env.sh``` script

```
source setup-ssh-env.sh
```

The default region is `nyc1`. You can find a list of available regions from [DigitalOcean](https://developers.digitalocean.com/documentation/v2/#list-all-regions).

Optionally, you can customize the datacenter *region* via:
```bash
export TF_VAR_do_region=fra1
```

After setup, call `terraform plan` and `terraform apply`

```bash
terraform init
terraform plan
terraform apply
```

That should do! Two Webserver is ready with HAProxy LoadBalancer ready

### Termination

```bash
terraform plan -destroy
```
After sanity check

```bash
terraform destroy
```
