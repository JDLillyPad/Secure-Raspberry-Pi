# Secure Raspberry Pi

## Objective

The Secure Raspberry Pi project aimed to setup a Raspberry Pi 5 and to harden it, in the process we will learn Linux adn some Windows fundamentals, system hardening, authentication, and service management. Next we can use the Raspberry Pi to do network discovery and scanning with nmap, arp-scan, and netdiscover to map a home network. We can automate these tasks by running a simple script. 

### Skills Learned

- System hardening, authentication, service management
- Reconnaissance, IP ranges, automation basics.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- ufw & fail2ban
- nmap, arp-scan and netdiscover

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

## SSH Key-Based Authentication (In progress)
For a more secure setup, I set up SSH keys using public-key cryptography, making them virually impassible to brute-force. Key-based authenication is less vunerable to guessing or dictionary attacks. You can generate different types of asymmetric algorithms, RSA, ECC, DSA just to name a few. Your choice. You need to make keys on the computer that you use to remote into the Raspberry Pi. I have a Windows computer. 

**Manually configure an SSH key on a Windows computer**
1.  C:\Users\USERNAME  > dir
2.  C:\Users\USERNAME  > cd .ssh
3.  C:\Users\USERNAME\.ssh > dir  [Check for existing SSH public keys, I do not have any ssh keys]
4.   C:\Users\USERNAME  > ssh-keygen [generates a new SSH key pair, use -t argument to specify the type of key and -C argument for a comment]
    C:\Users\USERNAME > ssh-keygen -t rsa -C "Dawn's Pi Key"
   When asked to enter file in which to save the key (FILE PATH SHOWN HERE): press Enter
   Enter passphrase (empty for no passphrase): press Enter
5. Run the following command to check the contents of the .ssh directory
   C:\Users\USERNAME\.ssh > dir
   You should see the files id_rsa and id_rsa.pub
   The id_rsa file contains your private key. Keep this secure on the computer you use to remotely connect to the Raspberry Pi.
   The id_rsa.pub file contains your public key. You will share this key with your Raspberry Pi. When you connect with the Raspberry Pi remotely, it will use this key to verify your identity.
  **Add the SSH key to your list of SSH identities**
   Open a Powershell Window as Administrator
   PS C:\WINDOWS\system32 > Start-Service ssh-agent
   PS C:\WINDOWS\system32> ssh-add $env:USERPROFILE\.ssh\id_rsa
   Verify it is added
   PS C:\WINDOWS\system32> ssh-add -l
    You should see a hash of your key here along with the comment "Dawn's Pi Key" 
**Copy a public key from Windows to your Raspberry Pi**
PS C:\WINDOWS\system32> type $env:USERPROFILE\.ssh\id_rsa.pub | ssh pi@<IP_ADDRESS> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
You can now log on to your Raspberry Pi without the need for the password and using SSH Authorization. 
