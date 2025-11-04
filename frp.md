# Setting up a website with FRP(Fast Reverse Proxy) client and server
FRP is a fast reverse proxy to help you expose a local server behind a NAT or firewall to the internet. It is written in Go and supports multiple protocols, including TCP, UDP, HTTP, and HTTPS.

## We'll need to accomplish:

- Set up an AWS EC2 instance (Ubuntu) to act as the FRP server
- Create a simple website locally (FRP client side)
- Configure FRP server on EC2
- Configure FRP client on your local machine
- Expose your local website through the FRP server

## Architecture

<b>FRP Server:</b> AWS EC2 (Ubuntu) - publicly accessible

<b>FRP Client:</b> Your local machine - running a simple website

<b>Flow:</b> Internet → EC2 (FRP Server) → Your Local Machine (FRP Client) → Local Website
<hr>
<br>

# 1. Set up an AWS EC2 instance (Ubuntu) to act as the FRP server 
## Step 1   - Launch an EC2 instance
- Log into AWS Console
- Navigate to EC2 Dashboard
- Click "Launch Instance"
- Configure:
    - Name: frp-server
    - AMI: Ubuntu 
    - Instance Type: t3.micro 
    - Key Pair: Create new or use existing
    - Security Group: 
    Create with the following rules (by clicking on the edit button next to network settings):

        - SSH (22) - Your IP
        - Custom TCP (7000) - 0.0.0.0/0 (FRP bind port)
        - Custom TCP (8080) - 0.0.0.0/0 (Web traffic port)
        - HTTP (80) - 0.0.0.0/0 (optional)
<img width="1919" height="1034" alt="FRP IMAGE 1" src="https://github.com/user-attachments/assets/bf41de32-23cf-4ed0-a670-baacc68f7bd2" />

## Step 2 - Connect to your EC2 instance
use Putty
![install k3s](https://github.com/user-attachments/assets/c9981a67-e4ae-426c-9d50-e361153f7dbd)

## Step 3 - Install FRP server
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Download FRP (check for latest version at https://github.com/fatedier/frp/releases)
wget https://github.com/fatedier/frp/releases/download/v0.52.3/frp_0.52.3_linux_amd64.tar.gz

# Extract
tar -xzf frp_0.52.3_linux_amd64.tar.gz
cd frp_0.52.3_linux_amd64
```
<img width="1571" height="348" alt="FRP IMAGE 2" src="https://github.com/user-attachments/assets/3f559953-1b10-4bbc-8c72-7e1a0cd4ec3f" />

## Step 4 - Configure FRP server
Create a server configuration file:
```bash 
vim frps.toml
```
Add the following content:
```toml
# FRP Server Configuration
bindAddr = "0.0.0.0"
bindPort = 7000

# Dashboard (optional - for monitoring)
webServer.addr = "0.0.0.0"
webServer.port = 7500
webServer.user = "admin"
webServer.password = "admin123"

# Authentication token (CHANGE THIS!)
auth.token = "eth4"

```
![Checking if instlled or not](https://github.com/user-attachments/assets/0091ff05-fd72-4e1f-b9fb-fbeeaa718299)

## Step5 - Run the FRP server:
```bash
./frps -c frps.toml
```
## Step 6 - (Optional) Set up FRP server as a systemd service
```bash
sudo vim /etc/systemd/system/frps.service
```
ADD:
```ini
[Unit]
Description=FRP Server
After=network.target

[Service]
Type=simple
User=ubuntu
Restart=on-failure
RestartSec=5s
ExecStart=/home/ubuntu/frp_0.52.3_linux_amd64/frps -c /home/ubuntu/frp_0.52.3_linux_amd64/frps.toml

[Install]
WantedBy=multi-user.target
```
Enable and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable frps
sudo systemctl start frps
sudo systemctl status frps
```


FRP server is now set up and running.

# 2. Create a simple website locally (FRP client side)
I'll be doing this on my virtual machine (Ubuntu) but you can do it on your local machine as well.

## Step 1 - Install FRP on your Virtual box VM / Local machine
```bash
wget https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_amd64.tar.gz
tar -zxvf frp_0.61.1_linux_amd64.tar.gz
sudo mv frp_0.61.1_linux_amd64 /usr/local/frp
cd /usr/local/frp
```
<img width="1010" height="429" alt="systemrunning sd" src="https://github.com/user-attachments/assets/e6210dec-00d2-44ab-8710-81dea7a13320" />

## Step 2 - Setting up a web server 
```bash
sudo apt update 
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```
## Step 3 - Create a simple Website
```bash
cd /var/www/html
sudo rm index.html
sudo vim index.html
```
Add the following content:
```html
<html>HLO WElcome</html>
```
Press `esc` and `:wq` to save and exit.
## Step 4 - Configure FRP client
Create a client configuration file `frpc.toml`:
```bash
sudo rm usr/local/frp/frpc.toml
sudo vim usr/local/frp/frpc.toml
```
Add the following content:
```toml

serverAddr = "3.16.149.216" # Replace with your EC2 public IP
serverPort = 7000
token = "eth4" # Must match the server token

[[proxies]]
name = "web"
type = "http" 
localPort = 80
remotePort = 8080
```
Press `esc` and `:wq` to save and exit.
## Step 5 - Run the FRP client
```bash
sudo /usr/local/frp/frpc -c /usr/local/frp/frpc.toml
``` 
<img width="1000" height="612" alt="image" src="https://github.com/user-attachments/assets/20ea6d08-39f7-4e04-afed-67cb35dd6a25" />

# Access your website
Open a web browser and navigate to `http://<EC2-Public-IP>:8080`. You should see your simple website.
![](resources/Screenshot%202025-10-03%20162155.png)
