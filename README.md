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
- An Ubuntu ISO

### Skill Prerequisites(knowledge):

 - Basic knowledge of Cyber security concepts 
 - Basic Linux Command line interface(CLI) familiarity 
 - A Basic understanding of networking(IP addressing, firewalls)


