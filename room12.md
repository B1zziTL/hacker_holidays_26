**Room 12 (After Hours) = Windows - DFIR - Reverse Engineering**



1. Upon downloading, there are 3x `.map + 1x `.data + 1x `.brt files = `WMI repository`
2. Parsing the files through tools like "WMI\_Forensics" doesn't work, as advertised by the clues
3. I try to look for `Base64` encoding manually using `strings`, after decoding first:

```

$file = (\[WmiClass]'ROOT\\cimv2:Win32\_HardwareTelemetry').Properties\['ConfigData'].Value;

$o = New-Object IO.MemoryStream;

$d = New-Object IO.Compression.DeflateStream(\[IO.MemoryStream]\[Convert]::FromBase64String($file),\[IO.Compression.CompressionMode]::Decompress);

$b = New-Object Byte\[](1024);

$r = $d.Read($b,0,1024);

while($r -gt 0){

&#x20;   $o.Write($b,0,$r);

&#x20;   $r = $d.Read($b,0,1024);

```

4\. upon reading the decoded `Base64` code, it is visible a malicious payload was hidden into a class `Win32\_HardwareTelemetry` and a property `ConfigData` -> this will be the second found code

5\. I reverse engineer the second code by decompression -> reveals a `.NET` malware with a hidden "user password" (really another `Base64` code) -> flag: ***THM{P4tch\_op3ned\_th3\_BacKd00r}***

