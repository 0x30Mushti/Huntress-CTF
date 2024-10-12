# X-RAY

## Description: 
```
The SOC detected malware on a host, but antivirus has already quarantined it. 
Can you still make sense of what it does?
```


## How to Solve the CTF:

Antivirus software often employs techniques like XOR encryption to prevent quarantined malware from executing while it remains isolated. 
This ensures the malware is inactive and poses no threat to the system, but still allows for further analysis or decryption.

For this, we can use [DeXray](https://www.hexacorn.com/blog/category/software-releases/dexray/).

### Download and Set Up DeXray

1. Visit the DeXray website and copy the Perl code.
2. Create a file and paste the code into it.
3. Attempt to run the script.
4. If prompted about missing modules, install them by running:
```
cpan install module::name
```

### Execute DeXray

Once set up, run the following command:

```
./Dexray filename
```

This should output a file with a `.out` extension.

### Start the Analysis

To begin, examine the output file:
```
file x-ray.00000184_Defender.out

Output: PE32 executable (DLL) (console) Intel 80386 Mono/.Net assembly, for MS Windows
```

This indicates that the file is a 32-bit Windows DLL compiled as part of a Mono/.NET assembly for console applications on an Intel 80386 architecture. 

Let’s analyze this further using [dnSpy](https://github.com/dnSpy/dnSpy), a powerful open-source tool for decompiling, debugging, and editing .NET assemblies like .exe and .dll files. 
It's especially useful for reverse engineering, malware analysis, and other tasks where you need to inspect compiled .NET code.

The actual file name is [stage2.dll](stage2dll.png).

### Analyze the Classes

Upon inspection, you’ll find the following class of interest:

```
StageTwo @02000009
```


### Key Parts of the `StageTwo @02000009` Class

Within this class, there are some important methods relevant to the CTF:

#### `load()` Method (Hex-to-Byte Conversion)

The `load()` method converts a hex string into a byte array, which is essential for the XOR decryption process that will follow.

```
private static byte[] load(string hex)
{
    int length = hex.Length;
    byte[] array = new byte[length / 2];
    for (int i = 0; i < length; i += 2)
    {
        array[i / 2] = Convert.ToByte(hex.Substring(i, 2), 16);
    }
    return array;
}
```
### otp() Method (XOR Operation)
The otp() method takes two byte arrays and performs an XOR operation between them. 
One array represents the encrypted data, and the other serves as the key for decryption.
```
private static byte[] otp(byte[] data, byte[] key)
{
    byte[] array = new byte[data.Length];
    for (int i = 0; i < data.Length; i++)
    {
        array[i] = data[i] ^ key[i % key.Length];
    }
    return array;
}
```

### Main() Method
The Main() method orchestrates the decryption process by loading the encrypted hex strings, applying the XOR decryption, and printing the result:
```
public static void Main(string[] args)
{
    // Perform the main decryption process
    new StageTwo().main("", new StreamReader(Console.OpenStandardInput()));
    
    // Load the encrypted data and decryption key from hex strings
    byte[] array = StageTwo.load("REDACTED");
    byte[] array2 = StageTwo.load("REDACTED");
    
    // Perform XOR decryption
    byte[] array3 = StageTwo.otp(array, array2);
    
    // Convert the decrypted byte array into a string (the flag)
    string result = Encoding.UTF8.GetString(array3);
    Console.WriteLine(result);  // This should be the flag or the message
}
```

### Decode
I performed the decoding in Python, as I feel more comfortable with it.
Here's the Python version of the XOR decryption:
```
def load(hex_string):
    """Converts a hex string to a byte array."""
    return bytes.fromhex(hex_string)

def otp(data, key):
    """XORs the data with the key."""
    result = bytearray(len(data))
    for i in range(len(data)):
        result[i] = data[i] ^ key[i % len(key)]
    return bytes(result)

# Encrypted data and key as hex strings
hex_data = "REDACTED"
hex_key = "REDACTED"

# Step 1: Convert the hex strings to byte arrays
data = load(hex_data)
key = load(hex_key)

# Step 2: XOR decrypt the data with the key
decrypted_data = otp(data, key)

# Step 3: Convert the decrypted bytes to a string (UTF-8)
decoded_message = decrypted_data.decode('utf-8', errors='ignore')

# Output the decoded message (the flag)
print("Decoded message:", decoded_message)
```

The Python decoder replicates the XOR decryption (one-time pad) from the C# otp() method. 
It takes two hex-encoded strings (data and key), converts them into byte arrays, and applies the XOR operation between them. 
The result is the decrypted data, which can then be converted into a readable string, revealing the original message or flag.

The script is going to print the flag.