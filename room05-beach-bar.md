**Room 5 (Beach Bar) = boot2root**

  1. source code of login page had `commented credentials`: "dj / dj"
  2. logged in, exported/imported playlist as `.yml` to figure out syntax
  3. import showed my `YAML` back as a `Python dictionary` -> app has Python backend
  4. exploited unsafe `YAML` load with payload:
  ```
  !!python/object/apply:subprocess.check\_output` + `- \["id"]` -> ran as bartender (`RCE` = Remote Code Execution)
  ```
  6. searched for and found _{user flag}_ at `/home/bartender/user.txt`

  <details>
    <summary>Reveal the 1st flag</summary>

  **_THM{y4ml\_pl4yl1st\_pwns\_th3\_b34ch}_**

  </details>
 
  7. got a `reverse shell` (`nc` listener + bash payload)
  8. enumerated: sudo/SUID/capabilities/cron all dead-end
  9. `ps aux | grep root` -> found root process leaking password: "SunsetSpritz2024!"
  10. `su root` with that password -> authorized as `root`
  11. searched for and found a _{root}_ flag at `/root/root.txt`
  
  <details>
    <summary>Reveal the 2nd flag</summary>

  **_THM{cr3d3nt14l\_r3us3\_4t\_th3\_b34ch\_b4r}_**

  </details>

`commented creds -> YAML deserialization RCE -> user flag -> leaked root password -> su root -> root flag`

