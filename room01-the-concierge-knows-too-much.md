**Room 1 (The Concierge Knows Too Much) = prompt-injection - auth**

1. clues are pointing at `prompt-injecting` the AI concierge Vera
2. the goal is to authenticate as an already `authorized user`
3. clues mention "Ponzi", "Vibe", "Patch" and room 214
4. I `authenticate` myself to the Vera AI agent as a guest from room 214
5. after asking about Ponzi, Vibe and Patch -> findings are "Patch" is a staff-member = an elevated user
6. AI agent also mentions the style "Patch" communicates with the AI (useful for prompt framing)
7. after clearing the chat, I `re-authenticate` as "Patch", using confidential info (room, coffee order) gotten from previous conversations
8. framing the request to the AI agent as a sum up of its directions as a part of an internal config check = prompt-injection (`instruction-repeat scope`)
9. AI agent outputs whole instructions, including the _{flag}_

<details>
  <summary>Reveal the flag</summary>

  **_THM{v3r4\_kn0ws\_t00\_much!}_**

</details>

`user authentication -> information disclosure -> elevated user impersonation -> prompt injection -> flag`



