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

This question is multiple choice. I tried the different choices from top to down, starting with SQL injection. First I tried inputing a single quote, ' into the input field of the status page's form. This did not return a SQL error, but instead the same error page that contained the NetWars flag like the image below. 

![396615979-1022f923-3a7d-4948-aef3-44ad1b53b508](https://github.com/user-attachments/assets/2d9af7f0-f8e7-4196-a70f-13d0470e8610)


Next on the list of answer options to test is command injection. I know that there are many ways to attempt command stacking, mainly by putting a ;, &&, and || in between two commands. I tried multiple command stacking tests in the input field along with the whoami command. This is because the whoami command works on both Linux and Windows systems. I used a command that would work on both systems to make identifying the vulnerability easier. 
* 128 ; whoami
* 128 || whoami
* 128 && whoami

After trying the first two options in the bullet list above, I got the same error page as the screenshot above when I tried testing the form for an SQL injection vulnerability. However, when I tried the last option in the bullet list, I noticed the output was different. The whoami command ran successfully.

![396616531-aaac689c-b21f-4bc5-a893-7e589b7c52cc](https://github.com/user-attachments/assets/e6c33e90-707e-4791-8718-fb7b8ee91970)


# Vulnerability Exploitation

* Challenge - Use the vulnerability identified in the check submission functionality of the songs.issplaylist.com site to identify the flag in the web root directory c:\InetPub\songs.issplaylist.com\wwwroot.
* Points - 5

The general formula to target the page's vulnerability is typing 128 && followed by a Windows command, this challenge is about crafting the right command. I know the target is a Windows machine because of the file path in the question. 

On Windows machines, dir can be used to display directory contents. I used the dir command to find the filename associated with the flag in the web root directory by typing the below input into the field:
* 128 && dir c:\InetPub\songs.issplaylist.com\wwwroot

![Vulnerability Exploitation-1](https://github.com/user-attachments/assets/c9f131a4-2b24-45b7-b67b-1496f9d39a75)

The output of the attack confirms the presence of F-L-A-G.txt in the web root directory, wwwroot. Now that I know what file I'm looking for and the location, it's time to craft the right command to display the flag file's content. The Windows type command displays the contents of the specified text file. I used the following command:
* 128 && type c:\InetPub\songs.issplaylist.com\wwwroot\F-L-A-G.txt

![396619438-a8f9d367-381d-4049-9ab4-06b35863b21f](https://github.com/user-attachments/assets/c119b2be-1441-4dd8-8021-bc2aabee8cdf)

The attack works successfully and I'm able to see the output of the F-L-A-G.txt file. That output is the answer to this challenge. 


# Windows Version Enumeration

* Challenge - What version of Windows Server is used for the songs.issplaylist.com server?
* Points - 2

From the previous challenges, I know the following information:
* The songs.issplaylist.com target is vulnerable to an attack at the status page.
* The songs.issplaylist.com target is a Windows sserver system.

Windows has many built-in utilities for system information enumeration. One of them is the ver tool, which I used in an attack to get the version of Windows server. I entered the below command into the input field:
* 128 && ver

This returned the version number. 

![396621435-1f55c13d-dae6-41e1-8667-9c73acecd0e8](https://github.com/user-attachments/assets/b7b48be8-32d3-43a6-a80c-2799e232d272)


# Privilege Evaluation

* Challenge - Evaluate the privileges available to the attacker by exploiting this vulnerability. Which user account is assigned to the web server process?
* Points - 2

The whoami command shows user, group, and privilege information for the user logged onto the system. In this case, using it against the target server will display the user account assigned to the web server process. 

The output to whoami is in the general format:
* DomainName\username

I used a attack against the server with whoami by typing the following into the input field:
* 128 && whoami

The output shows the account that is assigned to the web server process. 

![396625298-bafd72b5-7d98-4636-b788-9b2a4b5b0823](https://github.com/user-attachments/assets/b5077479-b41b-4160-9bfc-c6683a550d65)


# Privileged Assessment

* Challenge - Enumerate the SMB shares on the songs.issplaylist.com server. Identify the name of the non-hidden share.
* Points - 3

The Windows net use command displays current and active shares. First I tried inputing the following command injection into the input field:
* 128 && net view /all \\songs.issplaylist.com

![396627601-4cf3a8d4-3cb4-47f3-986a-54bf53a67352](https://github.com/user-attachments/assets/feabc52a-fa49-48cb-9f8f-c4a027742008)

This did not show me the output I wanted. After some troubleshooting, I figured out that to use the net use command to list all of the shares, I needed to specify the server IP after //. 

At my attacker terminal, I ran the ping command against songs.issplaylist.com to get the IP of the server. 

![Privileged Assessment-2](https://github.com/user-attachments/assets/a8ed586a-7aed-43f8-86a6-071b7b91f9d8)

Now that I have the IP, I tried using the same net use command as above, but this time putting //IP instead of //Hostname. I entered the following command: 
* 128 && net view /all \\10.142.146.70

![396628727-b8a309e2-cd4a-402b-8347-05a8cb24e983](https://github.com/user-attachments/assets/efc83ed1-004a-4366-a362-957a446c54e2)

Hidden shares end in $, so the share not ending in $ is the non-hidden share and the answer to this challenge. 


# Access Share

* Challenge - Access the non-hidden share on the songs.issplaylist.com server. Retrieve the flag.
* Points - 6

To access the non-hidden share, I have two main tools, Rpcclient and Smbclient. Rpcclient is used to gather target configuration details. Smbclient is used to interact with shares, to enumerate, download and upload files. To use either of these tools, I need valid credentials

Re-using the command injection vulnerability, I create a new user account and add it to the administrators group with two commands. First, I input this command into the input field:
* 128 && net user /add Rockman Password1

![396629793-bdc4ff92-0352-4510-b2ed-ba81611bafbb](https://github.com/user-attachments/assets/0b5a25d5-8e92-4371-891b-40c54609f815)


The command output confirms that my user Rockman was added. Next I added my newly created user account to the administrators group with the following command injection: 
* 128 && net localgroup administrators /add Rockman

![396630275-deebe007-cb8f-4bb8-9212-e4fa076211cd](https://github.com/user-attachments/assets/8cecf12f-fa90-4fd6-b0f8-499a65fa09e7)

The command output confirmed that Rockman was added to the administrators group. Now it's time to go to my attacker terminal and use Smbclient to download the flag file from the share. When I typed the below command and entered my password, I was granted with an SMB prompt, and a connection to the non-hidden share specified in the command (syntax is //<domain_name>/<share_name>).

![396630916-de00efe1-5994-4d98-b8d9-206a4d048dd7](https://github.com/user-attachments/assets/2931788f-17d9-4557-ac3d-db4c034e8f3e)

Now that I'm connected to the share it's time to enumerate the share's contents. With Smbclient, this can be done with ls. I typed ls at the SMB prompt to enumerate the share's files.  

![Access Share-4](https://github.com/user-attachments/assets/c453e9bf-e0a4-4006-9895-4407e95ba2fe)

Among the files in the share is flag.txt, which contains the answer to this challenge. With Smbclient, you can get followed by the filename to download the SMB share's file to your local system. I do this for flag.txt to download the file to my Linux VM, close the SMB connection, confirm the flag.txt file was download, and show its output to find the flag. 

![396631885-dfbaec17-acde-4350-9826-f9005e29b40c](https://github.com/user-attachments/assets/62d80b65-578d-4c39-971d-8af1255a476e)


# Always Be Cracking

* Challenge - Use your credentials to access the songs.issplaylist.com server. Submit the NT password hash value for the Administrator account.
* Points - 5

For this challenge, I'll be using Metasploit with the windows/smb/psexec module against the target system. 

![Always Be Cracking-1](https://github.com/user-attachments/assets/d93ae1d0-e1a0-48a2-a347-0e1ad8461b03)

I'll keep the default payload the same. After viewing the options for the module and the payload, it's time to set them. 

![Always Be Cracking-2](https://github.com/user-attachments/assets/f7af6b25-6618-4ee3-8169-66281e7c2af3)

The SMB options are for the user and password to login as when the exploit runs successfully. RHOST is the target system, songs.issplaylist.com, and I got the target's IP by running "ping songs.issplaylist.com" in a new terminal window. LHOST is the IP of my system. With a reverse shell, the victim connects to the attacker, so the attacking machine must be ready to listen and receive that connection. I set these options and confirm them by running "show options" at the msfconsole prompt. 

![Always Be Cracking-3](https://github.com/user-attachments/assets/199652a5-0e20-4197-a4e5-9bc718b90c27)

Now that the options are set, I type "exploit" to send the exploit to the target system. It works and I gain a Meterpreter shell. 

After successfully gaining a reverse shell with Meterpreter against the target sytem. It's time to migrate to the LSASS process, and then run the hashdump command. The hashdump command fails, because it's a privileged operation. To fix the privilege issue, I migrated to the lsass.exe process. After this, I ran hashdump again and was able to get the password hashes for several users. 

![396669565-85e8dd7c-e50a-43c9-8732-68c23f788ed2](https://github.com/user-attachments/assets/94a9e021-4bd1-49ed-9c68-635fa1566d7d)

Hashdump is used to dump the contents of the SAM database on a Windows system. The SAM database stores user password information, similar to how /etc/shadow on Linux stores password hashes. Now that I have the password hashes, I copied the hashes to my clipboard. Next, I typed "exit" at the Meterpreter prompt to drop my reverse shell connection to the target system. Lastly, I typed "exit" to exit the Metasploit console and return to the Linux termal. From there I used Gedit to create a new file, paste the hashes into it, save the file, then close it. 

![396670563-c513bb0e-864d-42da-a8ec-6aaa637f3207](https://github.com/user-attachments/assets/ca5c0e42-caef-428c-9255-5ef1c8e168f3)

Now that I have the hashfile, it's time to look for the NT hash for the Administrator account. The screenshot above shows the hashes, and the NT password hash is the rightmost section of the colon separated fields from hashdump’s output. The NT hash for the Administrator account is the bc50ab76b6db99f148629a76bf0766c4. 


# Always Be Cracking (2)

* Challenge - Submit the Administrator account plaintext password.
* Points - 5

With the hash file, I use Hashcat to crack the recovered password hashes. Note that the screenshot output is not the full output.

![Always Be Cracking (2)-1](https://github.com/user-attachments/assets/3fad03c7-b492-4684-b618-f7546a8ba492)

With Hashcat, you can specify the following generic format to view already cracked passwords and their associated usernames for a hashfile:
* hashcat hashfile_name --show --user

I used these arguments for the songs-hashes.txt file. The password for the Administrator account was successfully cracked. 

![396672664-05da8276-2165-448c-b9e2-7168d58b3018](https://github.com/user-attachments/assets/b96de60c-8e3f-42b7-b903-bc2ac84fb320)
