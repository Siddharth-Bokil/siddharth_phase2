# 1. Web Gauntlet
> Can you beat the filters? Log in as admin http://shape-facility.picoctf.net:56849/ http://shape-facility.picoctf.net:56849/filter.php

## Solution:

- The filters page lists out what all is blocked for every round.
-  The first round asked for a username and password. Initially I entered anything random and that displayed the SQL command being run. After learning a bit about SQL comments, I entered `admin' --` as the username and `hi` as the password to clear the 1st round.
- In the second round, the single line comment `--` was blocked. Entering `admin' /*` as the username and `hi` as the password cleared the 2nd round.
- After some more trial and error, entering `admin';` as username and `hi` as password cleared the 3rd round.
- In the 4th round, whenever I entered admin as the username the SQL query wouldn't show up. I thought this meant the username had to be different at first, but soon realised I had to input admin without fully typing admin. I tried using the concat() function but tht wouldn't work either. Entering `ad'||'min';` as the username and `hi` as the password cleared the 4th round.
- Surprisingly, the 4th and 5th round both had the same solution. Entering `ad'||'min';` as the username and `hi` as the password cleared the 5th round.
- After completing the 5th round, the flag was in the filters page.

## Flag:

```
picoCTF{y0u_m4d3_1t_79a0ddc6}
```

## Concepts learnt:

- SQL injection
- Comments in SQLite
- Concatenation in SQLite

## Notes:

- Initially I went on a completely wrong course and started to inspect the html code and checking the cookies and all but when I read the hints it made sense it had to be different.

## Resources:

- Learnt about SQL [here](https://www.w3schools.com/sql/sql_comments.asp).

<br><br><br>
***
<br><br><br>


# 2. SSTI1

> I made a cool website where you can announce whatever you want! Try it out! I heard templating is a cool and modular way to build web apps! Check out my website here!e

## Solution:

- I first googled `ssti` to learn what it was.
- Based on the below table, I identified the template as Jinja2.

<img width="1346" height="516" alt="image" src="https://github.com/user-attachments/assets/520649da-ce77-46a0-b477-fd1d37a2dd60" />

- After searching for Jinja2 payloads, I ran `request.application.__globals__.__builtins__.__import__('os').popen('ls').read()` and found a file named flag.
- To read it, I ran `request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read()`.

## Flag:

```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_f5438664}
```

## Concepts learnt:

- Server Side Template Injection techniques
- Server Templates
- Remote Code Execution(RCE) bypassing 

## Notes:

- It took me some time to figure out the right RCE payload but eventually I found it. I realised that my first step to any challenge should be to simply google the name of the challenge as sometimes the name seems to be a bigger hint than the actual hints, as also seen in the Forensics TFTP challenge.

## Resources:

- [This](https://portswigger.net/web-security/server-side-template-injection#constructing-a-server-side-template-injection-attack) helped in figuring out it was Jinja2.
- Used [this](https://onsecurity.io/article/server-side-template-injection-with-jinja2/) to find the right Jinja2 payload.
<br><br><br>
***
<br><br><br>


# 3. Cookies

> Put in the challenge's description here

## Solution:

- Include as many steps as you can with your thought process
- You **must** include images such as screenshots wherever relevant.

```
put codes & terminal outputs here using triple backticks

you may also use ```python for python codes for example
```

## Flag:

```
picoCTF{}
```

## Concepts learnt:

- Include the new topics you've come across and explain them in brief
- 

## Notes:

- Include any alternate tangents you went on while solving the challenge, including mistakes & other solutions you found.
- 

## Resources:

- Include the resources you've referred to with links. [example hyperlink](https://google.com)
- 

