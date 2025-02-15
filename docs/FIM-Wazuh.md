# File integrity and monitoring with Wazuh

# What is FIM

File Integrity  and monitoringis a security feature that detects and watches changes to selected directories and files. It's useful for detecting malware and monitoring the activities of attackers in public directories.

## Setting up FIM in Wazuh

### Locate the Ossec configuration file

#### Linux
To navigate to the ossec terminal on Linux, open the terminal and Use the command

    sudo nautilus /var/ossec/etc/


#### Windows
 For Windows, open File Explorer and navigate to:

C:\Program Files (x86)\ossec-agent\


### Edit ossec.conf file
- Make a copy of the file to use as a backup
- Open the file with Notepad(windows) or text editor(linux)

#### Change the frequency at which syscheck looks for changes: 

The default for this is 12 hours(43200s) which is way too long for a personal project, so I'm going to change it to 10 minutes(600s) 

#### Select the directories that syscheck will monitor
Under default files to be monitored, copy and paste this
    
     <directories>file path</directories>

Replace "file path" with the actual path of the file

- Save and close the Ossec file

### Restart Wazuh 
For the changes we've made in the Ossec.conf file to take effect, the Wazuh manager and Agent must be restarted.

#### Linux
use the command

    sudo systemctl restart wazuh-agent

#### Windows
- Open run using Win + R, type services.msc
- Right click on the Wazuh service.


## Case 1: Detecting malware with Wazuh
Scenario: An attacker has implanted Malware within an important directory. we need to detect, analyze and respond to it using Wazuh's FIM


### Step 1: Select A directory to monitor
For this I'll be choosing a directory with "sensitive" data. In my case it would be
 /home/obah/Documents/important data
 
to do this: 
- Add a new entry under syscheck

      <directories check_all="yes">/home/obah/Documents/important data</directories>
- save the changes made to the ossec.conf file
- Restart Wazuh

### Step 2: Malware Implantation
For this step I'll be using the EICAR test file. The Eicar test file is a harmless test file used to check the functionality of antiviruses and other security tools

- To enable Wazuh detect that the EICAR file is indeed malware

### Step 3:Check Wazuh for security alerts 

### Step 4: Enable Active response


## Case 2: 
let's say we have an application that stores important passwords, we'll use FIM to monitor this file for changes



[Guide video](https://www.youtube.com/watch?v=aO2jUOFa9Hs&t=461s)
