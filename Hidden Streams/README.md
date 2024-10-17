# Hidden Streams 

## Description

```
Beneath the surface, secrets glide,
A gentle flow where whispers hide.
Unseen currents, silent dreams,
Carrying tales in hidden streams.

Can you find the secrets in these Sysmon logs?
```

## How to solve the ctf. 

The name of the CTF, "Hidden Streams," gives a strong hint about what it's focused on—Alternate Data Streams (ADS).

In the context of monitoring, Sysmon Event ID 15 is crucial for detecting the creation of these hidden file streams. ADS is a feature of the NTFS file system that allows additional data to be attached to a file without altering its visible size or contents.

This means that information or even malware can be hidden inside a file without showing up in normal file listings or affecting the file's apparent size. Because of this stealthy nature, Sysmon Event ID 15 is triggered when any ADS is created, which could indicate suspicious activity, such as hidden data or malware on the system.

Next, let’s filter the Sysmon event log for Event ID 15.


```
2024-08-28 00:19:11.899
EV_RenderedValue_2,00
2460
C:\Windows\system32\WindowsPowerShell\v1.0\PowerShell.exe
C:\Temp:$5GMLW
2024-08-28 00:00:22.726
SHA1=B1C3068058ADDF418D3E1418CD28414325B7A757,MD5=E754797031C6B367D0B6209092F34B3B,SHA256=F414CBA3A5D8C6EF18B1BE31F09C848447DDB37A5712E36EB7825E4E1EFAE868,IMPHASH=00000000000000000000000000000000
<REDACTED BASE64 STRING>
WIN-UL3TI0T0LM6\Administrator 
```
This looks promising. I found a base64-encoded string (redacted here). Once you decode that, the flag will be revealed.
