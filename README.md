# Secure Raspberry Pi

## Objective

The Secure Raspberry Pi project aimed to setup a Raspberry Pi 5 and to harden it, in the process we will learn Linux fundamentals, system hardening, authentication, and service management. Next we can use the Raspberry Pi to do network discovery and scanning with nmap, arp-scan, and netdiscover to map a home network. We can automate these tasks by running a simple script. 

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

My plan is to set up this Raspberry Pi as a headless computer, accessible only on the network. The kit I purchased already had Raspberry Pi OS installed on the MicroSD card. So initially, I did hook up a montior, keyboard, and mouse to the Raspberry Pi. Before inserting the power cord, I had all peripheral connected along with the microSD card. NOTE: A Raspberry Pi will automatically boot from a microSD card when the slot contains a card. 

2. Fire up the Raspberry Pi
3. There is a wizard and I followed the steps, creating a username/ password and set up WiFi user/password.
4. Next, you need to enable SSH. Go to preferences, naviage to Interfaces Tab, and selected Enabled next to SSH.
5. Restart
6. Now without using peripherals, I remoted into my pi using SSH and a password.
7. ssh username@<IP_ADDRESS>

## SSH Key-Based Authentication (In progress)
For a more secure setup, I set up SSH keys using public-key cryptography, making them virually impassible to brute-force. Key-based authenication is less vunerable to guessing or dictionary attacks. 

WORK IN PROGRESS...
drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*
