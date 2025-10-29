# Secure Raspberry Pi

## Project Goal
The Secure Raspberry Pi project aimed to setup a Raspberry Pi 5 and to harden it, in the process we will learn Linux adn some Windows fundamentals, system hardening, authentication, and service management. Next we can use the Raspberry Pi to do network discovery and scanning with nmap, arp-scan, and netdiscover to map a home network. We can automate these tasks by running a simple script. 

### Skills Learned

- System hardening, authentication, service management
- Reconnaissance, IP ranges, automation basics.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- ufw & fail2ban
- nmap, arp-scan and netdiscover (In Projess)

## Steps

## Raspberry Pi Initial Setup

1. Purchase a Raspberry Pi

Today I received my Raspberry Pi 5 Starter Kit in the mail. The starter kit was purchased from Vilros and consists of the following:
 - two micro-HDMI to standard HDMI cable
 -  a class 10 128 GB Micro SD Memory Card
 -   Micro SD to USB Adapter/Reader
 -   Pi 5 Multi-Use Case w/ built-in Passive & Active cooling
 -   8GB RAM Raspberry Pi 5
 -   USB -C Raspberry Pi 5 Compaticle Power Supply
 -   Mini to Standard Camera Module Adapter Cable
 -   Raspberry Pi Case
    
![IMG_0743](https://github.com/user-attachments/assets/7e5b14c8-d740-4092-aab7-d7ca5144ffec)
*Ref 1: Vilros Raspberry Pi Starter Kit*

My plan is to set up this Raspberry Pi as a headless computer, accessible only on the network. The kit I purchased already had Raspberry Pi OS installed on the MicroSD card. So initially, I did hook up a montior, keyboard, and mouse to the Raspberry Pi. Before inserting the power cord, I had all peripherals connected along with the microSD card. NOTE: A Raspberry Pi will automatically boot from a microSD card when the slot contains a card. 

2. Fire up the Raspberry Pi
3. There is a wizard and I followed the steps, creating a username/ password and set up WiFi user/password.
4. Next, you need to enable SSH. Go to preferences, naviage to Interfaces Tab, and selected Enabled next to SSH.
5. Restart
6. Now without using peripherals, I remoted into my pi using SSH and a password.
7. ssh username@<IP_ADDRESS>

## SSH Key-Based Authentication
For a more secure setup, I set up SSH keys using public-key cryptography, making them virually impassible to brute-force. Key-based authenication is less vunerable to guessing or dictionary attacks. You can generate different types of asymmetric algorithms, RSA, ECC, DSA just to name a few. Your choice. You need to make keys on the computer that you use to remote into the Raspberry Pi. I have a Windows computer. 

**Manually configure an SSH key on a Windows computer**
1.  C:\Users\USERNAME  > dir
2.  C:\Users\USERNAME  > cd .ssh
3.  C:\Users\USERNAME\.ssh > dir
    - Check for existing SSH public keys, I do not have any ssh keys
4.  C:\Users\USERNAME  > ssh-keygen
    - generates a new SSH key pair, use -t argument to specify the type of key and -C argument for a comment
5.  C:\Users\USERNAME > ssh-keygen -t rsa -C "Dawn's Pi Key"
    - When asked to enter file in which to save the key (FILE PATH SHOWN HERE): press Enter
    - Enter passphrase (empty for no passphrase): press Enter
6. C:\Users\USERNAME\.ssh > dir
   - Run the above command to check the contents of the .ssh directory
   - You should see the files id_rsa and id_rsa.pub
   - The id_rsa file contains your private key. Keep this secure on the computer you use to remotely connect to the Raspberry Pi.
   - The id_rsa.pub file contains your public key. You will share this key with your Raspberry Pi. When you connect with the Raspberry Pi remotely, it will use this key to verify your identity.
 
  **Add the SSH key to your list of SSH identities**
   1. Open a Powershell Window as Administrator
   2. PS C:\WINDOWS\system32 > Start-Service ssh-agent
   3. PS C:\WINDOWS\system32> ssh-add $env:USERPROFILE\.ssh\id_rsa
   4. PS C:\WINDOWS\system32> ssh-add
      - Verify the key is added
      - You should see a hash of your key here along with the comment "Dawn's Pi Key" 

**Copy a public key from Windows to your Raspberry Pi**
1. PS C:\WINDOWS\system32> type $env:USERPROFILE\.ssh\id_rsa.pub | ssh pi@<IP_ADDRESS> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
    - You can now log on to your Raspberry Pi without the need for the password and using SSH Authorization.

## Disable Password SSH Login
Diabling password-based SSH login prevents Brute-Force Attacks, Enforeces Key-Based Authentication & Reduces the attack surface. This also improves audit and control as keys are typically tied to a specific user.
1. sudo vi /etc/ssh/ssh_config
   - This edits the SSH config file
2. Find and update the below listed lines:
   - PasswordAuthentication no
   - PermitRootLogin no
 3. sudo systemctl restart ssh
    - Restart SSH
    - This ensures only key-based logins are allowed and root login is blocked - a common attack vector.

  ** Change SSH Port (Optional but useful)
  1. sudo vi /etc/ssh/ssh_config
  2. Find and update the below listed line (you can change it to any port you would like)
     - Port 22  (Standard SSH port)
     - Port 2222 (Updated SSH Port)
  3. sudo systemctl restart ssh
     - Restart SSH

  ## Install and Configure UFW (Uncomplicated Firewall)
  Installing UFW (Uncomplicated Firewall) is another way to secure your Raspberry Pi It simplfies firewall management by being user-friendly, you can allow/ deny traffic with simple commands, protects against unauthorized access (UFW blocks all incoming connections by default, except those you explicitly allow, which is the principle of least priviledge), it also works seamlessly with Fail2Ban. UFW sets static rules, Fail2Ban adds dynamic bans. Together they create a layered defense system (Defense in Depth). 
  1. sudo apt update && sudo install ufw
     - Say "Y" when asked to continue
  2. sudo ufw allow 2222/tcp
     - Allows SSH on the chosen port
     - Rule should update
  3. sudo ufw enable
     - Enable UFW
  4. sudo ufw status verbose
     - Check status of the UFW firewall
     - UFW gives you control over which services are exposed.

 ## Install and Configure Fail2Ban 
 Security Tool for Linux systems to help protect against brute-force attacks, useful for SSH, FTP and webservers
  1. sudo apt install fail2ban
  2. sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
     - Create a local jail config. Preserves custom settings: jail.config is default configuration file
     - jail.local overrides jail.conf: Fail2Ban reads both files, but jail.local takes priority
     - Supports version upgrades: When Fail2Ban updates default rules, local.jail remains untouched - so hardened setup stays intact
  3. sudo nano /etc/fail2ban/jail.local
  4. Edit jail.local file to
      - Enabled = true [Actives this jail]
      - port = 2222 [Matches custom SSH port]
      - backend = systemd [reads logs using systemd journal - journalctl]
      - maxretry = 3 [Blocks IPs after 3 failed attempts]
      - bantime = 3600 [Bans for 1 hour, 3600 seconds]
  5. Save the file
  6. sudo systemctl start fail2ban
       - Start Fail2Ban
  7. sudo systemctl enable fail2ban
       - Enable it to start on boot
  8. sudo systemctl status fail2ban
       - Check the status
      
   
     
   
   

