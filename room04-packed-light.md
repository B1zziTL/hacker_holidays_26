**Room 4 (Packed Light) = Wireshark - network traffic**

1. room hints at decoding a capture of recovered network traffic
2. download includes a `.pcapNG` file (packet capture)
3. through Wireshark will investigate "Profile Hierarchy" and "Conversations"
- "Profile Hierarchy" -> using `TCP HTTP`
- "Conversations" -> seeing a surge of 616 bytes traffic to `port 8080` (the suspicious packets)
4. based on the findings and filtering, using `Follow HTTP Stream`, I find a `.py` file
5. the file is a `keylogger malware`, working on basis of `Base64 encoding` all captured keystrokes -> not directly readable
6. `reverse-engineering` the malware would be pulling the cookies from all beacon packets -> `Base64` decode each -> `XOR` (with provided key) -> concantenate in packet order builds the _{flag}_

<details>
  <summary>Reveal the flag</summary>

  **_THM{V3r4\_1s\_w4tch1ng\_0veR\_y0u}_**

</details>

`network traffic capture analysis -> Wireshark -> reverse-engineering of a keylogger (Base64 + XOR) -> flag`

