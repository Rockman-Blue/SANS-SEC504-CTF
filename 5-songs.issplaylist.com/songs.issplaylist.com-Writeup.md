# Port Scan

* Challenge - Which TCP ports are open on the songs.issplaylist.com server?
* Points - 2

Nmap is a popular tool that offers port scanning and other useful features. This question was multiple choice, so I limited my port scan to only the ports in the answer options to save time. When possible, use Nmap with sudo for more accurate results. 

![Port-Scan-1](https://github.com/user-attachments/assets/253223d1-8df9-4d38-b6b5-eb1ec85508c5)

Nmap tells me which ports are open and closed with the reason information. 


# Site Reconnaissance

* Challenge - Browse to the songs.issplaylist.com site. Examine the site to collect basic information. Find the file associated with search engine crawlers. Submit the flag.
* Points - 2

The ```robots.txt``` file is a file associated with search engine crawlers. It tells search engines which pages on the web server to hide from indexing. I went to http://songs.issplaylist.com/robots.txt and the flag is displayed in the ```robots.txt``` file.

![Site-Reconnaissance-1](https://github.com/user-attachments/assets/3d5780d1-7e5d-40f0-b31a-163cf80fdf91)


# Invalid Status

* Challenge - The songs.issplaylist.com site offers users the ability to check on the status for song submissions. Identify the flag associated with an invalid submission ID value.
* Points - 2

I went to the http://songs.issplaylist.com/ site and clicked on the Status page in the navbar at the top. From there, I see a field where I can input a number to check the status of the submission associated with it.

![Invalid-Status-1](https://github.com/user-attachments/assets/1fa88694-0ff8-4698-935e-8e6b535255a3)

The field expects an ID number, so anything that is not a number is an invalid submission ID value. I entered 'a' in the form and clicked on the submit button. This invalid submission returns the flag. 

![Invalid-Status-2](https://github.com/user-attachments/assets/5b6118c1-4a42-4a3c-8cf3-2546ee2f238f)


# Valid Status

* Challenge - Continue to examine the song submission status check feature on the songs.issplaylist.com website. Identify the flag associated with a valid submission ID value.
* Points - 3

At the status page, the first thing I tried was inputting several numbers starting from 1. However, I still kept getting the same error page as the screenshot above. After trying many numbers and not getting anywhere, I decided to look back on other challenges in this section of the CTF event.

The ```robots.txt``` file has a line with a number, ```128```. 

![Valid-Status-1](https://github.com/user-attachments/assets/8bf66d76-2602-4f72-aabb-db9eed5512d0)

I thought to try inputting ```128``` into the form on status page. This worked and the flag was displayed on the page. 

![Valid-Status-2](https://github.com/user-attachments/assets/2731adc4-e276-49be-a924-09fad93cfa78)


# Vulnerability Discovery

* Challenge - Explore the songs.issplaylist.com website to identify a vulnerability. Identify the type of vulnerability.
* Points - 4

This question is multiple choice. I tried the different choices from top to down, starting with SQL injection. First I tried inputing a single quote, ' into the input field of the status page's form. This did not return a SQL error, but instead the same error page that contained the NetWars flag like in the image below. 

![Vulnerability-Discovery-1](https://github.com/user-attachments/assets/a817b00e-b321-431b-9341-cca96adf9f17)

I know that there are many ways to attempt stack commands, mainly by putting a ```;```, ```&&```, and ```||``` in between two commands. I tried multiple command stacking tests in the input field along with the ```whoami``` command. I use this command because ```whoami``` works on both Linux and Windows systems. I used a command that would work on both systems to make identifying the vulnerability easier. 
* ```128 ; whoami```
* ```128 || whoami```
* ```128 && whoami```

After trying the first two options in the bullet list above, I got the same error page as the screenshot above when I tried testing the form for an SQL injection vulnerability. However, when I tried the last option in the bulleted list above, I noticed the output was different. The ```whoami``` command ran successfully.

![Vulnerability-Discovery-2](https://github.com/user-attachments/assets/e7b696ba-8758-48c3-959e-b970934ffe9e)


# Vulnerability Exploitation

* Challenge - Use the vulnerability identified in the check submission functionality of the songs.issplaylist.com site to identify the flag in the web root directory c:\InetPub\songs.issplaylist.com\wwwroot.
* Points - 5

The general formula to target the page's vulnerability is typing ```128 &&``` followed by a Windows command, so this challenge is about crafting the right command. I know the target is a Windows machine because of the file path in the question. 

On Windows machines, ```dir``` can be used to display directory contents. I used the ```dir``` command to find the filename associated with the flag in the web root directory by typing the below input into the field:
* ```128 && dir c:\InetPub\songs.issplaylist.com\wwwroot```

![Vulnerability-Exploitation-1](https://github.com/user-attachments/assets/a1b54562-6032-43dc-b3b0-b56e965afd99)

The output of the attack confirms the presence of ```F-L-A-G.txt``` in the web root directory, ```wwwroot```. Now that I know what file I'm looking for and the location, it's time to craft the right command to display the flag file's content. The Windows ```type``` command displays the contents of the specified text file. I used the following command:
* ```128 && type c:\InetPub\songs.issplaylist.com\wwwroot\F-L-A-G.txt```

![Vulnerability-Exploitation-2](https://github.com/user-attachments/assets/b29947e0-8d6b-486d-bab3-f9e361df1e37)

The attack works successfully and I'm able to see the output of the ```F-L-A-G.txt``` file. That output is the answer to this challenge. 


# Windows Version Enumeration

* Challenge - What version of Windows Server is used for the songs.issplaylist.com server?
* Points - 2

From the previous challenges, I know the following information:
* The songs.issplaylist.com target is vulnerable to an attack at the status page.
* The songs.issplaylist.com target is a Windows server system.

Windows has many built-in utilities for system information enumeration. One of them is the ```ver``` tool, which I used in an attack to get the version of Windows server. I entered the below command into the input field:
* ```128 && ver```

This returns the version number. 

![Windows-Version-Enumeration-1](https://github.com/user-attachments/assets/f5219df1-2727-4b2b-8165-7dc3ee39957c)


# Privilege Evaluation

* Challenge - Evaluate the privileges available to the attacker by exploiting this vulnerability. Which user account is assigned to the web server process?
* Points - 2

The ```whoami``` command shows user, group, and privilege information for the user logged onto the system. In this case, using it against the target server will display the user account assigned to the web server process. 

The output to ```whoami``` is in the general format:
* ```DomainName\username```

I used a attack against the server with ```whoami``` by typing the following into the input field:
* ```128 && whoami```

The output shows the account that is assigned to the web server process. 

![Privilege-Evaluation-1](https://github.com/user-attachments/assets/94ab6bd4-17b8-45bf-b4a9-66e297ab8cce)


# Privileged Assessment

* Challenge - Enumerate the SMB shares on the songs.issplaylist.com server. Identify the name of the non-hidden share.
* Points - 3

The Windows ```net view``` command can be used to display SMB shares shared by a specific computer. First I tried inputting the following command into the input field:
* ```128 && net view /all \\songs.issplaylist.com```

![Privileged-Assessment-1](https://github.com/user-attachments/assets/e2f90271-abaa-49c7-9628-9df99a21d48f)

This did not show me the output I wanted. After some troubleshooting, I figured out that to use the ```net view``` command to list all of the shares, I needed to specify the server IP after //. 

At my attacker terminal, I ran the ping command against songs.issplaylist.com to get the IP of the server. 

![Privileged-Assessment-2](https://github.com/user-attachments/assets/384157a6-cfe6-462f-8aa3-5d399d9310df)

Now that I have the IP, I tried using the same ```net view``` command as above, but this time putting ```\\IP``` instead of ```\\Hostname```. I entered the following command: 
* ```128 && net view /all \\10.142.146.70```

![Privileged-Assessment-3](https://github.com/user-attachments/assets/223fae26-62cb-4413-86e7-cf9e60f375ca)

Hidden shares end in $, so the share not ending in $ is the non-hidden share and the answer to this challenge. 


# Access Share

* Challenge - Access the non-hidden share on the songs.issplaylist.com server. Retrieve the flag.
* Points - 6

To access the non-hidden share, I have two main tools, Rpcclient and Smbclient. Rpcclient is used to gather target configuration details. Smbclient is used to interact with shares, to enumerate, download and upload files. To use either of these tools, I need valid credentials.

Re-using the site's vulnerability, I create a new user account and add it to the administrators group with two commands. First, I input this command into the input field:
* ```128 && net user /add Rockman Password1```

![Access-Share-1](https://github.com/user-attachments/assets/d0c3bb1e-8c0c-482c-a280-2768249cae9b)

The command output confirms that my user Rockman was added. Next I added my newly created user account to the administrators group with the following command injection: 
* ```128 && net localgroup administrators /add Rockman```

![Access-Share-2](https://github.com/user-attachments/assets/9ef1117e-8373-4ca2-aa90-65beb1a5cb83)

The command output confirmed that my user, Rockman was added to the administrators group. Now it's time to go to my attacker terminal and use Smbclient to download the flag file from the share. When I typed the below command and entered my password, I was granted with an SMB prompt, and a connection to the non-hidden share specified in the command (syntax is ```//<domain_name>/<share_name>```).

![Access-Share-3](https://github.com/user-attachments/assets/c8b512d4-91df-454e-9c41-9641eaf86378)

Now that I'm connected to the share, it's time to enumerate the share's contents. With Smbclient, this can be done with ```ls```. I typed ```ls``` at the SMB prompt to enumerate the share's files.  

![Access-Share-4](https://github.com/user-attachments/assets/b5b33d50-67bb-4cdb-bfb6-4debd6f35ce1)

Among the files in the share is ```flag.txt```, which contains the answer to this challenge. With Smbclient, you can type ```get <filename>``` to download the SMB share's file to your local system. I do this for ```flag.txt``` to download the file to my Linux VM. Next, I close the SMB connection, confirm the ```flag.txt``` file was download, and view the file's output to find the flag. 

![Access-Share-5](https://github.com/user-attachments/assets/17f782db-b0ef-4c76-8b5d-27f84921c7d0)


# Always Be Cracking

* Challenge - Use your credentials to access the songs.issplaylist.com server. Submit the NT password hash value for the Administrator account.
* Points - 5

For this challenge, I'll be using Metasploit with the ```windows/smb/psexec``` module against the target system. 

![Always-Be-Cracking-1](https://github.com/user-attachments/assets/00add64c-b5f9-4c9b-ae2c-fe0c96131cae)

I'll keep the default payload the same. After viewing the options for the module and the payload, it's time to set them. 

![Always-Be-Cracking-2](https://github.com/user-attachments/assets/1992ac25-5f1d-426a-ab55-b34d993a9950)

The SMB options are for the user and password to login as when the exploit runs successfully. ```RHOST``` is the target system, songs.issplaylist.com, and I got the target's IP by running ```ping songs.issplaylist.com``` in a new terminal window. ```LHOST``` is the IP of my system. With a reverse shell, the victim connects to the attacker, so the attacking machine must be ready to listen and receive that connection. I set these options and confirm them by running ```show options``` at the msfconsole prompt. 

![Always-Be-Cracking-3](https://github.com/user-attachments/assets/a3096c94-f1dc-40ea-ba4b-a083ef68fbf7)

Now that the options are set, I type ```exploit``` to send the exploit to the target system. It works and I gain a Meterpreter shell. 

After successfully gaining a reverse shell with Meterpreter against the target sytem, it's time to migrate to the LSASS process, and then run the ```hashdump``` command. The ```hashdump``` command fails, because it's a privileged operation. To fix the privilege issue, I migrated to the ```lsass.exe``` process. After this, I run ```hashdump``` again and was able to get the password hashes for several users. 

![Always-Be-Cracking-4](https://github.com/user-attachments/assets/8b3cd261-353a-4c67-99e9-b82436ed4e19)

Hashdump is used to dump the contents of the SAM database on a Windows system. The SAM database stores user password information, similar to how ```/etc/shadow``` on Linux stores password hashes. Now that I have the password hashes, I copied the hashes to my clipboard. Next, I typed ```exit``` at the Meterpreter prompt to drop my reverse shell connection to the target system. Lastly, I typed ```exit``` to exit the Metasploit console and return to the Linux termal. From there I used Gedit to create a new file, paste the hashes into it, save the file, then close it. 

![Always-Be-Cracking-5](https://github.com/user-attachments/assets/a0e9219f-a157-4b6a-abf0-5af367df321e)

Now that I have the hashfile, it's time to look for the NT hash for the Administrator account. The screenshot above shows the hashes, and the NT password hash is the rightmost section of the colon separated fields from hashdump’s output. So I just find the row for the Administrator account, and find the rightmost section, that's the NT hash.


# Always Be Cracking (2)

* Challenge - Submit the Administrator account plaintext password.
* Points - 5

With the hash file, I use Hashcat to crack the recovered password hashes. Note that the screenshot output is not the full output.

![Always-Be-Cracking-(2)-1](https://github.com/user-attachments/assets/79d805a4-be67-4c06-b57a-d271cce8971e)

With Hashcat, I can specify the following generic format to view already cracked passwords and their associated usernames for a hashfile:
* ```hashcat hashfile_name --show --user```

I used these arguments for the ```songs-hashes.txt``` file. The password for the Administrator account was successfully cracked. 

![Always-Be-Cracking-(2)-2](https://github.com/user-attachments/assets/7cc26404-1fef-4387-b7c9-5e884f7e177b)
