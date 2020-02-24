Web-Server Challenges

## HTML - Source Code

Look in the source code of the HTML page and you will see the password. 

<img src="images/ch1-1.png">

What's interesting is that if you don't use the inspector, the password will be tabbed so far right that you might miss it if you don't scroll sideways.

<img src="images/ch1-2.png">

The password to complete the level is `nZ^&@q5&sjJHev0y`

## Weak password

Guess the credentials. After guessing, the credentials end up being:<br>
username: `admin`
password: `admin`

The password to complete the level is `admin`

## HTTP - User-agent

For this challenge you need to request the web page with the `user-agent` string being `admin`. 
<img src="images/ch2-1.png">

You can do this using the following curl command: 
```shell
curl --user-agent "admin" http://challenge1.root-me.org/web-serveur/ch2/
```
<img src="images/ch2-2.png">

The password to complete the level is `rr$Li9%L34qd1AAe27`

## HTTP - Directory Indexing

This challenge provides you with the supposed directory for the password in the source code of the page.

<img src="images/ch4-1.png">

However, if you visit the page, there's nothing there. 

<img src="images/ch4-2.png">

If you go back and pay closer attention to the directory they gave you, it turns out to be a subdirectory, with the root directory being `/admin`. Navigate to `/admin` and you'll be able to see more files. 

<img src="images/ch4-3.png"> 

The password should be in `/admin/backup/admin.txt`.

The password to complete the level is `LINUX`

## Backup File

This one is a bit trickier. You are given a username and password field, but the intended solution doesn't include any improper text sanitization. If you are a vim god, you would know that [vim backup files are stored as the original name of the file, with a tilde character at the end of the filename.](https://medium.com/@Aenon/vim-swap-backup-undo-git-2bf353caa02f) 

<img src="images/ch11-1.png">

Knowing this, just navigate to `/index.php~`, and download the file. There, you will find the password. 

<img src="images/ch11-2.png">

The password to complete the level is `0CCY9AcNm1tj`

## PHP - Command injection

The first thing we see is a text field that accepts IP addresses, pings them, and returns the output as if it was a shell command. 

<img src="images/ch54-1.png">

Seeing the output look like it came from a unix shell, trying to put commands such as `ls` sounds like a good idea, but there seems to be some sanitization. To get around this, you can put in an IP address, along with another command using the `&` symbol. In the description of the challenge, it says the flag is in the `index.php` file, but if you look at the source code of the page, there's nothing there. Assuming that file is on the compromised service, we can inject a command to cat the file, and grep for the flag. The command I used was:
```shell
127.0.0.1 & cat index.php | grep flag
```

<img src="images/ch54-2.png">

The password to complete the level is `S3rv1ceP1n9Sup3rS3cure`

## HTTP - Headers

From the challenge name, we assume that it has something to do with the headers. To get the headers of the web page, we can use a curl command:

```shell
curl --head http://challenge01.root-me.org/web-serveur/ch5/
```

<img src="images/ch5-1.png">

Now we know what the headers are, we can change them using the curl command:

```shell
curl --include --header "Header-RootMe-Admin: Administrator" http://challenge01.root-me.org/web-serveur/ch5/
```

<img src="images/ch5-2.png">

The password to complete the level is `HeadersMayBeUseful`

## HTTP - Verb tampering

When visiting the web page for this challenge, we are asked for credentials.

<img src="images/ch8-1.png">

However, the intended solution for this challenge is not to manipulate input sanitization, but to tamper with different HTTP methods, called [verb tampering](https://wiki.owasp.org/index.php/Testing_for_HTTP_Verb_Tampering_(OTG-INPVAL-003)). We can use curl commands to request the web page with different verbs, and find the password. We can use this curl command to get the password (other verbs also yield the same result).

```shell
curl --request PUT http://challenge01.root-me.org/web-serveur/ch8/
```
	
<img src="images/ch8-2.png">

The password to complete the level is `a23e$dme96d3saez$$prap`

## Install files

We start with a blank and empty HTML page. Looking at the description of the challenge hints at the use of the `phpbb` software. Looking at the [documentation](https://www.phpbb.com/support/docs/) of the software, we see that an install file is in the directory `phpbb/install/install.php`. Go to this directory and you will get the password.

<img src="images/ch6-1.png">

The password to complete the level is `karambar`

## HTTP - Cookies

We start the challenge with a text box and a button that shows emails. When we click the button, the web page tells us that we need to be admin to see the saved emails. If you look at the URI, we can see that there is a parameter probably specifying what type of user we are.

<img src="images/ch7-1.png">

If we change the default value to `admin` and click the button to see the saved logins once again, the web page tells us that the cookie is incorrect. Go into your browsers cookie settings and change the value of the only cookie to `admin`. Refresh the page and you'll get the password.

<img src="images/ch7-2.png"> 

The password to complete the level is `ml-SYMPA`

## SQL injection - Authentication

We start the challenge with an authentication portal, and as the title of the challenge suggests, we need to use SQL injection.

<img src="images/ch9-1.png">

If you try a basic SQL injection payload, such as `username=admin&password=' or 1=1 --`, user1 will be returned with the password hidden with the `password` HTML tag (you can change the tag in the source code to reveal the hidden text, or copy it directly from the source). 

<img src="images/ch9-2.png">

However, this is NOT the password for the challenge. Instead of user1, we need to get the password for the administrator, as was said in the description of the challenge. After some trial and error, the SQL payload that worked and returned the administrator credentials was: `username:admin' or '1=1&password=admin`.

<img src="images/ch9-3.png">

The password to complete the level is `t0_W34k!$`

## HTTP - Improper redirect 

We start the challenge with a login portal that says we're not authenticated and can't view this page (take note of the URI). 

<img src="images/ch32-1.png">

If you change the URI to contain `index.php`, it will quickly go to that page, and redirect back to the one we started at. You can use burp suite, ZAP proxy, or cult to capture the request. I used the curl command:

```shell
curl "http://challenge01.root-me.org/web-serveur/ch32/index.php"
```

<img src="images/ch32-2.png">

If you want to do the challenge properly, you can use burp suite or any other program that intercepts server responses. 
When in burp suite, go the `proxy` tab, and then `options` tab, and enable `Intercept Server Responses` (this is off by default). 

<img src="images/ch32-3.png">

Then, turn on intercept and request the same webpage as we did in the curl command above. Forward the requests until you get a response, it should be an `HTTP 302 Found` response. Change the code to `200 OK` and forward the response to your browser, you should get a web page with the flag.

<img src="images/ch32-4.png">

<img src="images/ch32-5.png">

The password to complete the level is `ExecutionAfterRedirectIsBad`

## HTTP - POST

With this challenge, we are greeted with a fun little game that generates a random number every time you click the button. 

<img src="images/ch56-1.png">

After seeing the source code, we see that the button sends a POST request with two values: `generate` and `score`. 

<img src="images/ch56-2.png">

We can use CURL to send a POST request with a score over `999999` (If you send a POST request with just a `score` parameter, it won't work, make sure to include the `generate` value as well). We can use the CURL command: 

```shell
curl --request POST --form 'generate=Give+a+try%21' --form 'score=1000000' http://challenge01.root-me.org/web-serveur/ch56/
```

<img src="images/ch56-3.png">

THe password to complete the level is `H7tp_h4s_N0_s3Cr37S_F0r_y0U`
