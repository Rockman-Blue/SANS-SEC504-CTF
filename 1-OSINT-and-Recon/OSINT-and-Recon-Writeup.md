# Website Reconnaissance

* Challenge - What is ISS Employee Jerald Orestes' title?
* Points - 1

This challenge tests basic website reconnaissance. The company name is ISS Playlist, so I navigated to their site at issplaylist.com. From there I go to the company page using the navbar at the top. Then I used CTRL + F to search for Jerald Orestes. From there, I see his job title underneath his name.

![Website-Reconnaissance-1](https://github.com/user-attachments/assets/200c7ab1-5b53-4bdd-a36f-fa126b29ce1d)

![Website-Reconnaissance-2](https://github.com/user-attachments/assets/bfc453c4-6e8e-4e4a-8834-a3b4938742fe)


# Username Format Reconnaissance

* Challenge - What convention does ISS Playlist use for username creation?
* Points - 2

At the company page, I hover over the names to see the email and the convention for usernames. 

![Username-Format-Reconnaissance-1](https://github.com/user-attachments/assets/2cdb463c-d17b-4f5f-8ee0-7b6e29d79a1b)


# ClippedBin OSINT 1

* Challenge - Examine the website contents at http://www.clippedbin.com. Submit the flag.
* Points - 2

I navigate to clippedbin.com. In the CTF introduction, SANS tells me that the flag answers are in the format of NetWars{FlagValue}. So After going to clippedbin.com, I went to the Trending page, and searched for "NetWars". This returns a paste that has the flag value.  

![ClippedBin-OSINT-1-1](https://github.com/user-attachments/assets/ece7bc76-6f1c-4249-9b7a-9ba4a9f3c476)

![ClippedBin-OSINT-1-2](https://github.com/user-attachments/assets/fe32c4ba-43c9-4b0f-aa11-ecc607fe4661)


# ClippedBin OSINT 2

* Challenge - Use the http://www.clippedbin.com website to identify ISS Playlist password information disclosed by an attacker. There's a password repeated 4 times in the list. Submit the password as the answer.
* Points - 2

At the ClippedBin trending page, I search for "password". This returned three different pastes, after opening each one in a new tab, I found the paste that has the password information. The password in lines 5-8 are repeated, for a total of four repeated passwords.

![ClippedBin-OSINT-2-1](https://github.com/user-attachments/assets/2928e5e7-75df-40d2-b749-0b14b9e266f6)

![ClippedBin-OSINT-2-2](https://github.com/user-attachments/assets/5be2bf83-290d-42a5-9e17-bcc71ebb1434)


# DNS Reconnaissance

* Challenge - Use the vulnerability on the DNS server at ns1.sunsetisp.com to obtain information about the issplaylist.com domain. Submit the flag.
* Points - 3

Regarding DNS reconnaissance, the biggest vulnerability a DNS server can have is when an attacker can perform a zone transfer request. Zone transfers are used by admins to replicate DNS records and other data from the primary server to the secondary server. This is so the secondary server can be a backup to the primary to enforce redundancy. If you can successfully perform a zone transfer request, you can get lots of valuable information about your target. Dig is a common tool used for DNS reconnaissance, and you can specify the record type AXFR to attempt a zone transfer request. At my attacker terminal, I run the following command to perform a zone transfer request against the target, which displayed the flag. 

![DNS-Reconnaissance-1](https://github.com/user-attachments/assets/20e85917-b8f2-49e1-ae80-4f4ef2adb1e8)


# IP Camera Target

* Challenge - What is the IPv4 address of the camera target?
* Points - 1

From the same output of the Dig command that performs a zone transfer, I can see the IP of the host "cam.issplaylist.com". Cam is for the camera target. 

![IP-Camera-Target-1](https://github.com/user-attachments/assets/555539a1-6052-4198-859d-b432caae71d2)


# IP Camera Logon

* Challenge - Access the IP camera logon page. Submit the flag.
* Points - 1

I already know the hostname for the camera target from the output of the above screenshot. In Firefox, I type "cam.issplaylist.com" in the search bar. The flag is displayed at the bottom of the page. 

![IP-Camera-Logon-1](https://github.com/user-attachments/assets/9058d8a4-279b-495b-82ed-8d858c400203)


# IP Camera Access

* Challenge - Gain access to the IP camera system through the web interface logon page. Submit the flag displayed after logging in to the system.
* Points - 4

From the previous challenges in this section, I know that I should use ClippedBin for OSINT against the target. So I go to clippedbin.com, and then the recent page. From there, I search for the hostname "cam.issplaylist.com" and find a paste that contains the credentials for this target.

![IP-Camera Access-1](https://github.com/user-attachments/assets/3c67fa55-392d-4915-af6a-f1f015756c2d)

![IP-Camera-Access-2](https://github.com/user-attachments/assets/5d160609-dfc7-4175-8ed9-aed79d654b96)

Going back to the host at cam.issplaylist.com, I use the admin credentials from the paste to login. After logging in, the flag is displayed at the bottom of the page. 

![IP-Camera-Access-3](https://github.com/user-attachments/assets/9af0838e-90e7-420a-b99a-c87950197f07)


# IP Camera Disclosure

* Challenge - Who does Bridget need to follow up with?
* Points - 2

Once I'm logged in at the cam.issplaylist.com page with the admin credentials, I use the camera controls on the page to look at the board for the answer. 

![IP-Camera-Disclosure-1](https://github.com/user-attachments/assets/376c5bf3-a3a1-496f-bcc0-1e136f0a703c)
