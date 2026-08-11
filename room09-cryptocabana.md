**ROOM 9 (CryptoCabana) = Azure Key Vault - Azure CLI**

1. upon inspection of the given URL's page source, there is an `app.js` script linked
2. clues point to a "value that is freshly rotated" -> hardcoded `SAS token` in the `.js` script
3. thanks to the hardcoded credentials, it is possible to list all containers by `az storage container list`
4. 3 cointainers: `$web`, `backups`, `vault` -> clues mention a page where it "never once points you" -> `vault`
5. using the credentials we access the container `vault`, by `az storage blob list`
6. uncover and download both files: `backup-service-account.json` and `seed\_phrase.txt`
7. `.txt` file was empty, but the `.json` reveals: `client\_id`, `client\_secret`, `tenant\_id`, `key\_vault\_uri` -> using this to log in with the `Azure Key Vault` (as a robot account)
8. `az keyvault secret list` reveals 3 `key-shard`(s) and a `master-key`, `az keyvault secret show` reveals them
9. `key-shard-1` and `key-shard-2` contain parts of the flag, `key-shard-2` was rotated out, `master-key` is unavailable
10. `az keyvault secret list-versions` recovers the id of the previous version for `key-shard-2`, now possible to read this version directly -> 3 parts of the _{flag}_

<details>
  <summary>Reveal the flag</summary>

  **_THM{n0t\_ur\_k3ys\_n0t\_ur\_c01ns!}_**

</details>

`"app.js" -> hardcoded SAS token -> container content -> leaked secrets -> Key secrets version id recovery -> flag`

