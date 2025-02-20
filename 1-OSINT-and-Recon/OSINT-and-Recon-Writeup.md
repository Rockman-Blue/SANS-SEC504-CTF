# Website Reconnaissance

* Challenge - What is ISS Employee Jerald Orestes' title?
* Points - 1

This challenge tests basic website reconnaissance. The company name is ISS Playlist, so I navigated to their site at issplaylist.com. From there I go to the company page using the navbar at the top. Then I used CTRL + F to search for Jerald Orestes. From there, I saw his job title underneath his name.

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

I navigate to clippedbin.com. In the CTF introduction, SANS tells me that the flag answers are in the format of NetWars{FlagValue}. So After going to clippedbin.com, I went to the Trending page, and searched for "NetWars". This returned a paste that has the flag value.  

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

Regarding DNS reconnaissance, the biggest vulnerability a DNS server can have is a zone transfer request. Transferring DNS records and zone files data from the primary server to the secondary server is done with a zone transfer. This is so admins can have the secondary server be a backup to the primary for redundancy.By using a zone transfer request, you can get lots of valuable information about your target. Dig is a common tool used for DNS reconnaissance, and you can specify the record type AXFR to attempt a zone transfer request. At my attacker terminal, I ran the following command to perform a zone transfer request against the target, which displayed the flag. 

![DNS-Reconnaissance-1](https://github.com/user-attachments/assets/41aa27bd-6a04-41f6-884c-11d34ac93270)


# IP Camera Target

* Challenge - What is the IPv4 address of the camera target?
* Points - 1

From the same output of the dig command that performs a zone transfer, we can see the IP of the host "cam.issplaylist.com". Cam is for the camera target. 

![396193111-3f8a0591-ff61-47d9-ad03-60ac8a6ae1ed](https://github.com/user-attachments/assets/0d4c7f84-3e8f-473a-9c63-4ea225064968)


# IP Camera Logon

* Challenge - Access the IP camera logon page. Submit the flag.
* Points - 1

I already know the hostname for the camera target from the output of the above screenshot. In Firefox, I type "cam.issplaylist.com" in the search bar. The flag is displayed at the bottom of the page. 

![396193111-3f8a0591-ff61-47d9-ad03-60ac8a6ae1ed](https://github.com/user-attachments/assets/3d3f72df-3091-4b8c-a74f-6a09711e004c)


# IP Camera Access

* Challenge - Gain access to the IP camera system through the web interface logon page. Submit the flag displayed after logging in to the system.
* Points - 4

From the previous challenges in this category/section, I know that I should use ClippedBin for OSINT against the target. So I go to clippedbin.com, and then the recent page. From there, I search for the hostname "cam.issplaylist.com" and find a paste that contains the credentials for this target.

![IP Camera Access-2](https://github.com/user-attachments/assets/819e4ec0-ae38-44d9-b130-3e629c540cd4)
![IP Camera Access-3](https://github.com/user-attachments/assets/8a32e139-837b-4500-a028-855187409f61)

Going back to the host at cam.issplaylist.com, use the credentials from the paste - admin/admin to login. After logging in, the flag is displayed at the bottom of the page. 

![396193111-3f8a0591-ff61-47d9-ad03-60ac8a6ae1ed](https://github.com/user-attachments/assets/de9280cf-6126-4796-b062-2676914b7e06)


# IP Camera Disclosure

* Challenge - Who does Bridget need to follow up with?
* Points - 2

Once logged in at the cam.issplaylist.com page with admin/admin, use the camera controls on the page to look at the board for the answer. 

![396193111-3f8a0591-ff61-47d9-ad03-60ac8a6ae1ed](https://github.com/user-attachments/assets/f703841c-e64a-4c10-9490-8b0229ca217f)
