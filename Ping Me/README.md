# Ping Me

## Description:
```
We found this file in the autoruns of a host that seemed to have a lot of network activity... can you figure out what it was doing?
```

## How to solve the CTF:

## Initial Script Examination:

The provided script was full of complex arithmetic and chr() function calls, which is often used to obfuscate the true intention of the code by making it difficult to read at first glance. Here is an example of a part of the script:


```
Execute chr(-8710+CLng(&H224A))&chr(CLng(&H1C3C)-7123)...
```

This line and the subsequent ones use the chr() function to convert hexadecimal and decimal values into ASCII characters, which would form a command or a string when evaluated.

## Safely Decoding the Script:

Instead of directly executing the script, which might run harmful commands, we can modify it to print the output instead of running it. This can be done by replacing Execute with WScript.Echo.

The WScript.Echo command prints the output to the console, allowing us to safely inspect the decoded script:
```
WScript.Echo chr(-8710+CLng(&H224A))&chr(CLng(&H1C3C)-7123)...
```
By using this approach, we could see that the script decoded into another VBScript that looked like this:
```
Dim sh, ips, i:
Set sh = CreateObject("WScript.Shell"):
ips = Array("102.108.97.103", "123.54.100.49", "98.54.48.52", "98.98.49.98", "REDACTED IP", "50.98.56.98", "REDACTED IP", "101.50.54.100", "53.49.53.56", "57.125.35.35"):
For i = 0 To UBound(ips):    
	sh.Run "cmd /Q /c ping " & ips(i), 0, False:
Next
```

This script initializes an array of IP addresses and attempts to silently run a series of ping commands on them in the background.

## Extracting the Hidden Message:

The real challenge seemed to lie within the array of IP addresses. Upon closer inspection, each segment of the IP addresses was in the range of ASCII characters (0-127). It appeared that the segments of the IP addresses were encoded characters.

The IP addresses were as follows:
```
"102.108.97.103", "123.54.100.49", "98.54.48.52", "98.98.49.98", ...
```

To [decode](https://www.dcode.fr/ascii-code) this into a readable string, we extracted each part of the IP addresses and converted them into ASCII characters. For example, the first IP 102.108.97.103 would map to the characters f.l.a.g.

Using a script to convert these IP segments into their ASCII equivalents, we obtained the following message:

flag{REDACTED}