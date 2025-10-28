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
- then you have to Go to SSH
- Go to Auth
- Choose key pair file you made while creating instance.
```bash
login as username: ubuntu
```
![EC2 Instance](![first image](![Image 1 login as ubuntu](https://github.com/user-attachments/assets/e2803c4d-d826-4ada-8d7b-69574d5552ed)
)
)

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

```
![EC2 Instance](![first image](![apahe install](https://github.com/user-attachments/assets/f0b33ae2-e482-4a7e-bb9d-39f5591a8dc7)
)

###HTml code is inserted
- ![this is my webpage](![entering html codde for the file](https://github.com/user-attachments/assets/e110993c-b8fb-47d1-87d9-509f3ff4eaf7))

### 4 This is my web page
![EC2 Instance](![first image](![this is my webpage](https://github.com/user-attachments/assets/01b91997-eab6-4cd2-9f3e-010ee9196bb1)
)
