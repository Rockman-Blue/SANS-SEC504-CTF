# Port Scan

* Challenge - Which TCP ports are open on the ops.issplaylist.com server?
* Points - 2 

This question is multiple choice, so I limited my scan to only those ports. Nmap shows me the open ports. Use Nmap with sudo whenever possible for the best restuls. 

![Port-Scan-1](https://github.com/user-attachments/assets/2ddf01fd-2fa9-4b4c-bca6-25959a0a29db)

Nmap shows me that there are three open ports

# Service Interaction

* Challenge - Access the service running on TCP/10000. Submit the flag.
* Points - 2

Initially, I tried connecting to the open port with Netcat. This didn't work out, but then I remembered that you can use a web browser to connect to a port on the webserver by specifying a colon, followed by the port number. 

![Service-Interaction-1](https://github.com/user-attachments/assets/a9fe70c6-602c-40dc-93b6-f5d3de2ddae8)

After typing in the URL:portnumber for the server, the flag is displayed on the page. 

# Vulnerability Exploitation

* Challenge - Exploit the vulnerability associated with the Webmin service on the ops.issplaylist.com site. Retrieve the flag value stored in the /flag.txt file.
* Points - 8

For this challenge, I will use Metasploit. First I started the console, searched for Webmin eploits, and loaded one to use. I picked #0 because it seemed the simplest to use from reading its description compared to the other ones. 

![Vulnerability-Exploitation-1](https://github.com/user-attachments/assets/71f87631-70f2-4d22-b0e0-5326f2e943a5)

Now that the exploit is loaded, it is time to show and set the options so that the exploit runs successfully. The main options to worry about are RHOSTS (the target) and LHOST (my attacking system). 

![Vulnerability-Exploitation-2](https://github.com/user-attachments/assets/44faa2dc-65ab-4f0a-bd01-ee7769fccbb1)

Now that my options are set, it's time to exploit the vulnerability. I send the exploit and it works successfully. I display the contents of the /flag.txt file to get the answer to this challenge. 

![Vulnerability-Exploitation-3](https://github.com/user-attachments/assets/7e3f4948-e290-4bf5-8d68-09ce3afde57d)


# Webmin Password File

* Challenge - Use your access to obtain the Webmin local user authentication file. Submit the flag.
* Points - 5

Using the same access gained through the exploit used in the challenge above, it's time to look for the webmin password file. After trying to dig around in the filesystem to find the file, I couldn't find it on my own. 

Next, I went to ClippedBin and searched for "webmin" in the recent pastes page. Whenever I get stuck on a particular challenge, I search the keywords of that challenge in ClippedBin to find OSINT information to help me. 

![Webmin-Password-File-1](https://github.com/user-attachments/assets/c782ebe6-625f-46ab-afc7-da4ed944824b)

The ClippedBin search reveals two pastes. After looking at both, one of them tells me the location of the webmin password file. 

![Webmin-Password-File-2](https://github.com/user-attachments/assets/460fee1f-3b86-4cd6-9faf-6dfce1122851)

Now that I know the file location, it's as simple as running a cat command to obtain the file and the flag. 

![Webmin-Password-File-3](https://github.com/user-attachments/assets/b9ab718d-295c-43e0-bca7-2d5a1359dc06)


# Always Be Cracking

* Challenge - Recover the plaintext password for the Webmin admin user. Submit the plaintext password.
* Points - 5

I copied the admin hash from output of the command from the challenge above and opened a new terminal. On my terminal, I created a new file and pasted the hash with Gedit. 

![Always-Be-Cracking-1](https://github.com/user-attachments/assets/4f9c57fb-1a70-4bc1-bceb-b29ca79bf139)

I looked at the hashfile and noticed that both hashes started with $1, which indicates that this hash is an MD5 hash. With this knowledge, I ran Hashcat to crack the admin password using the command below
* hashcat -m 500 -a 0 ops-hash.txt /usr/share/wordlists/passwords.txt

After cracking the password, I modified my previous command to include --show and --user to show cracked passwords and their associated username.

![Always-Be-Cracking-2](https://github.com/user-attachments/assets/dd110034-2449-44b9-8a5a-ce714eeeda42)


# Credential Reuse

* Challenge - Login to the server over SSH. Submit the flag.
* Points - 6 

I know the credentials for the Webmin admin from previous challenges. With SSH, the generic command format is ssh username@domain/IP. I tried to SSH into the server with the admin credentials, but I got a permissioned denied error. I thought I typed in the password wrong, so I typed the password more slowly twice and still got the same error.

![Credential-Reuse-1](https://github.com/user-attachments/assets/ff84a533-d7c4-46c4-a7c0-a09570c0ed5f)

Instead of trying to SSH, I went to http://ops.issplaylist.com:10000/ and logged in with the admin credentials. I was able to successfully login to the Webmin admin interface. 

![Credential-Reuse-2](https://github.com/user-attachments/assets/fcefac44-5f56-42be-b09a-994110133893)

From there I clicked on System in the left side pane, then Users and Groups. From there I found a new user to login over SSH. The username is Hacker1, but the password is hashed. 

![Credential-Reuse-3](https://github.com/user-attachments/assets/297f4edf-ae7c-4519-a844-7d2ebacbb037)

I return to my terminal to create a new file, paste the hashed password, then save and exit using Gedit. After creating the hashfile that contains the hashed password for the Hacker1 user, I cracked it with Hashcat. 

![Credential-Reuse-4](https://github.com/user-attachments/assets/2f380bde-0c86-40f0-bf60-b451db1a2c66)

Now that I have a user on the system to SSH with, Hacker1/Password. I ran “ssh Hacker1@ops.issplaylist.com” and entered the password. Once I logged in, I got the flag.

![Credential-Reuse-5](https://github.com/user-attachments/assets/badcab88-6156-47ce-a6d9-e1916995d10e)


# Post-Exploitation Credential Gathering

* Challenge - Submit the password for the ftpacctng user accessing the system.
* Points - 9 

From the Webmin server running at http://ops.issplaylist.com:10000, I found the password hash of the ftpacctng user by going to System > Users and Groups > ftpacctng.

![Post-Exploitation-Credential-Gathering-1](https://github.com/user-attachments/assets/9e494ad6-053a-4642-935e-cebbfdd89f1b)

I copy pasted the line with the ftpacctng user’s password into hashfile.txt and tried to use Hashcat to crack it. The $1 in the hash, which indicates that this hash is MD5. 

![Post-Exploitation-Credential-Gathering-2](https://github.com/user-attachments/assets/9d68717a-1f7e-4b2b-ac22-686f6502ef42)

I kept getting the “exhausted” status, even when trying using rules with the hashcat command. This dead end made me think about the user name, and how it likely supports login over FTP. The server that I am trying to access runs FTP, and FTP is a plain-text protocol. Because of this, I can sniff-out the authentication credentials and capture them when the ftpacctng user logs in. 

Using the server access obtained from the previous challenge, I tried to use Tcpdump to capture the authentication credentials on the server with Tcpdump. I got a permission denied error, so I typed "exit" at my SSH prompt to drop the connection. 

![Post-Exploitation-Credential-Gathering-3](https://github.com/user-attachments/assets/71d23d8c-4410-4b0e-b425-ed9110906f0c)

One of the hints said I need root access, and that I should obtain it through previous attack mechanisms launched against this target. To obtain root access, I went back to the Webmin admin interface. From there I went to System > Users and Groups. I clicked on Hacker1, since that is the user I used to login to the server over SSH. From this page, I changed the Hacker1 user's primary group from users to root. I also changed the User ID to 0 to match the root user. After this I hit save at the bottom of the screen. 

![Post-Exploitation-Credential-Gathering-4](https://github.com/user-attachments/assets/753c006d-17bf-4d90-b740-14357496d357)

Now it's time to test that my privilege escalation attack worked. I SSH into the server again, and noticed that my prompt changed to #, the root prompt. I confirm that I am root by running whoami.

![Post-Exploitation-Credential-Gathering-5](https://github.com/user-attachments/assets/a427d093-6384-4921-9167-e5d89447d404)

Now that I have root access, I can run Tcpdump to start capturing traffic on the target system. After running Tcpdump for a few minutes, I pressed CTRL + C to stop capturing traffic. 

![Post-Exploitation-Credential-Gathering-6](https://github.com/user-attachments/assets/83430a34-cdec-4f35-8a0f-2179c77f50f1)

Now I used Tcpdump with the -r argument to read from the pcap file and search for the password with Grep. 

![Post-Exploitation-Credential-Gathering-7](https://github.com/user-attachments/assets/df2036f2-6cf5-4c7f-9de1-2c737b317594)
