# Apache-Install

# Hosting a Website on AWS EC2 with  

### 1. Launch EC2 Instance  
- Go to AWS Console, then EC2, then Launch Instance  
- Choose **Ubuntu**  
- Create and Use key pair
- Use Putty
- Allow **HTTP (80)**, **HTTPS (443)**, and **SSH (22)**  
- Launch instance
## Launched EC2 Instance
![EC2 Instance](![first image](https://github.com/user-attachments/assets/de344547-2ec4-4590-b897-c6870bf89837)
)


### 2. Connect to EC2 (using PuTTY/SSH)  
- Download Putty.exe file and install it in your device.
- Go to connection.
- Go to SSH
- Go to Auth
- Choose key pair file you made while creating instance.
```bash
login as username: ubuntu
```

### 3. Install Apache  
```bash
sudo apt install apache2
```

Check in browser → `http://yourpublicip`
- An Apache page will be shown to your public ip.

```bash
cd /var/www/html
sudo rm index.html
sudo vi index.html
```
- Press I to insert your own html code.
- Write your own code.
- After writing all code , Press Ctrl+C and then write ":wq" and Press Enter.

### 5. Map Domain with Route 53  
- Register your own domain name.
- Go to Route 53 on AWS, Click on Hosted Zones.
## Route 53 Hosted Zone
![Route 53 Hosted Zone](cloudimages/route53hostedzone.png)
- You will get four NS name server.
## NS Name Servers
  ![NS Name servers](cloudimages/nsserver.png)
- Then update these name server in your domain.
## Domain Name adding Name server
![Nameservereditedondomain](cloudimages/registereddomain.png)
- Create **A record**
- Set your domain, Enter your allotted Public IP and then Map it.
## Website before SSL certfication (Showing Not Secure)
![Webiste before SSL certification](cloudimages/websitebeforesslcertification.png)

### 6. Enable SSL (HTTPS) using Certbot  
```bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache
```
- Then it will ask an email.
- Then it will ask your domain name , So just write it.
- Then SSL certification will deployed to your domain name.
  Check in browser → `https://yourpublicip`

## Website after SSL certfication using https (Showing Connection is Secure)
![Webiste ater SSL certification](cloudimages/securedsite.png)
