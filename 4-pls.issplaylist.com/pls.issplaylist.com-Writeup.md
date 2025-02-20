# Site Reconnaissance

* Challenge - Explore the pls.issplaylist.com site and observe a sample playlist. Enter the flag.
* Points - 2

I navigated to the site at pls.issplaylist.com and typed in "Rock" in the search bar and hit enter. 

![Site-Reconnaissance-1](https://github.com/user-attachments/assets/c26d62d4-3491-47bd-9508-354105434935)

The flag is displayed a bit further down the page. 

![Site-Reconnaissance-2](https://github.com/user-attachments/assets/e9a11345-0099-4a2b-9227-74bfbf4a0a89)


# Error Generation

* Challenge - Manipulate your search syntax when generating a playlist, attempting to produce an error. Enter the flag.
* Points - 4

Here, I can produce an error with unexpected inpupt. The first thing I tried was typing in a ```'``` in the search field to see if the page returns an SQL error. 

![Error-Generation-1](https://github.com/user-attachments/assets/47a36d1d-097e-4304-aae6-85b0cba5c562)

The page returns an SQL error and the flag is displayed. 


# Vulnerability Discovery

* Challenge - Explore the pls.issplaylist.com website to identify a vulnerability. Identify the type of vulnerability.
* Points - 5

From the previous challenge above, the error indicates that the search function queries the user's input from a backend database and returns the results of the query to the frontend of the site. 


# Vulnerability Exploitation

* Challenge - Exploit the vulnerability identified on the pls.issplaylist.com website. Retrieve the flag value stored in the flag table.
* Points - 5

Manual SQL injection can be trick and time consuming. Instead, I'll use the Sqlmap tool to make exploiting the site easier. Several uses of Sqlmap are needed to get the flag. When you use SQL map, there are two rules to follow:
* Use a valid, non-error generating URL.
* Put the URL in quotes.

First, I use Sqlmap against the URL that was displayed in the browser when I typed the expected input of "Rock" into the search bar, http://pls.issplaylist.com/index.php?search=Rock. This command shows that the search field is vulnerable, and gives us some details on the injection point.  

![Vulnerability-Exploitation-1](https://github.com/user-attachments/assets/8dc8fe99-b1d8-41e2-80c2-a7c3533a03ce)

Next, I repeat the same command as above, but with the addition of the ```--dbs``` argument to enumerate the SQL databases that are present. 

![Vulnerability-Exploitation-2](https://github.com/user-attachments/assets/6f33c5e5-06b1-40ca-b352-5f17fb4dcf3d)

The next step is to enumerate the tables in the databases with the ```-D``` argument, which replaces the ```--dbs``` argument. This is to find which database has the flag table. I tested the ```-D``` argument with the ```information_schema```, ```pls```, and ```test``` databases to find which one has the flag table. Eventually, I ran the command below, and the ```pls``` database was the one that contained the flag table. 

![Vulnerability-Exploitation-3](https://github.com/user-attachments/assets/14df778b-9c6d-4383-bd8a-d32d8293c9e3)

Now that I know that the pls database has the flag table, it's time to find the flag. Sqlmap supports dumping table content with the ```--dump``` argument. The format is:
* ```sqlmap -u "URL" -D databaseName -T tableName --dump```

I did so with the following command. After running the below command to dump the table output, I found the flag. 

![Vulnerability-Exploitation-4](https://github.com/user-attachments/assets/45f067fb-2b9b-4374-af68-8f8ca8a368fe)

![Vulnerability-Exploitation-5](https://github.com/user-attachments/assets/19d557f8-3b28-482c-b0cd-174904fdd10c)


# Always Be Cracking

* Challenge - Use your access to the target database to retrieve the contents of the users table in the pls database. Recover the password for ISS Playlist employee epreston.
* Points - 4

For this challenge I will use Sqlmap again. From the previous challenge, I know how to dump the contents of a table for a specified database with the following generic command format:
* sqlmap -u "URL" -D databaseName -T tableName --dump

From the question, I know that we're looking for the users table in the pls database. So I ran the following command. Sqlmap asks me if I want to store the hashes to a temporary file. I hit y and press enter to continue dumping the content of the users table. Next, Sqlmap asks if I want to crack those hashes via a dictionary attack. I type n and press enter, and I'll crack the hashes with Hashcat later on instead. Sqlmap successfully gets the password hashes from the users table.

![Always Be Cracking-1](https://github.com/user-attachments/assets/0c054e03-19cf-426a-aa4d-d5b31493e2d2)

![396567468-32159ebf-15d5-4192-bcbe-a8ac7fba11d7](https://github.com/user-attachments/assets/b69ad21f-9bf0-4144-812f-a24d325d5216)

In the Sqlmap output from the second screenshot of the two above, it shows "writing hashes to a temporary file '/tmp/sqlmapy0bF_L4759/sqlmaphashes-TpoRtk.txt'". I copied that file to my working directory, confirmed it was copied over successfully, and displayed the contents. 

![396571455-ef269b8f-15a0-4eb8-a697-7e9829e7a1b9](https://github.com/user-attachments/assets/064c6704-011e-42e0-a26f-0fdce6cba520)

Now that I have the hashes from the user table, it's time to crack them with hashcat. It took some tries to get the right command. After the first error, I tried the command again, this time with the --user argument to tell Hashcat to expect a username in addition to the password. 

![Always Be Cracking-4](https://github.com/user-attachments/assets/59942191-a6f4-4a29-bc51-ef871d244928)

Now it's time to figure out which hash mode to specify in the command since Hashcat tells me that it can be one of the seven showed in the above screenshot. After experimenting with the different modes in the following generic command format, I found the right command to enter. 
* hashcat -a 0 -m <mode_from_output> sqlmaphashes-TpoRtk.txt /usr/share/wordlists/passwords.txt --user

![Always Be Cracking-5](https://github.com/user-attachments/assets/c93ed970-76a7-41b3-ae9c-0fded56c2708)

Next, I use the --show and --user arguments to tell Hashcat to display the cracked password and associated usernames. The password for epreson is shown.

![396574981-951181fc-b1b2-4d51-8cc8-c99c3fb3fa00](https://github.com/user-attachments/assets/cbfd96f1-84b5-4966-8f28-b7fa962bdd6d)


# Always Be Cracking (2)

* Challenge - Continue to recover passwords from the users table. Submit the password for the jorestes account.
* Points - 4

The password for the jorestes account was cracked when I did the challenge above. 
