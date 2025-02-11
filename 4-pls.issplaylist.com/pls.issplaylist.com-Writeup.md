# Site Reconnaissance

* Challenge - Explore the pls.issplaylist.com site and observe a sample playlist. Enter the flag.
* Points - 2

I navigated to the site at pls.issplaylist.com and typed in "Rock" in the search bar and hit enter. 

![Site Reconnaissance-1](https://github.com/user-attachments/assets/9f7cc207-61cf-45fe-8afe-72ff85698f37)

The flag is displayed a bit further down the page. 

![396549663-a5981359-65a1-4ed7-a77f-e23dba05e113](https://github.com/user-attachments/assets/1a04e7e9-c433-4f59-b382-d401a2ed5ca6)


# Error Generation

* Challenge - Manipulate your search syntax when generating a playlist, attempting to produce an error. Enter the flag.
* Points - 4
* Answer - NetWars{ErrorReported}

Here, I can produce an error with unexpected inpupt. The first thing I tried was typing in a ' in the search field to see if the page returns an SQL error. 

![396550594-b9b28aee-01a9-4234-93e0-9a2674e38505](https://github.com/user-attachments/assets/2fd6eeb3-a3a8-47bd-9f12-900d2e3518fa)

The page returns an SQL error and the flag is displayed. 


# Vulnerability Discovery

* Challenge - Explore the pls.issplaylist.com website to identify a vulnerability. Identify the type of vulnerability.
* Points - 5

From the above previous challenge, there's an SQL error. The SQL error indicates that the search function is querying from a backend database and returned the results of the query to the frontend of the site after submitting a search term. 


# Vulnerability Exploitation

* Challenge - Exploit the vulnerability identified on the pls.issplaylist.com website. Retrieve the flag value stored in the flag table.
* Points - 5

Manual SQL injection can be trick and time consuming. Instead, I'll use the Sqlmap tool to make exploiting the vulnerability easier. Several uses of Sqlmap are needed to get the flag. When you use SQL map, there are two rules to follow:
* Use a valid, non-error generating URL.
* Put the URL in quotes.

First, I use Sqlmap against the URL that was displayed in the browser when I typed the expected input of "Rock" into the search bar, http://pls.issplaylist.com/index.php?search=Rock. This command shows that the search field is vulnerable, and gives us some details on the injection point.  

![Vulnerability Exploitation-1](https://github.com/user-attachments/assets/c3e1d47d-c3b9-4abc-96bf-2b7f2bb57df3)

Next, I repeat the same command as above, but with the addition of the --dbs argument to enumerate the SQL databases that are present. 

![Vulnerability Exploitation-2](https://github.com/user-attachments/assets/ba1be97d-3582-4a93-84e0-573c49489bb2)

The next step is to enumerate the tables in the databases with the -D argument, which replaces the --dbs argument. This is to find which database has the flag table. I tested the -D argument with the information_schema, pls, and test databases to find which one had the flag table. Eventually, I ran the command below, and the pls database was the one that contained the flag table. 

![Vulnerability Exploitation-3](https://github.com/user-attachments/assets/ba1baa31-f68b-4654-988d-dcff6354a4d3)

Now that I know that the pls database has the flag table, it's time to find the flag. Sqlmap supports dumping table content with the --dump argument. The format is:
* sqlmap -u "URL" -D databaseName -T tableName --dump

I did so with the following command. After running the below command to dump the table output, I found the flag. 

![Vulnerability Exploitation-4](https://github.com/user-attachments/assets/f6838295-173e-4859-8131-ea57534688ae)

![396563914-6bee0bde-da58-417d-89b1-958e7f955557](https://github.com/user-attachments/assets/be3128ba-b0ab-4676-897c-165394b647fc)


# Always Be Cracking

* Challenge - Use your access to the target database to retrieve the contents of the users table in the pls database. Recover the password for ISS Playlist employee epreston.
* Points - 4
* Answer - spacestation

For this challenge I will use Sqlmap again. From the previous challenge, I know how to dump the contents of a table for a specified database with the following generic command format:
* sqlmap -u "URL" -D databaseName -T tableName --dump

From the question, I know that we're looking for the users table in the pls database. So I ran the following command. Sqlmap asks me if I want to store the hashes to a temporary file. I hit y and press enter to continue dumping the content of the users table. Next, Sqlmap asks if I want to crack those hashes via a dictionary attack. I type n and press enter, and I'll crack the hashes with Hashcat later on instead. Sqlmap successfully gets the password hashes from the users table.

![Always Be Cracking-1](https://github.com/user-attachments/assets/0c054e03-19cf-426a-aa4d-d5b31493e2d2)

![396567468-32159ebf-15d5-4192-bcbe-a8ac7fba11d7](https://github.com/user-attachments/assets/b69ad21f-9bf0-4144-812f-a24d325d5216)

In the Sqlmap output from the second screenshot of the two above, it shows "writing hashes to a temporary file '/tmp/sqlmapy0bF_L4759/sqlmaphashes-TpoRtk.txt'". I copied that file to my working directory, confirmed it was copied over successfully, and displayed the contents. 

![Always Be Cracking-3](https://github.com/user-attachments/assets/ef269b8f-15a0-4eb8-a697-7e9829e7a1b9)

Now that I have the hashes from the user table, it's time to crack them with hashcat. It took some tries to get the right command. After the first error, I tried the command again, this time with the --user argument to tell Hashcat to expect a username in addition to the password. 

![Always Be Cracking-4](https://github.com/user-attachments/assets/59942191-a6f4-4a29-bc51-ef871d244928)

Now it's time to figure out which hash mode to specify in the command since Hashcat tells me that it can be one of the seven showed in the above screenshot. After experimenting with the different modes in the following generic command format, I found the right command to enter. 
* hashcat -a 0 -m <mode_from_output> sqlmaphashes-TpoRtk.txt /usr/share/wordlists/passwords.txt --user

![Always Be Cracking-5](https://github.com/user-attachments/assets/c93ed970-76a7-41b3-ae9c-0fded56c2708)

Next, I use the --show and --user arguments to tell Hashcat to display the cracked password and associated usernames. The password for epreson is spacestation.

![Always Be Cracking-6](https://github.com/user-attachments/assets/951181fc-b1b2-4d51-8cc8-c99c3fb3fa00)


# Always Be Cracking (2)

* Challenge - Continue to recover passwords from the users table. Submit the password for the jorestes account.
* Points - 4
* Answer - Carolina1

The password for the jorestes account, Carolina1 was cracked when I did the challenge above. 
