**Room 6 = OSINT**

1. clue given "track down an account nobody was supposed to find" -> user-account mentioned in the given screenshot: "lambobytelotushotel@gmail.com"
2. conversation hints at a platform that allows linking social media (eg. Linktree), starting with "G" -> `Gravatar`
3. `Gravatar` uses `MD5-hashing` of the user's email -> "gravator.com/d4a5fc5d3128890778667e24617d7cc0"
4. leads to "https://gravatar.com/cheerfullysongf28e3c3716" -> reveals "VEhNe1MzY3JlVF9QcjBmaWwzX0g0c19iMzNuX0lkZW50MWZpM2R9"
5. the given hash isn't `MD5`, but could be `Base64` -> flag: *THM{S3creT\_Pr0fil3\_H4s\_b33n\_Ident1fi3d}*



`analysis -> OSINT -> MD5 hashing -> Base64 decoding -> flag`

