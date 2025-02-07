# Security Home Lab with Wazuh

## Description

This project sets up a security home lab built using Wazuh, an open source Security Information and Event Management(SIEM), and XDR(extended detection and response)

## What exactly is Wazuh?

Wazuh is a free and open source SIEM that allows for Endpoint security, Threat intelligence, Security operations and cloud security.

An SIEM is a tool that allows large scale log analysis over multiple systems and networks 

An XDR allows for integration of data from multiple sources, it essentially extends the reach of an SIEM

Wazuh is divided into three components:

- Wazuh Indexer: The  Wazuh indexer stores data received from the agents, and makes it available for querying. It's essential for log analysis on a large scale.

- Wazuh Server: The Wazuh server processes the data received from the agents and performs analysis based on a set of predefined rules and policies.

- Wazuh dashboard: The Wazuh dashboard allows users to manage services through a well structured GUI.

You can learn more about Wazuh [Wazuh official website](https://wazuh.com)

## Table of contents

- [Description](#description)
- [Objectives](#objectives)
- [Prerequisites](#prerequisites)
- [Lab Setup](#lab-setup)
- [Tools Used](#tools-used)
- [Configuration](#configuration)
- [Queries](#queries)
- [Dashboards](#dashboards)
- [Acknowledgements](#acknowledgements)


## Objectives 

The main objectives of this project are:

1. Set up a secure environment consisting on Ubuntu, windows and another Linux distro 

2. Install and configure the Wazuh server on Ubuntu 24.04 to act as the Log aggregator and central SIEM

3. Monitor logs from The Wazuh agents(Windows and Kali) to identify potential security threats.

4. Simulate common security attacks and defensive actions to practice Incident Detection and Response.


## Prerequisites 


Must have before starting the project

### Technical Prerequisites(tools/hardware)

- A hypervisor or virtualization software(VMware or VirtualBox)
- A 64-bit operating system that supports Wazuh server(in this case Ubuntu)
- A PC that meets the minimum system requirements to run Wazuh locally
- A Windows ISO
- A Ubuntu ISO
- Docker(optional)

### Skill Prerequisites(knowledge):

 - Basic knowledge of Cyber security concepts 
 - Basic Linux Command line interface(CLI) familiarity 
 - A Basic understanding of networking(IP addressing, firewalls)


## Lab setup

### Step 1: Install Ubuntu

Ubuntu can be installed either by using a virtualization software or by downloading and installing the ISO directly on a PC(Bare metal installation). I'll be using the latter.

#### Bare metal

- Download Ubuntu from [Download Ubuntu](https://ubuntu.com/download/desktop)
- Install on a hard drive partition or a PC without an OS [follow installation guide](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview)
- Once installed, update your system:
  ```bash
  sudo apt update && sudo apt upgrade

#### Virtualization 
- Download the latest Ubuntu Server ISO from the [official website](https://ubuntu.com/download/server).
- Create a VM for Ubuntu Server and follow the installation guide.
- Once installed, update your system:
  ```bash
  sudo apt update && sudo apt upgrade


### Step 2: Install Wazuh on Ubuntu

- Head over to [Wazuh documentation](https://documentation.wazuh.com/current/quickstart.html) for a detailed installation guide.
- Run the installation script: On the Ubuntu terminal use the command 
  ```bash
   curl -sO https://packages.wazuh.com/4.10/wazuh-install.sh && sudo bash ./wazuh-install.sh -
- Save login credentials: Once The installation process is complete, copy the username and password
- Find The server's IP address: To find your severs ip address, use the command
   ```bash
  ip ad s
 Copy the the IP address and paste it in you web browser.
- Log into the Wazuh web interface: Once on the Wazuh web log in screen, enter you the username and password copied earlier. 


### Step 3: Deploying Wazuh agents
Wazuh agents can be deployed on several platforms for endpoint detection, log accumulation and Threat-hunting. Not only can docker be run on serveral different Operating Systems it can also be run on a variety of platforms.

#### Deploying an agent on Linux
- Head to the Wazuh server, on the overview page select deploy new agent
- Select the operating system and architecture for the new agent, I'm using Ubuntu hence I'll select Deb amd 64
- Assign a Server address: type in the address you copied earlier(the address that was used to access the dashboard)
- Assign agent name: It's optional but you can assign an agent name that uniquely identifies the new agent 
- Download and install the agent: On the ubuntu terminal, run the command 
  ```bash
   wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.10.1-1_amd64.deb && sudo WAZUH_MANAGER='your server's address' dpkg -i ./wazuh-agent_4.10.1-1_amd64.deb
- Start the agent by running the following commands
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable wazuh-agent
  sudo systemctl start wazuh-agent
- Verify the agent is up and running: to verify the statud of the agent use the command
  ```bash
  sudo systemctl status wazuh-agent

#### Deploying agents on Windows
- Deploy a new agent by following the same process as above 
- Select operating system: choose Windows under the, select package to download menu
- Assign Server IP address: copy the address of the wazuh server and paste it here
- Assign an agent name: Again this field is optional, if you do not fill it out Wazuh will use the agent's hostname to identify the agent
- Select one or more existing groups: Another optional field used to group agents together. 
- On powershell run the following command to download and install the agent 
     ```bash
     Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.10.1-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='your server's ip address'
 change "your server's ip address' to the actual ip address of your Wazuh server
- Verify the agent is running using the command
  ```bash 
   Get-Service -Name WazuhSvc

#### Deploying agents from a container(Ubuntu)
-
-
-
-
