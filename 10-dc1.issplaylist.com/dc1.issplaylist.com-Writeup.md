# Exploitation

* Challenge - Gain access to the dc1.issplaylist.com server. Submit the flag in C:\flag1.txt.
* Points - 10
* Answer - NetWars{PrettyCloseNow}

The dc1 server isn’t accessible directly from your attacker system. I must use Metasploit to exploit a previously compromised system, then pivot to dc1 from there. From previous challenges and compromises, the non-hidden share in the 9-stor.issplaylist.com series of challenges is vulnerable to SMB attacks. I will Metasploit with the psexec exploit to attack this server’s SMB functionality. I start Metasploit, load the exploit, configure the options, and send the exploit to get a reverse TCP shell on the server with Meterpreter. 

![397445591-6b37e190-bf87-43a5-8fba-afd4f5abb9c0](https://github.com/user-attachments/assets/90b9989c-2041-420b-9ce1-e9c5ddcf009e)

![397446205-aec023f1-51b3-4a5c-80d5-925030f4e41a](https://github.com/user-attachments/assets/0effe357-ac01-4470-900d-2f34ca99686a)

Now that I have Meterpreter access to a system inside the network, I can pivot to the DC1. But first, I need the IP of the dc1 system. In a new terminal, I run "ping dc1.issplaylist.com" to get the DC1's IP, 10.142.145.150.

I run "background" to send my current Meterpreter session to the background so I can interact with the Metasploit console again. I type "sessions" to view the session number associated with my Meterpreter session. I must use the existing Meterpreter session with the portfwd or route command to pivot from inside the network to the dc1 server. I add the route and confirm that it has been defined. 

![Exploitation-3](https://github.com/user-attachments/assets/6b43657c-48cf-4647-92a0-87c304f008cd)

Now that the route is added, I have to change the RHOST to the IP address of the dc1 system. After I change the RHOST option, I send the exploit. After running "exploit" I am successfully able to get a Meterpreter shell against the dc1 system.

![Exploitation-4](https://github.com/user-attachments/assets/d1f286bc-eae3-415d-aa55-8f7195961b02)

In Meterpreter, I get a system shell, then access to PowerShell. The file path for the flag is C:\flag1.txt, so I use the type command to display the contents of the file. 

![Exploitation-5](https://github.com/user-attachments/assets/fe946e37-7a92-485e-96c9-650c72d6e1bf)


# Windows Persistence

* Challenge - Use your access to dc1.issplaylist.com to identify a second flag associated with a persistence method.
* Points - 7
* Answer - NetWars{PersistentAccess}

There are many ways to establish persistence on a Windows system. Some examples include using processes, backdoors, services, account creation, and scheduled tasks. With PowerShell, I use the Get-Service cmdlet to interrogate the services running on the system. I find the flag in the output of Get-Service.

![Windows Persistence-1](https://github.com/user-attachments/assets/e004ec53-3000-4b66-a8d4-06da66614920)


# Suspicious User

* Challenge - Which user that is not part of the ISS Playlist Information Technology Team has logged on to the domain controller? Submit the username.
* Points - 3
* Answer - prosa

The C:\Users directory on Windows contains information about the users who have accessed the system recently. I cd to that directory, and use the dir command to display the directory contents. The prosa account seems suspicious, since only Administrators should log onto the domain controller. 

![Suspicious User-1](https://github.com/user-attachments/assets/1afc2f60-ff63-48e7-a7ad-92931701493c)

To give prosa the benefit of the doubt, I will check to see if they are part of the ISS Playlist Information Technology Team. I visit the company page at the site for ISS Playlist, which shows the name, email, and role for the employees of ISS playlist. From a previous challenge, I know that the user format is first initial followed by the last name. 

![Suspicious User-2](https://github.com/user-attachments/assets/faee73e6-de78-4ee7-8451-bb00f2247b1e)

So Phyliss Rosa is prosa, which is the director of sales. It doesn’t make sense that someone in this role would be accessing the dc1 system. Therefore, prosa is the answer for this challenge.  

# Password Hash Retrieval

* Challenge - Retrieve password hashes for the IP domain. Submit the NT password hash for the prosa account.
* Points - 10 
* Answer - 9d5c7d52a9fe6adad03db7a4613937d6

From the PowerShell prompt I have on the dc1 system, I type exit to return to the Windows command prompt. From there, I type exit again to return to the Meterpreter prompt. Meterpreter supports the hashdump command, which will allow me to dump the password hashes for this Windows domain controller system. If you try to run the hashdump command right away in Meterpreter, you will get an operation failed error. 

![Password Hash Retrieval-1](https://github.com/user-attachments/assets/93b844d7-ea86-4682-8dc4-d6568f38ef3e)

To get around this, I have to move the Meterpreter shell from the initial process to one running inside of lsass.exe. This will fix the permission issue. After migrating to lsass.exe, I ran the hashdump command again to obtain the hashes for the domain controller. It works, and the NT hash is the right most string in the colon separated fields.

![Password Hash Retrieval-2](https://github.com/user-attachments/assets/bbfbfd80-50fa-4046-ac77-8ab64299b9da)


# Always Be Cracking

* Challenge - Submit the plaintext password for the prosa account.
* Points - 5
* Answer - Sharks21

Now that I have obtained the password hashes for the IP domain, I copy them into my clipboard. I exit Meterpreter and Metasploit to return to my local terminal. From there, I create a new file with Gedit, paste the hashes, save the file, then close it. 

![Always Be Cracking-1](https://github.com/user-attachments/assets/cc1afe0a-c19b-486a-97ed-92209468593e)

Next, I use Hashcat to crack the passwords with a straight attack. 

![Always Be Cracking-2](https://github.com/user-attachments/assets/9a9a6acf-db5d-4d1d-b328-963129936abd)

After Hashcat finishes cracking the passwords, I run it again with --show and --user to display cracked passwords and their associated usernames. The password for the prosa user is Sharks21. 

![Always Be Cracking-3](https://github.com/user-attachments/assets/49a30eb8-921e-45b8-931f-c858fb30b3d2)
