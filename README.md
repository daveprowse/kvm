# Lab: Building a mkdocs webserver on Digital Ocean using Terraform and cloud-config.

In this lab you create an Ubuntu-based Apache webserver on Digital Ocean (using cloud config), and install and build mkdocs for immediate use.
(optional) Then you scale the server to 2 CPU/2 GB RAM.
You will need an account with DigitalOcean and will need Terraform installed to your computer. This script will build an Ubuntu 22.04 Server.
Step-by-step procedure below.

!!! warning
        ***Digital Ocean will charge you for usage! Destroy all resources when they are no longer needed!***

!!!  Lab files are available at: https://github .........
Digital Ocean Terraform reference: https://docs.digitalocean.com/reference/terraform/
Video demonstration: 

---

## Procedure

### Analyze the main.tf file 
Make changes as necessary for users, zone, and droplet.

### Login and create an API token at Digital Ocean: 
https://cloud.digitalocean.com/account/api/tokens
  - Copy the access token to the terminal:
  `export DIGITALOCEAN_ACCESS_TOKEN=` and add the actual token after the = sign. 
  - Keep Read and Write on. 
  - Give it a 30 day expiration. 

3. Analyze the apache-mkdocs.yaml file. This is the script that will install Apache on the Ubuntu server, and install mkdocs. Be sure that you understand everything that is occuring in the script.

4. Create an SSH key pair in the terminal
I recommend ed25519 or RSA. Use your email address. 
Example:
`ssh-keygen -t ed25519 -C "email@your_domain.com" -f ./apache-mkdocs`
Recommend using a passphrase for the key pair.
That will create a private key (apache-mkdocs) and a public key (apache-mkdocs.pub). *Show the private key to no one!*

5. Copy and paste the public key to the apache-mkdocs.yaml file in the "ssh_authorized_keys" section. 
For example:
```
ssh_authorized_keys:
  - ssh-rsa AAAA*#&^$*#&^$*&#^$..........
``` 

6. Analyze the outputs.tf file. This will display the IP address of your new droplet once it is created.

7. Terraform your server!
In the terminal, type `terraform init`.
Then type `terraform apply`
Type "yes" to accept the new configuration.
That should build the Ubuntu server on Digital Ocean. Give it a couple of minutes to complete the configuration in the cloud-config script. You should be able to see the droplet in your DigitalOcean dashboard.

*Optional*: ping the server's IP and use nmap to scan it and see the open ports:
`ping <ip_address>`
`nmap <ip_address>`

8. Access the web server.
Write down the IP address that is displayed when the `terraform apply` is complete. 
Access the IP address from a browser. http://<ip_address>. Replace <ip_address> with your IP address.
If you see the Apache home page, then the script has not completed yet. Remember to wait a few minutes for the YAML script to complete. 
You should see a default, beautiful mkdocs web server.

9. SSH into the server. Use whatever user account name you selected and the SSH key you created. Example:
`ssh spiderman@<ip_address> -i apache-mkdocs`
- View the version of Ubuntu:
`cat /etc/os-release`
- View the mkdocs directory
`ls mkdocs/mkdocs-project`
You will see the markdown "docs" directory and the actual "site" directory. That is where the Apache site comes from. We didn't actually have to do this, as you would normally run and build mkdocs locally and simply upload it to the web server. But this way, we can see a finished server.
- View the html directory:
`ls /var/www/html`
This directory contains the mkdocs website. This is where you would subsequently upload your website.
- Exit out of the SSH session.

10. Scale the server
Change the size of the resource in main.tf from "s-1vcpu-1gb" to "s-2vcpu-2gb" so that the server will use 2 CPUs and 2 GB of RAM. 
Then, run `terraform apply` to terraform the change.
SSH back into the server and verify the change.
Type `nproc` to see that there are 2 CPUs.
Type `free` to see that there are now 2 GB of RAM. 
Or, use the top program to see both. 
Exit from the SSH session when you are done.

11. Destroy the droplet
**!!! IMPORTANT !!!** Be sure to destroy the droplet when you are done. Until you do Digital Ocean will continue to bill you!
Type `terraform destroy` and "yes" to accept the destruction of the droplet.

Notes: 
- This lab should only cost you pennies (as long as you destroy the resource.)
- This lab does not incorporate TLS certificates, or domain names. That is another story...
- You can install mkdocs locally by following the instructions here: https://www.mkdocs.org/user-guide/installation/

