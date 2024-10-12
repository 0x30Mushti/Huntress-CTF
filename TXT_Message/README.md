# TXT Message

## Description
```
Hmmm, have you seen some of the strange DNS records for the ctf.games domain? 
One of them sure is odd...
```


## How to Solve the DNS Challenge

First, check the DNS records for the `ctf.games` domain. You can use the `nslookup` command as follows:

```
nslookup -type=ANY ctf.games
```

I found something unusual—a string in octal format:
```
146 154 141 147 173 061 064 145 060 067 062 146 067 060 065 144 064 065 070 070 062 064 060 061 144 061 064 061 143 065 066 062 146 144 143 060 142 175
```

To decode this octal string, you can use an octal-to-ASCII converter like the one found here: https://cryptools.dev/converters/oct-to-ascii.

The flag should be displayed after the conversion.