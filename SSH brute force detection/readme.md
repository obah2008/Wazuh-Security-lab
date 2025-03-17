
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



