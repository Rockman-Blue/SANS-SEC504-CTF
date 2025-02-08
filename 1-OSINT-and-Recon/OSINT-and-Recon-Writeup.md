# Website Reconnaissance

* Challenge - What is ISS Employee Jerald Orestes' title?
* Points - 1

This challenge tests basic website reconnaissance. The company name is ISS Playlist, so I navigated to their site at issplaylist.com. From there I go to the company page using the navbar at the top. Then I used CTRL + F to search for Jerald Orestes. From there, I saw his job title underneath his name.

![Screenshot 2024-12-16 115918](https://github.com/user-attachments/assets/2ef9c741-5967-4f1a-b366-8d83faab7f8a)

![396193111-3f8a0591-ff61-47d9-ad03-60ac8a6ae1ed](https://github.com/user-attachments/assets/b67cbe1b-d1ec-4c95-880b-78d5e03fa33b)


# Username Format Reconnaissance

* Challenge - What convention does ISS Playlist use for username creation?
* Points - 2

At the company page, hover over the names to see the email and the convention for usernames. 

![396193111-3f8a0591-ff61-47d9-ad03-60ac8a6ae1ed](https://github.com/user-attachments/assets/9cf47844-9b13-4f3a-8757-cbe5ef4b67cf)


# ClippedBin OSINT 1

* Challenge - Examine the website contents at http://www.clippedbin.com. Submit the flag.
* Points - 2

Navigate to clippedbin.com. In the CTF introduction, SANS tells you that the flag answers are in the format of NetWars{FlagValue}. So After going to clippedbin.com, I went to the Trending page, and searched for "NetWars". This returned a paste that has the flag value.  

![ClippedBinOSINT 1-2](https://github.com/user-attachments/assets/1204c6ad-56db-406a-8573-7541f6a05b09)
![396193111-3f8a0591-ff61-47d9-ad03-60ac8a6ae1ed](https://github.com/user-attachments/assets/b91fbe0d-5d3c-4503-b48f-3b157d1f2e82)


# ClippedBin OSINT 2

* Challenge - Use the http://www.clippedbin.com website to identify ISS Playlist password information disclosed by an attacker. There's a password repeated 4 times in the list. Submit the password as the answer.
* Points - 2

At the ClippedBin trending page, I searched for password. This returned three different pastes, after opening each one in a new tab, I found the paste that has the password information. The password in lines 5-8 are repeated, for a total of four repeated passwords.

![ClippedBin OSINT 2-2](https://github.com/user-attachments/assets/0ab1bb58-7c34-4caa-be25-655ef815b285)
![396193111-3f8a0591-ff61-47d9-ad03-60ac8a6ae1ed](https://github.com/user-attachments/assets/a64cc828-53c9-4d61-bdfe-72a8764e2e48)


# DNS Reconnaissance

* Challenge - Use the vulnerability on the DNS server at ns1.sunsetisp.com to obtain information about the issplaylist.com domain. Submit the flag.
* Points - 3
* Answer - NetWars{ReconRewards}

Regarding DNS reconnaissance, the biggest vulnerability a DNS server can have is a zone transfer request. By using a zone transfer request, you can get lots of valuable information about your target. Dig is a common tool used for DNS reconnaissance, and you can specify the record type AXFR to attempt a zone transfer request. At my attacker terminal, I ran the following command to perform a zone transfer request against the target, which displayed the flag. 

![DNS Reconnaissance-2](https://github.com/user-attachments/assets/1c2c2c9b-b9b0-48a8-8595-716de7b3abda)


# IP Camera Target

* Challenge - What is the IPv4 address of the camera target?
* Points - 1
* Answer - 10.142.145.170

From the same output of the dig command that performs a zone transfer, we can see the IP of the host "cam.issplaylist.com". Cam is for the camera target. 

![IP Camera Target-2](https://github.com/user-attachments/assets/209baa6f-f20a-4d3c-a5b1-e16914985d61)


# IP Camera Logon

* Challenge - Access the IP camera logon page. Submit the flag.
* Points - 1
* Answer - NetWars{WebCamLogin}

I already know the hostname for the camera target from the output of the above screenshot. In Firefox, I type "cam.issplaylist.com" in the search bar. The flag is displayed at the bottom of the page. 

![IP Camera Logon-2](https://github.com/user-attachments/assets/6f60a1ca-0b8b-4cb3-932b-55adb39c519a)


# IP Camera Access

* Challenge - Gain access to the IP camera system through the web interface logon page. Submit the flag displayed after logging in to the system.
* Points - 4
* Answer - NetWars{WebCamAccess}

From the previous challenges in this category/section, I know that I should use ClippedBin for OSINT against the target. So I go to clippedbin.com, and then the recent page. From there, I search for the hostname "cam.issplaylist.com" and find a paste that contains the credentials for this target.

![IP Camera Access-2](https://github.com/user-attachments/assets/819e4ec0-ae38-44d9-b130-3e629c540cd4)
![IP Camera Access-3](https://github.com/user-attachments/assets/8a32e139-837b-4500-a028-855187409f61)

Going back to the host at cam.issplaylist.com, use the credentials from the paste - admin/admin to login. After logging in, the flag is displayed at the bottom of the page. 

![IP Camera Access-4](https://github.com/user-attachments/assets/9ac9d7b9-0b4d-47ad-99f1-268a24590f74)

# IP Camera Disclosure

* Challenge - Who does Bridget need to follow up with?
* Points - 2
* Answer - Phyliss

Once logged in at the cam.issplaylist.com page with admin/admin, use the camera controls on the page to look at the board for the answer. 

![IP Camera Disclosure-2](https://github.com/user-attachments/assets/333df755-0b79-4498-964d-57e5ae4c3aa0)
