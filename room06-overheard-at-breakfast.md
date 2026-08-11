**Room 6 (Overheard at Breakfast) = OSINT**

1. clue given "track down an account nobody was supposed to find" -> user-account mentioned in the given screenshot: _lambobytelotushotel@gmail.com_
2. conversation hints at a platform that allows linking social media (eg. Linktree), starting with "G" -> `Gravatar`
3. `Gravatar` uses `MD5-hashing` of the user's email -> _gravator.com/d4a5fc5d3128890778667e24617d7cc0_
4. leads to _https://gravatar.com/cheerfullysongf28e3c3716_ -> reveals "VEhNe1MzY3JlVF9QcjBmaWwzX0g0c19iMzNuX0lkZW50MWZpM2R9"
5. the given hash isn't `MD5`, but could be `Base64` -> decoding reveals _{flag}_ 

<details>
  <summary>Reveal the flag</summary>

  **_THM{S3creT\_Pr0fil3\_H4s\_b33n\_Ident1fi3d}_**

</details>

`analysis -> OSINT -> MD5 hashing -> Base64 decoding -> flag`

