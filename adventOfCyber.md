# 1. Linux CLI - Shells Bells

> Explore the Linux command-line interface and use it to unveil Christmas mysteries.


## Solution
- Ran `cd Guides/` and then `ls -al`. Found a hidden file and read the flag in it using `cat .guide.txt`.
- Ran `find /home/socmas -name *egg*` and then `cat /home/socmas/eggstrike.sh` to get the second flag.
- Ran `sudo su` to become root and then accessed the bash history using `cat /root/.bash_history` and found the flag there.


## Flag

```
THM{learning-linux-cli}
THM{sir-carrotbane-attacks}
THM{until-we-meet-again}
```


## Concepts learnt

- Basic CLI commands


## Notes
Pretty much the same as `pwn.college`, nothing heavy


## Resources
None




<br><br><br>
***
<br><br><br>




# 2. Phishing - Merry Clickmas

> Learn how to use the Social-Engineer Toolkit to send phishing emails.


## Solution
Simply followed the instructions given on the site and used `setoolkit` to obtain the password

## Flag

```
unranked-wisdom-anthem
```


## Concepts learnt

- Social Engineering
- setoolkit


## Notes
Fun challenge


## Resources
None




<br><br><br>
***
<br><br><br>




# 3. Splunk Basics - Did you SIEM?

> Learn how to ingest and parse custom log data using Splunk.


## Solution
- Ran `index=main sourcetype="web_traffic" |  timechart span=1d count | sort by count | reverse` in the search query to find dates on which there was heavy traffic.
- Ran `Search query: index=main sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox*` to remove common legitimate user agents and only show the suspicious ones. This allowed me to find the IP address used to attack the server as `198.51.100.55`.
- `index=main client_ip="198.51.100.55"` gave the path traversal attempts.
- The bytes transferred were found with `sourcetype=firewall_logs action=ALLOWED |  table src_ip dest_ip bytes_transferred |  stats sum(bytes_transferred)` as `12167`.

## Flag

```
1: 198.51.100.55
2: 2025-10-12
3: 993
4: 658
5: 126167
```


## Concepts learnt

- Splunk


## Notes
None


## Resources
Splunk documentation




<br><br><br>
***
<br><br><br>




# 4. AI in Security - old sAInt nick

> Unleash the power of AI by exploring it's uses within cyber security.


## Solution
- I used the AI to build a python script which perfoms a sql exploit to the target website, `10.48.147.245:5000`
- I used the script generated, only having to input the target ip address. The script when ran with `python3 script.py` gave the flag.
```
Response Status Code: 200

Response Headers:
  Date: Tue, 23 Dec 2025 18:37:51 GMT
  Server: Apache/2.4.65 (Debian)
  X-Powered-By: PHP/8.1.33
  Expires: Thu, 19 Nov 1981 08:52:00 GMT
  Cache-Control: no-store, no-cache, must-revalidate
  Pragma: no-cache
  Vary: Accept-Encoding
  Content-Encoding: gzip
  Content-Length: 540
  Keep-Alive: timeout=5, max=99
  Connection: Keep-Alive
  Content-Type: text/html; charset=UTF-8

Response Body:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard - SQLi Lab</title>
    <link href="assets/css/bootstrap.min.css" rel="stylesheet">
    <link href="assets/css/style.css" rel="stylesheet">
</head>
<body class="dashboard-body">
    <div class="dashboard-container">
        <div class="welcome-banner">
            <h1>Welcome, admin!</h1>
            <p>You have successfully logged in to the system.</p>
        </div>
        
        
        <div class="alert alert-success alert-dismissible fade show" role="alert">
            <h4 class="alert-heading">Exploit Successful!</h4>
            <hr>
            <p class="mb-0"><code>FLAG: THM{SQLI_EXPLOIT}</code></p>
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
        
        <a href="logout.php" class="btn btn-danger">Logout</a>
    </div>
    
    <script src="assets/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```
