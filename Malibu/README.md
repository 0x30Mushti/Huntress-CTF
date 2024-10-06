# Malibu

## Description:

```
What do you bring to the beach?

As an extra tip, once you understand what the service is, try to connect in a more effective way. Use context clues and critical thinking based on its response and the challenge description. You won't need any brute-forcing once you grasp the infrastructure and enumerate effectively. ;)
```

## Steps to Solve the CTF:

First, we try to connect to the instance:

nc challenge.ctf.games <PORT> 

We don’t receive any instructions, so we attempt to type "HELLO":
```
wslario@LT-5CG411013H:/mnt/c/Users/Forensic/Desktop/CTF/huntress/Malibu$ nc challenge.ctf.games 30595 
HELLO 

HTTP/1.1 400 Bad Request 
Content-Type: text/plain; charset=utf-8 
Connection: close
```

HTTPS... seems interesting. Let's attempt to send a valid request.

After a few trials, I decided to enter the URL into the Chrome browser.

Based on the logs and the "Access Denied" response, I concluded that it was hosted on an Amazon instance:
```
HTTP/1.1 403 Forbidden 
Accept-Ranges: bytes 
Content-Length: 254 
Content-Type: application/xml 
Server: MinIO Strict-Transport-Security: 
max-age=31536000; includeSubDomains 
Vary: Origin 
Vary: Accept-Encoding 
X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8 
X-Amz-Request-Id: 17FB564C1D8644C9 
X-Content-Type-Options: nosniff 
X-Ratelimit-Limit: 59 
X-Ratelimit-Remaining: 59 
X-Xss-Protection: 1; 
mode=block Date: Fri, 04 Oct 2024 19:22:46 GMT

<?xml version="1.0" encoding="UTF-8"?>
<Error><Code>AccessDenied</Code><Message>Access Denied.</Message><Resource>/</Resource><RequestId>17FB564C1D8644C9</RequestId><HostId>dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8</HostId></Error>
```

So, I tried accessing `http://challenge.ctf.games:30595/public` and got a response. 

```
Error> <Code>AccessDenied</Code> <Message>Access Denied.</Message> <BucketName>public</BucketName> <Resource>/public</Resource> <RequestId>17FBE8F1434EA75E</RequestId> <HostId>dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8</HostId> </Error>
```
"Access Denied" is better than nothing. ;)

After being stuck and sleeping on it, I remembered that the CTF description was "What do you bring to the beach?"

So, I tried testing some bucket names, for example:

Sunscreen, Towel, Beer, Water, Food, Shovel, Bucket,

"Bucket" returned a result, and I got a lengthy XML response:
```
<Key>0g15hrKN/60Zl3u3n/eDEO9nrk/eXF2FPnTklap7751</Key> 
<LastModified>2024-10-05T09:39:40.739Z</LastModified> 
<ETag>"048f51cd14539300a90acccea8689bbb"</ETag> 
<Size>1569</Size> 
<Owner> <ID>02d6176db174dc93cb1b899f7c6078f08654445fe8cf1b6ce98d8855f66bdbf4</ID> 
<DisplayName>minio</DisplayName> 
</Owner> <StorageClass>STANDARD</StorageClass> 
</Contents> <Contents> 
<Key>0rjjHdbXvMFD2s7n</Key> 
<LastModified>2024-10-05T09:40:54.939Z</LastModified> 
<ETag>"39ba36310ee1bb288b7b763fa3d5d640"</ETag> 
<Size>1979</Size> <Owner> 
<ID>02d6176db174dc93cb1b899f7c6078f08654445fe8cf1b6ce98d8855f66bdbf4</ID> 
<DisplayName>minio</DisplayName> </Owner> 
<StorageClass>STANDARD</StorageClass> 
```

There were many more entries in the XML.

We could see the files, their sizes, last modified dates, and more. 

There were too many files to download by hand, so let's use a Bash script for this:

```
#!/bin/bash

# The base URL
BASE_URL="http://challenge.ctf.games:30595/bucket"

# Fetch the XML content from the bucket URL
XML_CONTENT=$(curl -s "$BASE_URL")

# Extract the Key values from the XML
KEYS=$(echo "$XML_CONTENT" | grep -oP '(?<=<Key>).*?(?=</Key>)')

# Loop through each key and download the corresponding file
for KEY in $KEYS; do
    # Replace any slashes in the Key to create a valid URL
    FILE_URL="$BASE_URL/$KEY"
    
    # Download the file
    echo "Downloading: $FILE_URL"
    wget -q --show-progress "$FILE_URL" -O "$(basename "$KEY")"
done

echo "Done"
```

chmod 777 ;) <scriptname>

Run the script.

When the script was executed, the files were downloaded:

```
strings * | grep flag 

54nO5a4Br2ezDsnfZSSIQ6L3775vwQxa8pv80HekjRBuXpFskLJe8bS6Uvp0EeIt5eM2ivuW3EdqzqUCzgPmqR9wBozLEhkCZy1eXmJxcdKs2eHPggQl37szhhOvwjSS1y9TLVJlk<span $${\color{red}flag{REDACTED_FLAG}xN63GRRxSLXQ1qMAhTF4xCmxvpgi62AakeiyDU4dtvrIGFum1xavs3RLyUm6n3qer8ycEbnc0YYtRtgiEtttY
````	
