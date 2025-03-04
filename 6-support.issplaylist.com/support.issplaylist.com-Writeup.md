# Port Scan

* Challenge - Which TCP ports are open on the support.issplaylist.com server?
* Points - 2

Since this question is multiple choice, I’ll only scan the ports that are possible answers to save time. 
 
![Port-Scan-1](https://github.com/user-attachments/assets/5e0fc418-53dc-46f0-ba4b-2bc4e9ef5206)

Viewing the output of Nmap, I see that three ports are open out of all the ports I scanned. 


# Service Version Enumeration

* Challenge - What version of OpenSSH is in use on the server?
* Points - 2

From the port scan in the previous challenge, I know that SSH is open on one of the ports. With Nmap, you can do a version scan on specific port(s) to get more detailed info. This is done with the -sV argument, as shown here: 

![Service-Version-Enumeration-1](https://github.com/user-attachments/assets/992a6cf2-9108-452b-8567-a7343a507eb0)

The output shows me the version of OpenSSH used.


# Backdoor Reuse

* Challenge - Gain access to the support.issplaylist.com system through the attacker-supplied backdoor. Submit the flag value in the /flag1.txt file.
* Points - 5

The backdoor will be on one of the open ports from the Port Scan challenge above. I looked at those scan results, and started thinking about which port to connect to with Netcat. I thought that SSH would be ruled out because I don't have credentials for a user on the target system. Next I looked at the venus service running on another open port and thought it was suspicious. To use Netcat to connect to the port running venus, I needed the IP address of the system. So I used ping to obtain the IP address, then I used Netcat to connect to the open port. 

![Backdoor-Reuse-1](https://github.com/user-attachments/assets/19717a21-4afe-4088-a500-fa2480a86346)

Now that I have a shell against the target system I run “ls” to enumerate files, then “cat flag1.txt” to get the flag. 

![Backdoor-Reuse-2](https://github.com/user-attachments/assets/b56b8920-8563-4e23-b0b6-c913b3385b41)


# User Access Enumeration

* Challenge - A listening process on the support.issplaylist.com server is deployed to grant the attacker backdoor access to the system. What is the user ID number granted to the attacker when they access the backdoor?
* Points - 3

I know the listening process that grants the attacker backdoor access to the system runs on the open port with the venus service. After connecting to the backdoor, I run the “id” command to view the user ID number - revealing the answer to this challenge. 

![User-Access-Enumeration-1](https://github.com/user-attachments/assets/dfba0964-7bd8-4358-ad3a-f41d378c0698)


# Local Privilege Escalation

* Challenge - In addition to the backdoor listener on TCP/2430, the attacker also left a privilege escalation backdoor on the system. What is the filename of the executable that grants the attacker root access?
* Points - 4

The key words here are privilege escalation. For Linux privilege escalation, there's a variety of commands to look for weak spots in the system configuration. I ran a find command to look for uncommon SETUID files. 

![Local-Privilege-Escalation-1](https://github.com/user-attachments/assets/03fc20e6-1050-4293-a8c1-e2922507775d)

After searching through the output of the find command that did not return permission denied, there were a few options. Most of those options resided in the /usr/bin/ folder, which makes sense since that directory contains the executable file for common Linux commands. 

The top file seems obvious due to its name, but I thought about if how I could identify it even if the name wasn't obvious. One indicator of a backdoor is an executable binary being located in a temporary directory, in this case /tmp. On Linux systems, executable binaries are typically stored in the /usr/bin/ directory, one being in /tmp is a something to investigate. 


# Root Privileges Required

* Challenge - Obtain root access on the system. Submit the flag value in the /flag2.txt file.
* Points - 3

From the previous challenge, I know that a file in /tmp is the file that grants the attacker root access. I run the file, and my prompt changed. To confirm I successfully obtained root access, I used the whoami command. 

![Root-Privileges-Required-1](https://github.com/user-attachments/assets/ca4b6afb-3f17-41e3-85a9-d4ce97af3c37)

After confirming that I have root access, I enumerated the files in the current working directory with ls and displayed the output of flag2.txt. 

![Root-Privileges-Required-2](https://github.com/user-attachments/assets/bc415b96-9d56-4d9b-b722-8c5bd302a7f3)


# More Backdoors

* Challenge - In addition to the TCP/2430 backdoor, the attacker added a 2nd backdoor access script to the system. Identify the 2nd attacker backdoor script, then submit the flag.
* Points - 8

First off, I started browsing different directories for suspicious files. After doing this for some time and not finding anything to note, I looked at some in-game hints. One of the hints mentioned that attackers may leave evidence behind. When I read that I thought about the bash history file and the history command. I checked the history of commands typed with the history command, but nothing is returned. This means that the attacker must have cleared it to avoid detection. 

Now it's time to find the bash_history.save file. After looking in different directories, I found it in the root directory. I used cat to view the bash_history.save file and noticed a suspicious file in /var/www/html/images. 

![More-Backdoors-1](https://github.com/user-attachments/assets/6f6e2123-9bd5-4f9c-8da9-4817486b3303)

I viewed the contents of the suspicious file in /var/www/html/images and it displayed the flag.

![More-Backdoors-2](https://github.com/user-attachments/assets/5e930e06-adc2-479f-b501-9addf80d2914)
