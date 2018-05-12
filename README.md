```
Login into cloudserver
1. Install unzip
	sudo apt-get install unzip -y

2. Download latest version of the terraform
	wget https://releases.hashicorp.com/terraform/0.11.7/terraform_0.11.7_linux_amd64.zip

	unzip terraform_0.11.7_linux_amd64.zip
	sudo mv terraform /usr/local/bin/
	terraform --version
```