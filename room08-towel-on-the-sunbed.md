**ROOM 8 (Towel on the Sunbed) = API - BurpSuite**

1. open in `BurpSuite` -> create a new account on the given app and claim the daily reward
2. upon claiming the reward, record the `POST request` -> simple repeat won't work, needs new `cookie`
3. create a fresh account without claiming the reward + copy a fresh `cookie` from the unclaimed account
4. using `Turbo Intruder` extension -> `POST` the claim 20 times to cause an overflow
5. now there is enough credits to access the "Whale Vault" and see the _{flag}_

<details>
  <summary>Reveal the flag</summary>

  **_THM{t0w3l\_0n\_th3\_sunb3d\_d0ubl3\_sp3nt}_**

</details>

`BurpSuite session -> registration -> swarm POST request with fresh cookie -> flag`

