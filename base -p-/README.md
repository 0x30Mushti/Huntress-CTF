# Base -p-

## Description

```
That looks like a weird encoding, I wonder what it's based on.
```

## How to Solve This CTF

### Step 1: Inspect the File

First, let’s take a look at the file `based.txt`.


```
file based.txt

based.txt: Unicode text, UTF-8 text
```


## Step 2: Research and Decode

After researching and testing various base encodings, I stumbled upon **Base65536** on [this website](https://www.better-converter.com/Encoders-Decoders/Base65536-Decode). 
After decoding, I got the following result:

```
H4sIAG0OA2cA/+2QvUt6URjHj0XmC5ribzBLCwKdorJoSiu9qRfCl4jeILSICh1MapCINHEJpaLJVIqwTRC8DQ5BBQ0pKtXUpTej4C4lBckvsCHP6U9oadDhfL7P85zzPTx81416LYclYgEAOLgOGwKgxgnrJKMK8j4kIaAwF3TjiwCwBejQQDAshK82cKx/2BnO3xzhmEmoMWn/qdU+ntTUIO8gmOw438bbCwRv3Y8vE2ens9y5sejat497l51sTRO18E8j2aSAAkixqhrKFl8E6fZfotmMlw7Z3NKFmvp92s8+HMg+zTwaycvVQlnSn7FYW2LFYY0+X18JpB9LCYliSm6LO9QXvfaIbJAqvNsL3lTP6vJ596GyKIaXBnNdRJahnqYLnlQ4d+LfbQ91vpH0Y4NSYwhk8tmv/5vFZFnHWrH8qWUkTfgfUPXKcFVi+5Vlx7V90OjLjZqtqMMH9FhMZfGUALnotancBQAA
```
This result seemed more promising than other base encodings I had tried.

### Step 3: Use CyberChef to Decode

The next step was to paste the Base65536 result into **CyberChef** and use the **Magic** recipe.

The "Result snippet" column hinted that the content was a `.png` file.

In CyberChef, you can click the link in the "Recipe (click to load)" column, and the content will now be displayed as a `.png` image.

See below:

[before.png](before.png)
[after.png](after.png)

### Step 4: Analyze the Image

Download the `.png` image. There are 13 distinct colors in the image, and these different colors likely contain information that leads to the solution.

We can also translate the colors to their hex values. Let’s visit [Image Color Picker](https://imagecolorpicker.com/).

Upload the `.png` file to extract all the distinct hex color values.

### Step 5: Decode the Colors

After obtaining the hex color values, remove the `#` symbol from each value and paste them into **CyberChef**. Use the **Magic** operation again, and the flag will be revealed.

### Flag

The flag is displayed after running the above steps in CyberChef.