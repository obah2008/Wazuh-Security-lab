
# What exactly is SSH?
SSH or Secure shell is a network protocol that enables a secure remote access to a computer.

## Objectives
The objectives of this project's subdomain are:

- Set up Metasploitable as a Wazuh-agent
- Use Kali linux and hydra to execute a brute force attack on the agents open SSH port
- Analyze alerts on the Wazuh dashboard to practice intrusion detection and gain insights on the Threat actor
- Set up an automated threat response

## What is a Brute-force attack??
A brute-force attack involves going through every possible username and password combination to find the correct login details. The time it takes to attain the correct credentials obviously depend on the password strength. A weak password could be gotten in a matter of seconds, while a complex one might take millions of years.

## Setup 
For this Lab I'll be using Metasploitable-an intentionally vulnerable machine created by metasploit, and Kali linux-a linux distro designed for security experts, to act as the Threat actor. I'll also be using tools like Nmap(for port scanning) and Hydra(for bruteforcing)

### Setting up Metasploitable 3

There are two ways to go about this:

- Build your own OVA file: this is rather complicated so I won't go over how to do it here. Luckily Hackersploit made a great video about how to do it, check it out [here](https://www.youtube.com/watch?v=errn34YrEjM&t=247s)

- Using a community prebuilt OVA: Although much faster and easier to use, it can be a security risk if downloaded from an unverified source.

### Set up Metasploitable 3 as a Wazuh agent 
I'm using the Ubuntu version of Metasploitable hence I'll add it as an agent the same way I did it in the [Lab Setup]()

