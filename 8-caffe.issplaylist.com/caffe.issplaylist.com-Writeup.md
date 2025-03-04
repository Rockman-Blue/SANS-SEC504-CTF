# Port Scan

* Challenge - Which TCP port(s) are open on the caffe.issplaylist.com server?
* Points - 2

This question is multiple choice, so I limited my scan to the only possible answers to save time. From the output, only one port is open. 

![Port-Scan-1](https://github.com/user-attachments/assets/b97ed407-8bb9-482f-90de-130ae693bcef)


# Server Connection

* Challenge - Connect to the server. Submit the flag.
* Points - 4

I know that Netcat can be used to connect to an open port on a server. First, I need the server's IP address. After obtaining that with a ping command, I connect to the open port with Netcat and retrieve the flag. 

![Server-Connection-1](https://github.com/user-attachments/assets/7537c96f-2c44-4d16-9927-2d3f36508e96)


# Vulnerability Discovery

* Challenge - Explore the caffe.issplaylist.com IoT device to identify a vulnerability. Identify the type of vulnerability.
* Points - 9

Now that I’m connected to the server, I will start to look for vulnerabilities. Looking at the menu for this server from the screenshot above, 97 is a connectivity checker. I select it by typing 97 at the prompt and hitting enter. 

![Vulnerability-Discovery-1](https://github.com/user-attachments/assets/03fd9229-e7e1-4eef-b95a-a1d37345ab82)

Out of the possible options, number 4 seems vulnerable. What I am expecting is that the ping test accepts your input and appends it to a ping command run by the server. Before trying to exploit a server/service/page, it's good practice to observe normal and expected input. So I type in an IP address and hit enter to test the ping service. 

![Vulnerability-Discovery-2](https://github.com/user-attachments/assets/0964c878-a194-42a4-aefd-4d0daeacb069)

The ping functionality of this server works as expected. After experimenting with different inputs with ;, ||, and &&, I am able to perform an input based attack. The fact that ls ran successfully indicates the server is vulnerable. 

![Vulnerability-Discovery-3](https://github.com/user-attachments/assets/76988d12-9ac9-48cb-aa35-3e7eac228324)


# Vulnerability Exploitation

* Challenge - Exploit the vulnerability on the caffe.issplaylist.com server. Submit the flag value in /flag.txt.
* Points - 7

I know from the previous challenge that the server is vulnerable to an input attack. Now it's just about crafting the right command. In the previous attack, ls shows that the flag.txt file is in the current working directory. So I use cat to display the flag value in the file. 

![Vulnerability-Exploitation-1](https://github.com/user-attachments/assets/86a70867-04a7-4163-b9cd-84b785767a85)
