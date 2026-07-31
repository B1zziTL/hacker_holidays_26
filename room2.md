**Room 2 = directory enumeration - gobuster**

1. mentioned clues: quiet room (112), room 404, "room that isn't on the floor plan"
2. source code reveals only 1 functioning link "/booking"-> returns a `404 error`
3. "room that isn't on the floor plan" hints at unlinked/hidden directories
4. enumerating hidden paths with `gobuster` -> "/.git" came up
5. using `curl -i` on "./git" -> showed "Server: Werkzeug" (Flask app)
6. using `curl -s` on "./git/HEAD" -> `ref: refs/heads/main` (live downloadable git repository)
7. would download the git repo through `git-dumper`, BUT `pip install` is timing out, will use `wget -r` + `git checkout` instead
8. reconstructed repo contains "README.md" with flag: *THM{byt3\_l0tus\_n3v3r\_f0rg3ts}*



`source code search -> directory enumeration -> git rebuild -> flag`



