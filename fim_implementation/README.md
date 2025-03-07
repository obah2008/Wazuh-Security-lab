# File integrity and monitoring with Wazuh

# What is FIM

File Integrity  and monitoringis a security feature that detects and watches changes to selected directories and files. It's useful for detecting malware and monitoring the activities of attackers.

## Setting up FIM in Wazuh

### Locate the Ossec configuration file

#### Linux
To navigate to the ossec terminal on Linux, open the terminal and Use the command

    sudo nautilus /var/ossec/etc/


#### Windows
 For Windows, open File Explorer and navigate to:

C:\Program Files (x86)\ossec-agent\


### Edit ossec.conf file
- Make a copy of the file to use as a backup(very important!!!. Learnt that the hard way)
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
For this step I'll be using the EICAR test file. The Eicar test file is a harmless non malicious test file used to check the functionality of antiviruses and other security tools.

- Navigate to the monitored directory: on the linux terminal, Use the command



        cd /home/obah/Documents/"important data"

- Download the EICAR file with the command

Linux

      curl -O https://secure.eicar.org/eicar.com

Windows

Download the EICAR file from [download EICAR](https://www.eicar.org/download-anti-malware-testfile/)

### Step 3:Check Wazuh for security alerts
On the Wazuh dashboard, navigate to the FIM event menu, we can see the new file that has been added, but the SIEM tool is currently unable to detect that this file is malware.
To enable Wazuh detect that the EICAR file is indeed malware, I'll be using VirusTotal. VirusTotal is a service that analyses files and URLs for potential threats.
 
#### virustotal integration to wazuh
-  Step 1: Get VirusTotal API key: log into the [Virustotal website](https://www.virustotal.com) and copy the API key
-  Step 2: configure Virustotal integration on the wazuh manager: Navigate to the ossec.conf file on the wazuh manager and 
   add this XML line at the right before the last line
   ```
     <integration>
      <name>virustotal</name>
      <api_key>API_KEY</api_key> 
      <alert_format>json</alert_format>
    </integration>                              
-  Replace "API_key" with the virustotal API key and save the changes to the file
- Restart the Wazuh-manager

Now we can put the EICAR file in the directory monitored by FIM, and Wazuh should now be able to detect it as malware.

Under Malware detection option virustotal should flag the EICAR.com file as malware with a rule.id of 87105
  
Side note: If you're using windows, before you put the EICAR file in your monitored directory, turn off windows defender.
 ### Step 4: Enable Active response


