# Port Scan

* Challenge - Which TCP port(s) are open on the caffe.issplaylist.com server?
* Points - 2

This question is multiple choice, so I limited my scan to the only possible answers to save time. From the output, only one port is open. 

![397426293-07f2da3c-47e6-463a-a4f3-fd101a41fe5c](https://github.com/user-attachments/assets/fffc0675-f448-49be-8b0d-0972514c5dc1)


# Server Connection

* Challenge - Connect to the server. Submit the flag.
* Points - 4
* Answer - NetWars{ItsACoffeeMaker}

I know that Netcat can be used to connect to an open port on a server. First, I need the server's IP address. After obtaining that with a ping command, I connect to port 11111 with Netcat and retrieve the flag. 

![397427227-9704e3a2-c40d-4087-8727-5423887fdcd9](https://github.com/user-attachments/assets/8f7f011a-ecb5-4bf5-b901-178bc6a675fb)


# Vulnerability Discovery

* Challenge - Explore the caffe.issplaylist.com IoT device to identify a vulnerability. Identify the type of vulnerability.
* Points - 9
* Answer - Command Injection

Now that I’m connected to the server, I will start to look for vulnerabilities. Looking at the menu for this server from the screenshot above, 97 is a connectivity checker. I select it by typing 97 at the prompt and hitting enter. 

![Vulnerability Discovery-1](https://github.com/user-attachments/assets/7ea6843c-20e8-47a4-bba6-896d23000c0a)

Out of the possible options, number 4 seems vulnerable. What I am expecting is that the ping test accepts your input and appends it to a ping command run by the server. Before trying to exploit a server/service/page, it's good practice to observe normal and expected input. So I type in an IP address and hit enter to test the ping service. 

![Vulnerability Discovery-2](https://github.com/user-attachments/assets/3785e985-6c5e-47d4-9181-dab32b158371)

The ping functionality of this server works as expected. After experimenting with different inputs with ;, ||, and &&, I am able to perform a command injection attack. The fact that ls ran successfully indicates the server is vulnerable. 

![Vulnerability Discovery-3](https://github.com/user-attachments/assets/038129ce-cca9-4e08-bbd6-0b0f271dee03)


# Vulnerability Exploitation

* Challenge - Exploit the vulnerability on the caffe.issplaylist.com server. Submit the flag value in /flag.txt.
* Points - 7
* Answer - NetWars{CoffeeIsWonderful}

I know from the previous challenge that the server is vulnerable to a command injection vulnerability. Now it's just about crafting the right command. In the previous attack, ls shows that the flag.txt file is in the current working directory. So I use cat to display the flag value in the file. 

![Vulnerability Exploitation-1](https://github.com/user-attachments/assets/fd6c4da0-228c-4a4e-aeb5-afdb39db793d)
