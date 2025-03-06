# Exploitation

* Challenge - Gain access to the dc1.issplaylist.com server. Submit the flag in C:\flag1.txt.
* Points - 10

The dc1 server isn’t accessible directly from your attacker system. I must use Metasploit to exploit a previously compromised system, then pivot to dc1 from there. From previous challenges and compromises, the non-hidden share in the 9-stor.issplaylist.com series of challenges is vulnerable to SMB attacks. I will Metasploit with the psexec exploit to attack this server’s SMB functionality. I start Metasploit, load the exploit, configure the options, and send the exploit to get a reverse TCP shell on the server with Meterpreter. 

![Exploitation-1](https://github.com/user-attachments/assets/8611e8a7-4948-41fa-9a88-fd50ef0b087a)

![Exploitation-2](https://github.com/user-attachments/assets/68d1d29f-950e-4dfe-85ff-696a0b67f8fa)

Now that I have Meterpreter access to a system inside the network, I can pivot to the DC1. But first, I need the IP of the dc1 system. In a new terminal, I run "ping dc1.issplaylist.com" to get the DC1's IP, 10.142.145.150.

I run "background" to send my current Meterpreter session to the background so I can interact with the Metasploit console again. I type "sessions" to view the session number associated with my Meterpreter session. I must use the existing Meterpreter session with the portfwd or route command to pivot from inside the network to the dc1 server. I add the route and confirm that it has been defined. 

![Exploitation-3](https://github.com/user-attachments/assets/62f3aa3e-ae42-4df6-bd16-85d3e58be38a)

Now that the route is added, I have to change the RHOST to the IP address of the dc1 system. After I change the RHOST option, I send the exploit. After running "exploit" I am successfully able to get a Meterpreter shell against the dc1 system.

![Exploitation-4](https://github.com/user-attachments/assets/bff323bb-95d3-419c-a67d-e96e799a8502)

In Meterpreter, I get a system shell, then access to PowerShell. The file path for the flag is C:\flag1.txt, so I use the type command to display the contents of the file. 

![Exploitation-5](https://github.com/user-attachments/assets/c17c6c20-e1a8-4857-9309-31818c1b642d)


# Windows Persistence

* Challenge - Use your access to dc1.issplaylist.com to identify a second flag associated with a persistence method.
* Points - 7

There are many ways to establish persistence on a Windows system. Some examples include using processes, backdoors, services, account creation, and scheduled tasks. With PowerShell, I use the Get-Service cmdlet to interrogate the services running on the system. I find the flag in the output of Get-Service.

![Windows-Persistence-1](https://github.com/user-attachments/assets/abafa2f0-8380-478f-b0a0-45516c2ae8ad)


# Suspicious User

* Challenge - Which user that is not part of the ISS Playlist Information Technology Team has logged on to the domain controller? Submit the username.
* Points - 3

The C:\Users directory on Windows contains information about the users who have accessed the system recently. I cd to that directory, and use the dir command to display the directory contents. I see an account that follows the ISS Playlist username convention found earlier in the CTF.  

![Suspicious-User-1](https://github.com/user-attachments/assets/72653a40-3d6b-4e2d-9610-43816834902f)

Only administrators should log onto the domain controller, so I will check if the user is an admin in the IT team. I visit the company page at the site for ISS Playlist, which shows the name, email, and role for the employees of ISS playlist. From a previous challenge, I know that the username format - so finding the employee name from there is easy.  

![Suspicious-User-2](https://github.com/user-attachments/assets/1d65fa7f-5dc5-4167-ae7d-88f4de508460)

The user shown in C:\Users directory for this domain controller is the director of sales. It doesn’t make sense that someone in this role would be accessing the dc1 system. Therefore, that user is the answer for this challenge.  

# Password Hash Retrieval

* Challenge - Retrieve password hashes for the IP domain. Submit the NT password hash for the prosa account.
* Points - 10 

From the PowerShell prompt I have on the dc1 system, I type exit to return to the Windows command prompt. From there, I type exit again to return to the Meterpreter prompt. Meterpreter supports the hashdump command, which will allow me to dump the password hashes for this Windows domain controller system. If you try to run the hashdump command right away in Meterpreter, you will get an operation failed error. 

![Password-Hash-Retrieval-1](https://github.com/user-attachments/assets/ae922b38-bc4d-4012-b3f9-99bdfe4a7fa6)

To get around this, I have to migrate the Meterpreter shell from the initial process to one running inside of lsass.exe. This will fix the permission issue. After migrating to lsass.exe, I ran the hashdump command again to obtain the hashes for the domain controller. It works, and the NT hash is the right most string in the colon separated fields.

![Password-Hash-Retrieval-2](https://github.com/user-attachments/assets/2d6aff9c-b134-406d-bd27-ed3e118d5092)


# Always Be Cracking

* Challenge - Submit the plaintext password for the prosa account.
* Points - 5

Now that I have obtained the password hashes for the IP domain, I copy them into my clipboard. I exit Meterpreter and Metasploit to return to my local terminal. From there, I create a new file with Gedit, paste the hashes, save the file, then close it. 

![Always-Be-Cracking-1](https://github.com/user-attachments/assets/f3fdd91c-5768-4ca0-9eb0-6029071fd719)

Next, I use Hashcat to crack the passwords with a straight attack. 

![Always-Be-Cracking-2](https://github.com/user-attachments/assets/0832ea0f-2534-4100-bf4f-01fa2d25bbd6)

After Hashcat finishes cracking the passwords, I run it again with --show and --user to display cracked passwords and their associated usernames to see the plaintext password for the prosa account. 

![Always-Be-Cracking-3](https://github.com/user-attachments/assets/d014f032-0eae-49d6-b15b-ab0faeaf2abe)
