# Search Engine Restriction

* Challenge - Retrieve the search engine restriction resource on the web server orders.issplaylist.com. Enter the flag.
* Points - 2

The robots.txt file restricts which webpages can be indexed by search engines. You can access it by specifying it in the URL, for example here I got the flag by typing http://orders.issplaylist.com/robots.txt. 

![Search-Engine-Restriction-1](https://github.com/user-attachments/assets/bc5588ff-adfe-489a-aee4-3b230cd64cd6)


# Apache Struts Version

* Challenge - Using the information on the orders.issplaylist.com website, identify the version of Apache Struts in use.
* Points - 2

Looking at the robots.txt file screenshot from the above challenge, I can see the struts version listed in one of the lines.  


# Vulnerability Research

* Challenge - Research the version of Apache Struts using in-game assets or public Internet resources. Which CVE entry describes a known vulnerability for this version of Apache Struts?
* Points - 2

When I saw the term "in-game assets" I thought about ClippedBin and how it was used for the OSINT and Recon section of this CTF. I went to ClippedBin and searched for "struts" and found an entry. After viewing the entry, I see a CVE entry for this version of Apache Struts. 

![Vulnerability-Research-1](https://github.com/user-attachments/assets/61f20ba3-d9e2-48b8-9363-a0abec860879)

![Vulnerability-Research-2](https://github.com/user-attachments/assets/ad97543c-2ea8-4d29-b1a3-34136dca3f21)


# Exploit Identification

* Challenge - Using in-game assets, identify an exploit left behind by the attackers. Enter the name of the exploit code author (first and last).
* Points - 2

Looking at the same ClippedBin entry from the above challenge, the author's name for the exploit code is on line 6. 

![Exploit-Identification-1](https://github.com/user-attachments/assets/80a8caa3-1561-4498-a57b-efedfc203678)


# Vulnerability Exploitation

* Challenge - Use the exploit to gain access to the orders.issplaylist.com server. Retrieve the flag in the /opt/tomcat/webapps/ROOT directory.
* Points - 5

First, I created a new file from my Linux terminal called exploit.py in Gedit:
* ```gedit exploit.py```

Once the editor screen came up, I pasted the entire exploit code from the ClippedBin entry used in the above two challenges. I then saved and closed the file. The ClippedBin entry lists the URL to use with the exploit.

Make the script executable and run it:
* ```chmod +x exploit.py```
* ```./exploit.py```

When I ran the exploit code, the output suggested to run it with -h to get help information. 

![Vulnerability-Exploitation-1](https://github.com/user-attachments/assets/da2d924a-e466-46ad-bb81-34d06b5cbcec)

I used the help information to craft the command needed. After experimenting with a few commands to run this script correctly, I arrived at the solution below.

![Vulnerability-Exploitation-2](https://github.com/user-attachments/assets/eb81b631-c1b7-4447-aba5-fe00e65bc23d)


# Web Password File

* Challenge - Using the same exploit access, examine the file used for storing encrypted passwords in the web root (/opt/tomcat/webapps/ROOT/). Submit the flag.
* Points - 3

First, I used the exploit used in the above challenge to view the contents of the ```/opt/tomcat/webapps/ROOT``` directory. 

![Web-Password-File-1](https://github.com/user-attachments/assets/33a7ce7c-1c70-4ff6-9d54-4d59ab74936b)

Viewing the directory, I see the ```.htpasswd``` file, which is the file used for storing encrypted passwords. The next step is to craft a command to display the contents of this file. For this, I changed the command to be run inside the single quotes from ```ls``` for show the directory's content, to ```cat``` so I can view the ```.htpasswd``` file.

![Web-Password-File-2](https://github.com/user-attachments/assets/a8441cb2-156f-4728-bafb-6724fc4ae8d9)


# Always Be Cracking

* Challenge - Crack the password for the tomcat user using the .htpasswd file contents. Submit the plaintext password for the tomcat user.
* Points - 3

From the output of the script used in the last challenge to view the encrypted passwords, I copied the hash to my clipboard. Back at my Linux terminal, I ran ```gedit orders-hash.txt``` to create a new file with Gedit and paste the hash into the file. Next I saved and closed the file. 

Next, I used hashcat to recover the password, supplying my newly created hashfile with the single hash and the word list provided from this course's labs. The password hash has a username, so I used ```--user``` in my hashcat command to tell hashcat to expect a username. 

![Always-Be-Cracking-1](https://github.com/user-attachments/assets/fc39df42-9bee-482d-9d19-a068138b81eb)

In Hashcat, cracked passwords are show in in the format ```username:hash:password``` when you use the ```--user``` argument. 

# Remote Shell

* Challenge - Using the compromised username and password, access a shell on the target server. Submit the flag shown when logging in.
* Points - 3

SSH is one way to get a remote shell to a server. I log into the server using the compromised credentials for the tomcat user, and the flag is shown.  

![Remote-Shell-1](https://github.com/user-attachments/assets/675139d6-afbd-448a-a871-2f0416fab3be)


# Local Privilege Management

* Challenge - The support system has a custom sudo configuration. Which command is the tomcat user allowed to run as root?
* Points - 2

Now that I have SSH access to the system as the tomcat user, I run the command below to see what the tomcat user can run as root.  

![Local-Privilege-Management-1](https://github.com/user-attachments/assets/c13e8c83-d2da-4336-a9dd-765e026eac43)

From the multiple choice options, one of the options is present in the output, so that's the right answer.


# Local Privilege Escalation

* Challenge - Using the commands available through sudo, escalate privileges to gain root access on the system. Retrieve the flag stored in the /etc/shadow file.
* Points - 4

From running the command in the above challenge, I see that the tomcat user can run chmod, chown, and chgrp as root. Chmod is used to change access permissions of files and directories, so I used the fact that I can run chmod as root to view the /etc/shadow file. 

![396520663-ac395b16-77da-43f0-99e7-d485ad417efb](https://github.com/user-attachments/assets/f2cef1b8-cee0-434e-a7b1-fdcf82db2dc4)


# jorestes Password

* Challenge - What is the login password for the jorestes user account?
* Points - 4

I copied the /etc/shadow contents into my clipboard and typed "exit" to drop my SSH session and return to my terminal. Then I pasted the password hashes into a new file.
* gedit tomcat-hashes.txt

After pasting the hashes in Gedit, I saved and closed the file. Now it's time to crack the jorestes password with Hashcat. I cracked the hashes with Hashcat, then I displayed the cracked passwords and associated usernames with the --show and --user arguments.

![Local Privilege Escalation-2](https://github.com/user-attachments/assets/758754f4-5125-414c-90f5-6d4f8b29f24b)

![396523377-c0623290-2e2c-4b13-8d89-a1e78ae52ccd](https://github.com/user-attachments/assets/3343a6c9-3556-4e9e-9c93-b038db434cbf)

The credentials for the jorestes user are retrieved by Hashcat in the format of <user>:<password_hash>:<plaintext_password>.


# pemma Password

* Challenge - What is the login password for the pemma user account?
* Points - 4

From the Hashcat commands ran in the above challenge, the password for the penma user was cracked. 
