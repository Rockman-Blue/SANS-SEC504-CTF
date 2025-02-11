# Search Engine Restriction

* Challenge - Retrieve the search engine restriction resource on the web server orders.issplaylist.com. Enter the flag.
* Points - 2

The robots.txt file restricts which pages can be indexed by search engines. You can access it by specifying it in the URL, for example here I got the flag by typing http://orders.issplaylist.com/robots.txt. 

![Search Engine Restriction-1](https://github.com/user-attachments/assets/0a83be58-0472-48f3-b8a8-ebd628f8e1c2)


# Apache Struts Version

* Challenge - Using the information on the orders.issplaylist.com website, identify the version of Apache Struts in use.
* Points - 2

Looking at the robots.txt file screenshot from the above challenge, we can see the struts version listed in one of the lines.  


# Vulnerability Research

* Challenge - Research the version of Apache Struts using in-game assets or public Internet resources. Which CVE entry describes a known vulnerability for this version of Apache Struts?
* Points - 2

When I saw the term "in-game assets" I thought about ClippedBin and how it was used for the OSINT and Recon section of this CTF. I went to ClippedBin and searched for "struts" and found an entry. After viewing the entry, I see a CVE entry for this version of Apache Struts. 

![Vulnerability Research-1](https://github.com/user-attachments/assets/7c1b2a92-f370-477e-8826-a1646eb964d7)
![396504087-bb3bd7d9-69f3-466d-831b-8a21a2040e59](https://github.com/user-attachments/assets/9dc83e9c-0151-44e5-96d7-751bf4e65ade)


# Exploit Identification

* Challenge - Using in-game assets, identify an exploit left behind by the attackers. Enter the name of the exploit code author (first and last).
* Points - 2

Looking at the same ClippedBin entry from the above challenge, the author's name for the exploit code is in line 6. 

![396504087-bb3bd7d9-69f3-466d-831b-8a21a2040e59](https://github.com/user-attachments/assets/a86beb50-9928-481a-8bbf-8b3572827fba)


# Vulnerability Exploitation

* Challenge - Use the exploit to gain access to the orders.issplaylist.com server. Retrieve the flag in the /opt/tomcat/webapps/ROOT directory.
* Points - 5

First, I created a new file from my Linux terminal called exploit.py in Gedit:
* gedit exploit.py
Once the editor screen came up, I pasted the entire exploit code from the ClippedBin entry used in the above two challenges. I then saved and closed the file. The ClippedBin entry lists the URL to use with the exploit.

Make the script executable and run it:
* chmod +x exploit.py
* ./exploit.py

When I ran the exploit code, the output suggested to run it with -h to get help information. 

![Vulnerability Exploitation-1](https://github.com/user-attachments/assets/69de6b86-a332-4cdc-9884-77fdcd6c5d24)

I used the help information to craft the command needed. After experimenting with a few commands to run this script correctly, I arrived at the solution below.

![396504087-bb3bd7d9-69f3-466d-831b-8a21a2040e59](https://github.com/user-attachments/assets/45d1da2b-8b0a-4d60-878e-515232c9f6b6)


# Web Password File

* Challenge - Using the same exploit access, examine the file used for storing encrypted passwords in the web root (/opt/tomcat/webapps/ROOT/). Submit the flag.
* Points - 3

First, I used the exploit used in the above challenge to view the contents of the /opt/tomcat/webapps/ROOT directory. 

![Web Password File-1](https://github.com/user-attachments/assets/add431f3-840d-4b4b-87a1-b407284f90f5)

Viewing the directory, I see the .htpasswd file, which is the file used for storing encrypted passwords. The next step is to craft a command to display the contents of this file. For this, I changed the command to be run inside the single quotes from ls to show the directory's content, to cat to view the .htpasswd file.

![Web Password File-2](https://github.com/user-attachments/assets/700eccf0-5aa6-4d1e-a18e-34e73e65a742)


# Always Be Cracking

* Challenge - Crack the password for the tomcat user using the .htpasswd file contents. Submit the plaintext password for the tomcat user.
* Points - 3

From the output of the script used in the last challenge to view the encrypted passwords, I copied the hash to my clipboard. Back at my Linux terminal, I ran "gedit orders-hash.txt" to create a new file with Gedit and paste the hash into the file. Then I saved and closed the file. 

Next, I used hashcat to recover the password, supplying my newly created hashfile with the single hash and the word list provided from the labs. The password hash has a username, so I used --user in my hashcat command to tell hashcat to expect a username. 

![Web Password File-2](https://github.com/user-attachments/assets/d31ebb94-d51d-465a-9abc-d8f83ef03357)

In Hashcat, cracked passwords are show in in the format username:hash:password when you use the --user argument. 

# Remote Shell

* Challenge - Using the compromised username and password, access a shell on the target server. Submit the flag shown when logging in.
* Points - 3

SSH is one way to get a remote shell to a server. I logged into the server using the compromised credentials from previous challenges in this section, tomcat/tacmot. 

![Web Password File-2](https://github.com/user-attachments/assets/82b5a4b5-4801-4c87-a32b-1e02602a4abd)


# Local Privilege Management

* Challenge - The support system has a custom sudo configuration. Which command is the tomcat user allowed to run as root?
* Points - 2

Now that I have SSH access to the system as the tomcat user, I can run the command below to see what the tomcat user can run as root.  

![Web Password File-2](https://github.com/user-attachments/assets/30ef4cd3-67e7-41ee-840d-6431884d8418)

From the multiple choice options present, one of the options in the output, so that's the right answer.


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
