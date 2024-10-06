# Unbelievable

## Description
```
Don't believe everything you see on the Internet!

Anyway, have you heard this intro soundtrack from Half-Life 3?
```
## Steps to Solve the CTF:

file <filename>

```
Half-Life_3_OST.mp3: PNG image data, 800 x 200, 8-bit/color RGB, non-interlaced
```

If you rename a .png picture file with a .mp3 extension, Windows will think it's an audio file. However, since the file content is still a picture, it won't play properly as an audio file, and media players will fail to open or produce an error. The file's actual format doesn't change, only the extension does.

The first eight bytes of a PNG file always contain the following (decimal) values:
```
89  50  4e  47  0d  0a  1a  0a
```
Lets open the file with Binary Ninja and dubble check that it matches. 
```
89  50  4e  47  0d  0a  1a  0a
```
Seems like a .PNG to me. 

Lets make it a .PNG

- Lets open this .mp3 in a text-editor.
- Save as "All types".
- Remove the .mp3
- Add .PNG. 

Open the .PNG and the flag will be there.

Answer: REDACTED




