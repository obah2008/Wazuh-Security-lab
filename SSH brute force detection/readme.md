
# What exactly is SSH?
SSH or Secure shell is a network protocol that enables a secure remote access to a computer.

## Objectives
The objectives of this project's subdomain are:

- Set up Ubuntu as a Wazuh-agent
- Use Kali linux and hydra to execute a brute force attack on the agents open SSH port
- Analyze alerts on the Wazuh dashboard to practice intrusion detection and gain insights on the Threat actor
- Set up an automated threat response

## What is a Brute-force attack??
A brute-force attack involves going through every possible username and password combination to find the correct login details. The time it takes to attain the correct credentials obviously depend on the password strength. A weak password could be gotten in a matter of seconds, while a complex one might take millions of years.

## Lab Setup
For this lab, I'll be using Ubuntu as the endpoint instead of Metasploitable3. Initially, I planned to use Metasploitable3 as the target machine, but due difficulties during the setup, I decided to switch to Ubuntu.

I'll also be using Kali Linux, a linux distro designed for security experts, as the threat actor. The following tools will be used:

- Nmap – for port scanning

- Hydra – for brute-force attacks

Although I’m not using Metasploitable3 for this project, I still plan to work with it in future security labs.

### Setting up the Ubuntu Endpoint
I'm not going to go deep into this since I've already covered it in the [Lab Setup](). If you need a detailed guide on how to do this refer to that section.

### Network configuration
Since I need both the attacker(Kali) and target(Ubuntu) to be able to communicate with each other in an isolated environment, I'll be using the host only Network configuration.

### Setting up SSH
SSH is not setup by default on the Ubuntu endpoint, so I'll have to do it manually.

- The first step is to install the OpenSSH server with the command below
  ``` bash
   sudo apt install openssh-server 

- Next start the service with:
  ``` bash
  sudo service ssh start

- Allow SSH through firewall: Ensure UFW isn't stopping any SSH traffic
  ``` bash
   sudo ufw allow ssh
   sudo ufw reload

#### Allow SSH to listen on all ports
Usually the SSH only listens on port 22 and is only available to devices on the same network as the client, But if we allow SSH to listen on all ports, anyone can gain access. Since I'm doing this in a controlled environment, I'll configure SSH to listen on all ports.

- Modify the SSH configuration file: using the nano command
   ``` bash
  sudo nano /etc/ssh/sshd_config

- Configure SSH to listen on all ports: On the configuration file change the "#port 22" to "port 0"

- Restart the SSH service
  ``` bash
  sudo systemctl restart ssh

## Network reconnaisance 
