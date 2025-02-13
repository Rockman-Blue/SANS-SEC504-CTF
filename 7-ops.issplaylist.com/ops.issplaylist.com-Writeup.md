# Port Scan

* Challenge - Which TCP ports are open on the ops.issplaylist.com server?
* Points - 2 

This question is multiple choice, so I limited my scan to only those ports. Nmap shows me the open ports. Use Nmap with sudo whenever possible for the best restuls. 

![397377818-4330b8ec-24c0-4643-b61a-b3cc31c9c2e4](https://github.com/user-attachments/assets/ad5c647e-ba57-4a9b-a4e9-5d59462ba23a)

Nmap shows me that there are three open ports

# Service Interaction

* Challenge - Access the service running on TCP/10000. Submit the flag.
* Points - 2

Initially, I tried connecting to the open port with Netcat. This didn't work out, but then I remembered that you can use a web browser to connect to a port on the webserver by specifying a colon, followed by the port number. 

![397378800-8ba8b0ec-afe1-4ba5-9265-c9490ecaeef5](https://github.com/user-attachments/assets/920f4f7e-3f67-4d9d-91b0-1bd1a199bd01)

After typing in the URL:portnumber for the server, the flag is displayed on the page. 

# Vulnerability Exploitation

* Challenge - Exploit the vulnerability associated with the Webmin service on the ops.issplaylist.com site. Retrieve the flag value stored in the /flag.txt file.
* Points - 8

For this challenge, I will use Metasploit. First I started the console, searched for Webmin eploits, and loaded one to use. I picked #0 because it seemed the simplest to use from reading its description compared to the other ones. 

![Vulnerability Exploitation-1](https://github.com/user-attachments/assets/c77854cd-2d87-47fb-addc-0e3daca07158)

Now that the exploit is loaded, it is time to show and set the options so that the exploit runs successfully. The main options to worry about are RHOSTS (the target) and LHOST (my attacking system). 

![Vulnerability Exploitation-2](https://github.com/user-attachments/assets/46e520b0-5b79-43c5-a187-aa3e50fe54d4)

Now that my options are set, it's time to exploit the vulnerability. I send the exploit and it works successfully. I display the contents of the /flag.txt file to get the answer to this challenge. 

![397382776-e3e6c9eb-0264-411c-96e4-a812e5e35bdb](https://github.com/user-attachments/assets/d79d917b-0a8a-4ee8-9c8a-ee16332de6b0)


# Webmin Password File

* Challenge - Use your access to obtain the Webmin local user authentication file. Submit the flag.
* Points - 5

Using the same access gained through the exploit used in the challenge above, it's time to look for the webmin password file. After trying to dig around in the filesystem to find the file, I couldn't find it on my own. 

Next, I went to ClippedBin and searched for "webmin" in the recent pastes page. Whenever I get stuck on a particular challenge, I search the keywords of that challenge in ClippedBin to find OSINT information to help me. 

![Webmin Password File-1](https://github.com/user-attachments/assets/f7047b1e-79c7-4493-b9eb-8af35c62e7e4)

The ClippedBin search reveals two pastes. After looking at both, one of them tells me the location of the webmin password file. 

![Webmin Password File-2](https://github.com/user-attachments/assets/ad51d82d-ab72-4bd5-bfa6-c38ac02c9e93)

Now that I know the file location, it's as simple as running a cat command to obtain the file and the flag. 

![397384886-0c480331-f3d1-4366-ba0b-900a218316bd](https://github.com/user-attachments/assets/164e5b11-237c-465c-81ad-320b6536238a)


# Always Be Cracking

* Challenge - Recover the plaintext password for the Webmin admin user. Submit the plaintext password.
* Points - 5

I copied the admin hash from output of the command from the challenge above and opened a new terminal. On my terminal, I created a new file and pasted the hash with Gedit. 

![397385871-7fe96846-150a-41fc-9344-b268b132893e](https://github.com/user-attachments/assets/28481c4a-7338-41ff-aa10-398cb101712a)

I looked at the hashfile and noticed that both hashes started with $1, which indicates that this hash is an MD5 hash. With this knowledge, I ran Hashcat to crack the admin password using the command below
* hashcat -m 500 -a 0 ops-hash.txt /usr/share/wordlists/passwords.txt

After cracking the password, I modified my previous command to include --show and --user to show cracked passwords and their associated username.

![397387272-6510431a-cb66-4b85-977b-998415bb9854](https://github.com/user-attachments/assets/ae80d95d-7374-4aec-a201-693bc23d3a58)


# Credential Reuse

* Challenge - Login to the server over SSH. Submit the flag.
* Points - 6 

I know the credentials for the Webmin admin from previous challenges. With SSH, the generic command format is ssh username@domain/IP. I tried to SSH into the server with the admin credentials, but I got a permissioned denied error. I thought I typed in the password wrong, so I typed the password more slowly twice and still got the same error.

![Credential Reuse-1](https://github.com/user-attachments/assets/41cbf1c5-c91f-4b8f-8758-1a6f1ae66a65)

Instead of trying to SSH, I went to http://ops.issplaylist.com:10000/ and logged in with the admin credentials. I was able to successfully login to the Webmin admin interface. 

![Credential Reuse-2](https://github.com/user-attachments/assets/7db0322e-9ebb-4a7d-92bf-da28face4aae)

From there I clicked on System in the left side pane, then Users and Groups. From there I found a new user to login over SSH. The username is Hacker1, but the password is hashed. 

![Credential Reuse-3](https://github.com/user-attachments/assets/72c4091c-7842-4c92-a0cb-2278e2d8bc85)

I return to my terminal to create a new file, paste the hashed password, then save and exit using Gedit. After creating the hashfile that contains the hashed password for the Hacker1 user, I cracked it with Hashcat. 

![Credential Reuse-4](https://github.com/user-attachments/assets/d5ec4495-49a7-4c53-9e64-bf824b6161c0)

Now that I have a user on the system to SSH with, Hacker1/Password. I ran “ssh Hacker1@ops.issplaylist.com” and entered the password. Once I logged in, I got the flag.

![397402175-249ca5d6-c7f9-4672-ada5-5b4afb572599](https://github.com/user-attachments/assets/ecb0e81a-c91c-41d8-ad13-d2adbdf6a0ac)

# Post-Exploitation Credential Gathering

* Challenge - Submit the password for the ftpacctng user accessing the system.
* Points - 9 
* Answer - D4taD0wnload

From the Webmin server running at http://ops.issplaylist.com:10000, I found the password hash of the ftpacctng user by going to System > Users and Groups > ftpacctng.

![Post-Exploitation Credential Gathering-1](https://github.com/user-attachments/assets/87d548c4-a83f-49f4-8039-cb0890e9b81d)

I copy pasted the line with the ftpacctng user’s password into hashfile.txt and tried to use Hashcat to crack it. The $1 in the hash, which indicates that this hash is MD5. 

![Post-Exploitation Credential Gathering-2](https://github.com/user-attachments/assets/cbd6228b-4351-4a2a-b0f8-14bb44162e5d)

I kept getting the “exhausted” status, even when trying using rules with the hashcat command. This dead end made me think about the user name, and how it likely supports login over FTP. The server that I am trying to access runs FTP, and FTP is a plain-text protocol. Because of this, I can sniff-out the authentication credentials and capture them when the ftpacctng user logs in. 

Using the server access obtained from the previous challenge, I tried to use Tcpdump to capture the authentication credentials on the server with Tcpdump. I got a permission denied error, so I typed "exit" at my SSH prompt to drop the connection. 

![Post-Exploitation Credential Gathering-3](https://github.com/user-attachments/assets/60044d85-c98a-4a7f-8e25-bbc79fdd134f)

One of the hints said I need root access, and that I should obtain it through previous attack mechanisms launched against this target. To obtain root access, I went back to the Webmin admin interface. From there I went to System > Users and Groups. I clicked on Hacker1, since that is the user I used to login to the server over SSH. From this page, I changed the Hacker1 user's primary group from users to root. I also changed the User ID to 0 to match the root user. After this I hit save at the bottom of the screen. 

![Post-Exploitation Credential Gathering-4](https://github.com/user-attachments/assets/f6da82ee-85d8-4120-a1bc-a4ea40d5055e)

Now it's time to test that my privilege escalation attack worked. I SSH into the server again, and noticed that my prompt changed to #, the root prompt. I confirm that I am root by running whoami.

![Post-Exploitation Credential Gathering-5](https://github.com/user-attachments/assets/c301dd30-9df3-4248-9191-f6d5260c0df7)

Now that I have root access, I can run Tcpdump to start capturing traffic on the target system. After running Tcpdump for a few minutes, I pressed CTRL + C to stop capturing traffic. 

![Post-Exploitation Credential Gathering-6](https://github.com/user-attachments/assets/b1aef36e-cd6d-45e5-b30a-b95c618f2299)

Now I used Tcpdump with the -r argument to read from the pcap file and search for the password with Grep. 

![Post-Exploitation Credential Gathering-7](https://github.com/user-attachments/assets/f5a04e00-6fc1-47e9-98f0-d675b3ef1fa0)
