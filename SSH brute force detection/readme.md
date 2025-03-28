
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

## Network Reconnaissance 
Now that we have SSH up and running, it's time to Put on that black hat. Let's assume that the attacker found the target IP address from shodan search or a phishing attack. so the next step is to gather information about the target system.

### Scanning with Nmap
Like I mentioned earlier I'll be using Nmap a powerful reconnaissance tool.

- Since we already know the targets IP will run a scan to discover open ports and other information about the target 
  ``` bash
  nmap -a x.x.x.x

- Now the attacker has a list of all the open ports on the target and would usually try port 22 since it's the default for SSH but to make things a tad bit more interesting I put SSH on a custom port.
  ``` bash
   nmap -p- x.x.x.x

- The attacker now has a list of the open ports. and would search for OpenSSH with
   ``` bash
    nmap -sV -p <open-port> <target-ip>

- The attacker finally has the SSH port and can begin brute-forcing
 
## SSH Brute-Forcing
Now that SSH is up and running, it's time to simulate an attack.

- Step 1: Running the Brute-force Attack
 Using Hydra, we can attempt a brute-force attack on SSH with
  ``` bash
   hydra -l <username> -P /usr/share/wordlists/rockyou.txt ssh://<target-ip>

- Step 2: Monitor the login attempts
  While hydra is running the attack, we can head over to the target and see all the failed logins
  ``` bash
  sudo tail -f /var/log/auth.log | grep "Failed password"

- Unsuccessful brute-force
  The brute-force attack failed, showing:
   ``` bash
   1 of 1 target completed, 0 valid passwords found.
This outcome was expected since SSH by default has an account lockout policy after every 6 attempts. 

However, the main goal was to generate "noise" and trigger security alerts for incident response.

## Incident Response
Since we've generated some "noise" with hydra, it's time to investigate and respond to the incident
 
### Detection 
The first step in incident response is to detect any suspicious activity related to SSH login attempts.

### Check for failed login attempts
  On the target machine, use the following command to check authentication logs for failed SSH login attempts

    
     sudo grep "Failed password" /var/log/auth.log

Head over to the Wazuh Dashboard, under Threat Detection, where we can see several failed SSH login attempts. 

Now It's time to respond to the attacks.
![image](https://github.com/user-attachments/assets/5cca435d-1798-444b-bfcb-2d35d9b22934)

### Response
- Identify the attacker IP address: We can expand on any of the failed SSH login logs and see  the attacker's IP address
![image](https://github.com/user-attachments/assets/2c5b3473-1a33-4634-9b06-795395954db0)

- Block the attackers IP address: Now We have the attacker's IP address we can set a firewall rule to block that IP address using UFW(Uncomplicated Firwall):

      sudo ufw deny from <attacker-ip>
![image](https://github.com/user-attachments/assets/77b77241-dafa-4e8a-a70a-b6a211556e14)

- Install Fail2ban to prevent brute-force attacks from other Threat actors: Fail2ban is a security tool that prevents several failed login attempts

        sudo apt install fail2ban -y
        sudo systemctl enable --now fail2ban

With Fail2Ban installed, this marks the completion of the SSH Brute-Force Detection subdomain.


