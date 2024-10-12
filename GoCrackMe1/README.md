# GoCrackMe1

## Description:

```
TENNNNNN-HUT!

Welcome to the Go Dojo, gophers in training!

Go malware is on the rise. So we need you to sharpen up those Go reverse engineering skills. We've written three simple CrackMe programs in Go to turn you into Go-binary reverse engineering ninjas!

First up is the easiest of the three. Go get em!
```
### Key Elements of the Code

The function `main_main` is responsible for:
1. **Obfuscation**: The flag is hidden using XOR encryption.
2. **Memory Management**: Go's runtime functions manipulate memory and handle strings.
3. **Output**: After decrypting the string, the flag is printed.

## The Code Breakdown

Here’s an overview of the important parts of the code:

### The Encrypted String

The string `"0:71-44coc``3dg0cc3c`nf2cno0e24435f0n+"` is stored in an array called `v17`. This string is encrypted using XOR.

```
qmemcpy(v17, "0:71-44coc``3dg0cc3c`nf2cno0e24435f0n+", sizeof(v17));
```

### Decrypting the String
The string is XOR-encrypted with the key 0x56. The XOR operation is applied to each byte in the array v17.

```
for (i = 0LL; i < 38; ++i)
    v2[i] = v17[i] ^ 0x56;
```

### Converting to a String
After decryption, the data in v2 is converted back to a Go string.

```
runtime_slicebytetostring(0LL, v2, 38LL, v0);
```

### Checking a Condition
The decrypted string is then checked using this function:

```
main_checkCondition((bool)v5);
```

### Printing the Flag
If the string passes the condition check, the flag is printed using Go’s fmt_Fprintln function:

```
fmt_Fprintln(*(io_Writer_0 *)&v13, *(_slice_interface__0 *)&p_a, v7, v8);
```

**Open CyberChef**: Go to [CyberChef](https://gchq.github.io/CyberChef/).

**Input the Encrypted String**: `0:71-44coc``3dg0cc3c`nf2cno0e24435f0n+`.

**Use XOR**:
   - Add the `XOR` operation.
   - Set the key to `56` (hex).

**Result**: The decoded string is the flag
