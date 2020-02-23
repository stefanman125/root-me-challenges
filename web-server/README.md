## Web-Server Challenges

## HTML - Source Code

Look in the source code of the HTML page and you will see the password. What's interesting is that if you don't use the inspector, the password will be tabbed so far right that you might miss it if you don't scroll sideways.
<br>
<img src="images/ch1-1.png">
<br>
<img src="images/ch1-2.png">

The password to complete the level is `nZ^&@q5&sjJHev0y`
<br>

## Weak password

Guess the credentials. After guessing, the credentials end up being:<br>
username: `admin`
password: `admin`

The password to complete the level is `admin`

## HTTP - User-agent

For this challenge you need to request the web page with the `user-agent` string being `admin`. 
<img src="images/ch2-1.png">

You can do this using the following curl command: `curl --user-agent "admin" http://challenge1.root-me.org/web-serveur/ch2/`
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