# NAME: Vidhyasri
# REG.NO.: 212222230170

# EX 8 sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

# DESIGN STEPS:
Step 1:
Install kali linux either in partition or virtual box or in live mode

Step 2:
Investigate on the various categories of tools as follows:

Step 3:
Open terminal and try execute some kali linux commands

# EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/user-attachments/assets/da61b3bb-4268-4564-9c59-bc6fe508f45d)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/user-attachments/assets/ddf1e30a-3e72-4980-be8b-0d7d48ce864e)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/user-attachments/assets/3334db16-2cf2-4dbd-bb72-417d4fcb59ca)


Click on the menu Login/Register and register for an account

![image](https://github.com/user-attachments/assets/6a611e01-9c57-4ba3-9c1e-446be7a58b07)


Click on the link “Please register here”

![image](https://github.com/user-attachments/assets/fedf401b-9ee9-4060-9bd7-ae6375b2eac3)


Click on “Create Account” to display the following page:

![image](https://github.com/user-attachments/assets/71a30753-20cd-49b6-9340-31aa876ebaca)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/user-attachments/assets/62e8037e-55cf-426f-9819-6ee0e8c78a3e)


Click “Login”. The logged in page will show as below:

![image](https://github.com/user-attachments/assets/62716bcc-980a-49dd-b0fd-46f0080c6617)


##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/user-attachments/assets/40114fb8-6fdf-406b-b300-f04bc02a7af1)


Click the login button and you will see it enter into the administrator page.

![image](https://github.com/user-attachments/assets/2545466f-2e24-4008-8dc4-38ca2880ea67)


Union-based SQL injection:
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: image

![image](https://github.com/user-attachments/assets/e0a8144b-ab0c-463e-909b-97eda8797db5)
![image](https://github.com/user-attachments/assets/9c15d273-914b-4575-a2f7-b62e2f770acb)
![image](https://github.com/user-attachments/assets/4826ee20-9999-470e-9b62-2a92999c18c2)

![image](https://github.com/user-attachments/assets/f6eadfb7-e28e-43d4-b769-667f4692e330)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![image](https://github.com/user-attachments/assets/e07e51a4-29ca-4a8d-abec-b979b29deb8c)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

![image](https://github.com/user-attachments/assets/1c579e0e-2f2d-4eeb-b20a-171a72b56d92)


After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/user-attachments/assets/60104ea8-09b3-4087-81f7-cc877749ea8f)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/user-attachments/assets/7b0f6577-ae27-48ed-9db4-8bb4d6d1e919)

![image](https://github.com/user-attachments/assets/95967d44-8df8-485a-b251-c734ea91fa4a)


As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/user-attachments/assets/bd479b2b-3e44-4ce4-b6c1-9439063f3bb0)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/user-attachments/assets/e0b2c112-32b8-444e-a2d8-b9a777256da7)


As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/user-attachments/assets/ea0fdc9c-6fe5-42c0-b46b-e5a88525ac6b)


Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

![image](https://github.com/user-attachments/assets/d3cf4a22-aa7a-4ab3-b4ba-d7fbac2cd968)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

![image](https://github.com/user-attachments/assets/adcc44cc-48c8-47c9-a9e7-35ebe9dcf04e)


![image](https://github.com/user-attachments/assets/0aa24025-e218-44e6-a98b-3c974a45fa2d)


Reading and writing files on the web-server We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

![image](https://github.com/user-attachments/assets/22c0cdf8-80f7-4a8f-89aa-d46b44b5803b)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

# RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.

