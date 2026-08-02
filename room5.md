**Room 5 = boot2root**

1\. source code of login page had `commented credentials`: dj / dj

2\. logged in, exported/imported playlist as .yml to figure out syntax

3\. import showed my YAML back as a Python dict -> app has Python backend

4\. exploited unsafe YAML load with payload:

&#x20;  `!!python/object/apply:subprocess.check\_output` + `- \["id"]` -> ran as bartender (RCE = Remote Code Execution)

5\. searched for and found user flag at /home/bartender/user.txt -> *THM{y4ml\_pl4yl1st\_pwns\_th3\_b34ch}*

6\. got a reverse shell (`nc` listener + bash payload)

7\. enumerated: sudo/SUID/capabilities/cron all dead

8\. ps aux | grep root -> found root process leaking password: "SunsetSpritz2024!"

9\. `su root` with that password -> authorized as root

10\. searched for and found a root flag at /root/root.txt -> *THM{cr3d3nt14l\_r3us3\_4t\_th3\_b34ch\_b4r}*



`commented creds -> YAML deserialization RCE -> user flag -> leaked root password -> su root -> root flag`

