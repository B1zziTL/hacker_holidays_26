**Room 14 (Management Wants a Word) = DFIR - Cryptography**



1. download the files and create a visual file tree for easier navigation, it immediately shows a few key info:
• Vera's SID: `S-1-5-21-2529683458-431225740-1723070931-1000`
• Master key file (GUID): `c90719ef-5b98-474e-b934-136d606a702a`
• `/KAPE/C/Users/vera/Documents/backup` = the locked Vault containing the flag
2. "version number 1.26.29" in the clue refers to VeraCrypt, will be useful later
3. `/KAPE/C/Windows/System32/config` contains `SAM` (Windows password hashes) and `SYSTEM` (key to unlock `SAM`)
4. `impacket-secretsdump -sam SAM -system SYSTEM LOCAL` reveals `vera:1000:aad3b435b51404eeaad3b435b51404ee:1241186a4aac4f34f4bf7ace71b396a8:::` -> save to "hash.txt"
5. brute-force it using `john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt` -> password revealed as "minivera"
6. make a new directory (eg. vera) and move \& rename these files (for easier manipulation)
• `/Users/vera/AppData/Roaming/Microsoft/Protect/S-1-5-21-2529683458-431225740-1723070931-1000/c90719ef-5b98-474e-b934-136d606a702a` -> masterkey
• `/Users/vera/AppData/Roaming/Microsoft/Protect/S-1-5-21-2529683458-431225740-1723070931-1000/Preferred` (stays the same)
• `/Users/vera/AppData/Local/Google/Chrome For Testing/User Data/Local State` -> LocalState
• `/Users/vera/AppData/Local/Google/Chrome For Testing/User Data/Default/Login Data` -> LoginData
7. run 3-step decryption using `pypykatz`
```
SID="S-1-5-21-2529683458-431225740-1723070931-1000"
pypykatz dpapi prekey password "$SID" "minivera" -o prekeys.txt
pypykatz dpapi masterkey masterkey prekeys.txt -o mkeys.txt
pypykatz dpapi chrome mkeys.txt LocalState --logindata LoginData
```
8. decryption revealed `file: LoginData user: VeraSecretVault pass: b'Wh4t1sV3raD0inG0nTh1sH0st' url: http://bytelotus.thm:8080/login`
9. from the folder with backup file use `sudo cryptsetup --type tcrypt --veracrypt open backup veravault` and enter the passphrase above
10. mount the vault and navigate inside -> suspicious folder called "secret\_financial\_documents" contains a `.pdf` with the \_{flag}\_


<details>

&#x20; <summary>Reveal the flag</summary>



&#x20; \*\*\_THM{1t\_w4s\_V3r4\_A11\_Al0ng?!}\_\*\*



</details>



```

management-wants-a-word...\\KAPE\\C

│

├── Users

│   ├── Default

│   │   └── NTUSER.DAT (+ .LOG1/.LOG2)

│   │

│   └── vera

│       ├── NTUSER.DAT (+ .LOG1/.LOG2)

│       ├── Documents

│       │   └── ⭐ backup          ← 100 MB — the locked VAULT (flag is in here)

│       └── AppData

│           ├── Roaming\\Microsoft\\Protect\\S-1-5-21-2529683458-431225740-1723070931-1000

│           │   ├── ⭐ Preferred

│           │   └── ⭐ c90719ef-5b98-474e-b934-136d606a702a   ← DPAPI master key

│           └── Local

│               ├── Microsoft\\Windows\\UsrClass.dat (+ .LOG1/.LOG2)

│               └── Google\\Chrome For Testing\\User Data

│                   ├── ⭐ Local State                 ← Chrome's encrypted key

│                   ├── Default

│                   │   ├── ⭐ Login Data              ← Chrome's saved passwords

│                   │   ├── Login Data For Account

│                   │   ├── History, Cookies, Web Data, Preferences …

│                   │   └── (dozens of cache/db folders — ignore)

│                   └── (BrowserMetrics, Safe Browsing, ShaderCache … — ignore)

│

└── Windows

&#x20;   ├── ServiceProfiles

&#x20;   │   ├── LocalService\\NTUSER.DAT (+ logs)

&#x20;   │   └── NetworkService\\NTUSER.DAT (+ logs)

&#x20;   └── System32\\config

&#x20;       ├── ⭐ SAM      (+ .LOG1/.LOG2)   ← Windows password hashes

&#x20;       ├── ⭐ SYSTEM   (+ .LOG1/.LOG2)   ← key to unlock SAM

&#x20;       ├── SECURITY (+ logs)

&#x20;       ├── SOFTWARE (+ logs)

&#x20;       └── DEFAULT  (+ logs)

```



`visual file tree -> use SYSTEM to unlock SAM -> brute-force the hash -> 3-step pypykatz decryption -> navigate to vault -> reveal suspicious .pdf -> flag`



