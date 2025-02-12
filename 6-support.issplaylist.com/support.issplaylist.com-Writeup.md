# Port Scan

* Challenge - Which TCP ports are open on the support.issplaylist.com server?
* Points - 2

Since this question is multiple choice, I’ll only scan the ports that are possible answers to save time. 
 
![396673680-76056844-2607-4203-936a-4f813f330f21](https://github.com/user-attachments/assets/17921ff7-0520-4026-8591-a689446223ca)

Viewing the output of Nmap, I see that three ports are open out of all the ports I scanned. 


# Service Version Enumeration

* Challenge - What version of OpenSSH is in use on the server?
* Points - 2
* Answer - 8.2p1

From the port scan in the previous challenge, I know that SSH is open on TCP port 22. With Nmap, you can do a version scan on specific port(s) to get more detailed info. This is done with the -sV argument, as shown here: 

![Service Version Enumeration-1](https://github.com/user-attachments/assets/ef9cc514-b0c7-4262-bf68-ec44c4dd5937)

The output shows me the version of OpenSSH used, 8.2p1.


# Backdoor Reuse

* Challenge - Gain access to the support.issplaylist.com system through the attacker-supplied backdoor. Submit the flag value in the /flag1.txt file.
* Points - 5
* Answer - NetWars{ShellAccessIsGood}

The backdoor will be on one of the open ports from the Port Scan challenge above. I looked at those scan results, and started thinking about which port to connect to with Netcat. I thought that SSH would be rules out because I don't have credentials for a user on the target system. Next I looked at the service running on port 2430 and thought it was suspicious. To use Netcat to connect to the port of the target system, I needed the IP address of the system. So I used ping to obtain the IP address, then I used Netcat to connect to port 2430. 

![Backdoor Reuse-1](https://github.com/user-attachments/assets/330036e7-11da-4ca6-8880-f5bb40dcbc01)

Now that I have a shell against the target system I run “ls” to enumerate files, then “cat flag1.txt” to get the flag. 

![Backdoor Reuse-2](https://github.com/user-attachments/assets/c4a3998d-b655-42f6-b24f-2046fdd6f57a)


# User Access Enumeration

* Challenge - A listening process on the support.issplaylist.com server is deployed to grant the attacker backdoor access to the system. What is the user ID number granted to the attacker when they access the backdoor?
* Points - 3
* Answer - 1

I know the listening process that grants the attacker backdoor access to the system runs on TCP 2430 from the previous challenges. After connecting to the backdoor, I run the “id” command to view the user ID number. This shows a uid of 1. 

![User Access Enumeration](https://github.com/user-attachments/assets/4dc41aeb-c3db-4227-bd5d-69c107ff4483)


# Local Privilege Escalation

* Challenge - In addition to the backdoor listener on TCP/2430, the attacker also left a privilege escalation backdoor on the system. What is the filename of the executable that grants the attacker root access?
* Points - 4
* Answer - backdoor

The key words here are privilege escalation. For Linux privilege escalation, there's a variety of commands to look for weak spots in the system configuration. I ran a find command to look for uncommon SETUID files. 

![Local Privilege Escalation-1](https://github.com/user-attachments/assets/a83712d3-21b3-4952-a6a3-484357cabeb8)

After searching through the output of the find command that did not return permission denied, there were a few options. Most of those options resided in the /usr/bin/ folder, which makes sense since that directory contains the executable file for common Linux commands. 

![Local Privilege Escalation-1](https://github.com/user-attachments/assets/471d0168-28bf-4835-a4b0-183aa8ef510e)

The backdoor file seems obvious, but I thought about if how I could identify it even if the name wasn't backdoor. One indicator of a backdoor is an executable binary being located in a temporary directory, in this case /tmp. On Linux systems, executable binaries are typically stored in the /usr/bin/ directory, one being in /tmp is a something to investigate. 


# Root Privileges Required

* Challenge - Obtain root access on the system. Submit the flag value in the /flag2.txt file.
* Points - 3
* Answer - NetWars{RootAccessIsBetter}

From the previous challenge, I know that /tmp/backdoor is the file that grants the attacker root access. I run the backdoor file, and my prompt changed. To confirm I successfully obtained root access, I used the whoami command. 

![Root Privileges Required-1](https://github.com/user-attachments/assets/3f8b412b-ebeb-40da-be18-03b95bd6e32d)

After confirming that I have root access, I enumerated the files in the current working directory with ls and displayed the output of flag2.txt. 

![Root Privileges Required-2](https://github.com/user-attachments/assets/2295bdc6-2c47-4ef8-bb6b-0180f89aa538)


# More Backdoors

* Challenge - In addition to the TCP/2430 backdoor, the attacker added a 2nd backdoor access script to the system. Identify the 2nd attacker backdoor script, then submit the flag.
* Points - 8
* Answer - NetWars{WebshellBackdoor}

First off, I started browsing different directories for suspicious files. After doing this for some time and not finding anything to note, I looked at some hints. One of the hints mentioned that attackers may leave evidence behind. When I read that I thought about the bash history file and the history command. I checked the history of commands typed with the history command, but nothing is returned. This means that the attacker must have cleared it to avoid detection. 

Now it's time to find the bash_history.save file. After looking in different directories, I found it in the root directory. I used cat to view the bash_history.save file and noticed a suspicious file called bk.php in /var/www/html/images. 

![More Backdoors-1](https://github.com/user-attachments/assets/4233e66b-fad0-4108-8926-56efd19af6a5)

I viewed the contents of the suspicious file in /var/www/html/images and it displayed the flag.

![More Backdoors-2](https://github.com/user-attachments/assets/447686c6-050e-427f-9fd6-60a7e4bdd2ec)
