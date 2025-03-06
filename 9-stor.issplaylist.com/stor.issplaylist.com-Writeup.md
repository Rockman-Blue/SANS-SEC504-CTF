# Target Enumeration

* Challenge - What is the non-hidden share name on the stor.issplaylist.com server?
* Points - 8 

When I saw share in the challenge question, I thought of Windows and SMB. Two popular SMB tools are Rpcclient and Smbclient. Rpcclient is used for target configuration details, while Smbclient is used for file share access. In this case, Smbclient is the appropriate tool. With Smbclient, I need a username and password. I tried to look at ClippedBin to find usernames, but that didn’t work.

Instead, I tried to re-use already compromised credentials from previous challenges to try to access the SMB server. Some credentials I tested are for these users:
* tomcat
* pemma
* epreston
* cghislaine
* jorestes

After some testing, I was able to login as the jorestes user. In the command above, IP is the below, and jorestes is the user. The quotes around domain\user are needed since \ is interpreted as a shell escape character without it. The “-m SMB2” argument is needed to access modern windows systems.

![Target-Enumeration-1](https://github.com/user-attachments/assets/7c0e8a3a-2127-47b6-88eb-2003e9cc8353)

 There is one non-hidden share name is, the one that doesn’t have a $ at the end of the share name. The hidden shares are ADMIN$, C$, and IPC$ - or anything with a $ at the end of the name.


# Server Share Access

* Challenge - Access the data store on the stor.issplaylist.com server. Submit the flag value.
* Points - 8

In the previous challenge, I enumerated the SMB shares on the server to find the one non-hidden share. Now let’s connect to it to retrieve the flag. After noticing the error message at the bottom of the command above, "Failed to connect...", I changed my Smbclient command to connect with SMB3. I then authenticated with the password of for the jorestes user, which was discovered earlier in the CTF.  

![Server-Share-Access-1](https://github.com/user-attachments/assets/7c7a82b5-b404-49e1-b6e3-2bc4573edeb7)

The command ran successfully, and I was able to get a SMB prompt. I enumerated the files on the share with ls and saw that flag.txt is present. I used the get command to download the file to my local system. After the file was downloaded, I exit the SMB connection and go back to my local system. I confirm that flag.txt was download successfully, and displayed the contents. 

![Server-Share-Access-2](https://github.com/user-attachments/assets/e5880a96-07cc-4bbe-93b4-ba951b6c5e64)


# Embedded Data Retrieval

* Challenge - Examine the embedded data in any song from the STOR share. Submit the flag.
* Points - 7

Embedded data means data hidden in some or wrapped by some other data. Exiftool is a script that extracts metadata from a file. I connect back to the STOR share, enumerate the folders, cd to one, and download an audio file from the server.

![Embedded-Data-Retrieval-1](https://github.com/user-attachments/assets/404f5f1e-e57a-4935-89b7-ae2c3ea6e88b)

Now that I have an audio file from the server, I exit the SMB connection to return to my local system terminal by typing "exit" and hitting enter. I confirm the .mp3 file was successfully downloaded. I run Exiftool against the .mp3 file, the value of the comment field is the flag. 

![Embedded-Data-Retrieval-2](https://github.com/user-attachments/assets/d546dd23-c47d-4915-aa1f-40938ad38f05)


# Target Exploitation

* Challenge - Gain remote access to the stor.issplaylist.com server. Submit the flag value in C:\flag.txt.
* Points - 8

First, I use Smbclient to list all of the shares on the server. In the "Server Share Access" challenge, I connected to the non-hidden share to find the value in the flag.txt file on that share. Since I already looked at that share, I figured the flag.txt file for this challenge must be on a different share. 

![397442459-59d2b71e-4d83-4b25-84ac-d229a553fba9](https://github.com/user-attachments/assets/288747dd-0d84-4152-bbd8-9f7439753f93)

After experimenting with connecting to the hidden shares, I noticed that the C$ share had a flag.txt file. I connected to the C$ share, enumerated the files, and downloaded the flag.txt file. I exit the SMB connection and confirm the flag.txt file was downloaded onto my local system. From there, I display the file contents to get the flag. 

![397444118-056014ee-3d00-4a83-97cb-aa6db405d399](https://github.com/user-attachments/assets/866157e2-3224-4825-81fd-b566a23f718c)

