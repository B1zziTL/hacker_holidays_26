**Room 0 = OSINT - metadata**



1. first try was focusing on hidden metadata of provided PNG, using `exiftool` -> it turned out clean
2. the room mentioned Instagram account -> lead to a search of IG account "@thebytelotusresort"
3. Instagram scrapes metadata, so no use probing the posted photos
4. above mentioned IG account only followed 1 account, the concierge Vera -> "@veratheconcierge"
5. Vera's IG account shared 3 posts, labeled in parts 1-3
6. based on the "==" padding at the end of the shared code, it is identifiable as a `Base64` string broken into 3 parts
7. decoding gets the flag: *THM{V3r@s\_aCC0unt\_h4s\_b33n\_f0und!}*



`OSINT -> Base64 decoding -> flag`



