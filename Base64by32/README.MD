# Base64by32

## Description
```
This is a dumb challenge. I'm sorry.
```

What you want to do is decode the string **32 times** using Base64. Below is a sample Python script to achieve this:

```
import base64

file_path = r"enter_file_path" 

with open(file_path, 'r') as file:
    encoded_content = file.read()

for _ in range(32):
    encoded_content = base64.b64decode(encoded_content.strip()).decode('utf-8')

print(encoded_content)
```