- The next stage asked for the logs of the performed attacks and the AI gave its analysis -
```
LOG ENTRIES
198.51.100.22 - - [03/Oct/2025:09:03:11 +0100] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" "username=alice%27+OR+1%3D1+--+-&password=test"

---

### 1. Analysis of the Logs
- IP Address: The attacker\u2019s IP address is 198.51.100.22.
- Time and Date: The attack occurred on 3 October 2025 at 09:03.
- URL: The page accessed was /login.php.
- Username: The username attempted was alice%27+OR+1%3D1+--+-&password=test.
- SQL Injection: The password parameter contains an SQL injection attempt: alice%27 OR 1=1. This indicates a vulnerability in the application, as SQL injection exploits untrusted user input.
- Conclusion: This log entry highlights a potential SQL injection attack, emphasizing the importance of monitoring logs for suspicious activity.

---

### 2. Why Logging Matters
Logging is essential for detecting security threats like SQL injection. It allows teams to identify patterns, track anomalies, and respond to incidents in real-time. By analyzing log entries, blue team members can:
- Identify vulnerabilities in the application.
- Understand the scope of potential threats.
- Enable proactive measures like patching or updating systems.

---

### 3. Blue Team Perspective
- Vulnerability Identification: The log reveals a potential flaw in the application, prompting immediate attention.
- Monitoring: Teams should continue monitoring logs for other signs of exploitation.
- Response: Actions like applying patches or reviewing user input validation can mitigate the risk.
```
- The next stage gave us an analysis of this days aoc application and talked about safer sql coding and gave the flag.
```
Explanation of Vulnerability:
The code initializes $user and $pass with ??, which means they are set to empty strings. This allows attackers to inject malicious data into the database, which can then be used for SQL injection attacks. For example, if the username is 'admin' and the password is '123', the code would output those values, which could be used to execute arbitrary SQL queries.

Why this is vulnerable:
SQL injection occurs when an attacker can inject malicious code into a database, which can be by allowing unvalidated input. This vulnerability is a classic example of how simple code can be exploited for serious security issues.

Tools and Techniques for Secure Development:
- Use OWASP Top 10 for secure coding practices
- Implement input validation
- Use prepared statements
- Use SQL map for secure database interactions
```
  
## Flag

```
THM{SQLI_EXPLOIT}
THM{AI_MANIA}
```


## Concepts learnt
- SQL injections and its prevention


## Notes
None


## Resources
None




<br><br><br>
***
<br><br><br>




# 5. IDOR - Santa’s Little IDOR

> Learn about IDOR while helping pentest the TrypresentMe website.


## Solution
- Loaded up the target page with the following credentials -
```
Username:niels
Password:TryHackMe#2025
IP address/Website:http://10.48.129.122
```
Inspected the page, went to the storage tab > local storage and then brute force the user value from 10, until I found a user with 10 children. This worked with the user `15`.

## Flag

```
1: Insecure Direct Object Reference
2: Horizontal
3: 15
```


## Concepts learnt

- IDOR
- Web-Exploitation




## Notes
None


## Resources
None




<br><br><br>
***
<br><br><br>




# 6. Malware Analysis - Egg-xecutable

> Discover some common tooling for malware analysis within a sandbox environment.

## Solution
- I Began with the static analysis of the given malware `hophelper.exe`. Loaded the executable into `PeStudio`, clicked on the `indicators` tab in the dropdown and found the SHA256Sum.
- Next, I clicked on the `strings` indicator on the left pane of `PeStudio`, to find a hidden flag at the bottom, which was `THM{STRINGS_FOUND}`
-  For the dynamic analysis, on RegShot, took the first shot and then ran the malware. This changed all the desktop icons to a egg. After this i ran the second shot and clicked `compare`.
-  The program added itself to the run during startup in the registry, which I was asked to check. this was done at the location `HKU\S-1-5-21-1966530601-3185510712-10604624-1008\Software\Microsoft\Windows\CurrentVersion\Run\HopHelper`
-  Next I used Procmon to analyse what all the malware was doing. Opened up procmon, then ran the malware again. After this I used a filter to only search for `HopHelper.exe` and applied another to search for only TCP operations.
-  Using this I was able to find what protocol the malware was using to communicate, which was `http`.

## Flag

```
1: F29C270068F865EF4A747E2683BFA07667BF64E768B38FBB9A2750A3D879CA33
2: THM{STRINGS_FOUND}
3: HKU\S-1-5-21-1966530601-3185510712-10604624-1008\Software\Microsoft\Windows\CurrentVersion\Run\HopHelper: "C:\Users\DFIRUser\Desktop\HopHelper\HopHelper.exe"
4: HTTP
```


## Concepts learnt
- Checksums - used within cyber security to track and catalogue files and executables. For example, you can Google the checksum to see if this has been identified before.
-  RegShot
-  Procmon


## Notes
None


## Resources
None



<br><br><br>
***
<br><br><br>



# 7. Network Discovery - Scan-ta Clause

> Discover how to scan network ports and uncover what is hidden behind them.

## Solution
- Identified what services the server is exposing to the internet using `Nmap` to probe for open ports. Used a Full Range Scan with the `-p-` flag and used the `--script=banner` flag to grab service information as `nmap -p- --script=banner MACHINE_IP`
- The scan identified an FTP server on a non-standard port `21212`, and this got me the first key.
- For the second key, I checked Port `25251` using `nc` to interact with the custom service.
- For the third key, I used `dig` and ran `dig @10.48.141.59 TXT key3.tbfc.local +short` and got the third key.
- By entering the combined keys into the web interface at `http://MACHINE_IP`, I gained access to the Secret Admin Console.
- Used the command `mysql -D tbfcqa01 -e "select * from flags;"` to get the final flag `THM{4ll_s3rvice5_d1sc0vered}`.

## Flag
```
1: Pwned by HopSec
2: 3aster_
3: 15_th3_
4: n3w_xm45
5: 3306
6: THM{4ll_s3rvice5_d1sc0vered}
```

## Concepts learnt
- Port scanning
- Nmap
- TCP and UDP

## Notes
None

## Resources
None

