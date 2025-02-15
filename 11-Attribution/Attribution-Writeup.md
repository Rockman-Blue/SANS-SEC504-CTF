# Email Assessment

* Challenge - Access the email account of the suspected ISS Playlist employee. Submit the flag.
* Points - 10

First I need to find out how to access the email service that ISS Playlist uses. In the “DNS Reconnaissance” challenge for the “OSINT and Recon” section, I discovered that it's possible to do a zone transfer with Dig.

![Email Assessment-1](https://github.com/user-attachments/assets/1b41e1ac-ab10-4e3b-a6d2-c654be719d82)

This displays all of the DNS records, disclosing the mail server - mail.issplaylist.com. In a web browser I go to that page and login with the credentials of the suspicious user discovered in the 10-dc1.issplaylist.com challenge category. I am able to successfully login to the user's email.

![Email Assessment-2](https://github.com/user-attachments/assets/fa271198-282d-48f9-a751-19eb35831db4)

I click on the top email from bsh@sunsetisp.com. This email displays the flag. 

![397463503-0dfeea7e-c91a-45ce-96ca-5b906a3a29c4](https://github.com/user-attachments/assets/ca02a3c3-f78a-4337-b485-31be3754991e)


# The Manifesto

* Challenge - Read the manifesto from the hacking team. Submit the flag.
* Points - 10
* Answer - NetWars{YouveReachedTheEnd}

From the same email that I viewed in the previous challenge, I click on the manifesto link. At the bottom of the page is the flag. 

![The Manifesto-1](https://github.com/user-attachments/assets/9069ee7e-0081-4cda-bde7-60e2401d8c49)
