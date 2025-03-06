# Email Assessment

* Challenge - Access the email account of the suspected ISS Playlist employee. Submit the flag.
* Points - 10

First I need to find out how to access the email service that ISS Playlist uses. In the “DNS Reconnaissance” challenge for the “OSINT and Recon” section, I discovered that it's possible to do a zone transfer with Dig.

![Email-Assessment-1](https://github.com/user-attachments/assets/13dcdbfa-445b-431d-ab55-98e02db70e75)

This displays all of the DNS records, disclosing the mail server - mail.issplaylist.com. In a web browser I go to that page and login with the credentials of the suspicious user discovered in the 10-dc1.issplaylist.com challenge category. I am able to successfully login to the user's email.

![Email-Assessment-2](https://github.com/user-attachments/assets/20a7694c-d3c0-44bd-b7c0-74a23070ada7)

I click on the top email from bsh@sunsetisp.com. This email displays the flag. 

![Email-Assessment-3](https://github.com/user-attachments/assets/9c5bddc0-de4f-462f-940b-e2d7c6d7fa90)


# The Manifesto

* Challenge - Read the manifesto from the hacking team. Submit the flag.
* Points - 10

From the same email that I viewed in the previous challenge, I click on the manifesto link. At the bottom of the page is the flag. 

![The-Manifesto-1](https://github.com/user-attachments/assets/77bddcc7-83d7-49a5-809f-1803e4af3b64)
