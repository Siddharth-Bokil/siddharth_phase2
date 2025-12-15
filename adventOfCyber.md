# 1. Linux CLI - Shells Bells

> Explore the Linux command-line interface and use it to unveil Christmas mysteries.


## Solution
- Ran `bash cd Guides/` and then `ls -al`. Found a hidden file and read the flag in it using `cat .guide.txt`.
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



