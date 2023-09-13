# Symmetric-key encryption

Write a command line interface (CLI) application that reads a passphrase as input.
Then gives the option to either encrypt a message and save it to a file, or
read a message from a file, decrypt and show it.

A passphrase is just a string of characters or words. Passphrases are longer than
usual passwords making them more secure.
So you can think of a passphrase as essentially just a long password.

- [Authenticated Encryption in .NET with AES-GCM](https://www.scottbrady91.com/c-sharp/aes-gcm-dotnet)
- [How to: Write text to a file](https://learn.microsoft.com/en-us/dotnet/standard/io/how-to-write-text-to-a-file)
- [How to: Read text from a file](https://learn.microsoft.com/en-us/dotnet/standard/io/how-to-read-text-from-a-file)

## Encodings

### Text encoding

There are many different ways to represent text as bytes.
Unicode is a system design to support text in all writing systems and any glyphs
that are commonly used independent of culture. Even emojis with different skin
color.

.NET uses UTF-16 (16-bit Unicode Transformation Format) internally for strings,
which represent most common characters in 2 bytes, but will expand to 4 bytes to
support all of Unicode.

UTF-8 is sometimes preferred as it represents characters in the latin alphabet
with only 1 byte.

```c#
byte[] binary = Encoding.UTF8.GetBytes("Witaj świecie");
string text = Encoding.UTF8.GetString(binary);
```

Note: The byte array in example above, will have a length of 14 instead of 13. Because "ś" is not a letter in standard latin alphabet.

### Binary encoding

Not all data are text. We call that binary data.

Examples: Cute cat photos. Or that video clip of your friend doing something
stupid while drunk.

Sometimes it can be convenient to represent binary data as text. Say you want to
visually compare to binary values, or you need to transfer binary data over a
protocol that only supports text.

One way is with hex. Representing 4 bits with 1 character:
```c#
string hexString = Convert.ToHexString(binaryByteArray);
byte[] binaryByteArray = Convert.FromHexString(hexString);
```

Another is Base64. Representing 6 bits with 1 character:
```c#
string b64String = Convert.ToBase64String(binaryByteArray);
byte[] binaryByteArray = Convert.FromBase64String(b64String);
```

## CLI

As an example, a session of using the program might look like this:

```
Passphrase: 
> ******
--------------------------------------------------------------------------------
1: Safely store message
2: Read message
0: Exit
> 1
Type a message to encrypt:
Hello World!
--------------------------------------------------------------------------------
1: Safely store message
2: Read message
0: Exit
> 2
Hello World!
--------------------------------------------------------------------------------
1: Safely store message
2: Read message
0: Exit
> 0
```

You can adjust however you like.

## macOS

`AesGcm` depends on platform provided crypto libraries.
macOS comes with an older version of OpenSSL that doesn't support GCM.

You can install a newer version of OpenSSL using homebrew.

Install homebrew if you don't have it already:
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Use brew (short for homebrew) to install openssl:
```sh
brew install openssl@3
```

Add following line to .zshrc so it knows where to look for the lib.
```sh
export DYLD_LIBRARY_PATH=/opt/homebrew/opt/openssl@3/lib
```

## Writing CLI programs

For simplicity we use a CLI for this assignment.

Since you may have had limited experience with writing CLI apps in .NET/C# -
here are some pointers.

To create a new CLI project in current folder:
```sh
dotnet new console
```

Remember that you can use `cd` to navigate between folders and `mkdir` to create
a new one.

Write/print a line of text:
```c#
Console.WriteLine("Hello World!");
```

Write just a character:
```c#
Console.Write('x');
```

Read a line of text:
```c#
string? enteredText = Console.ReadLine();
```

Read just a single key press without displaying it:
```c#
ConsoleKeyInfo key = Console.ReadKey(intercept: true);
// Check if a specific key was pressed
bool isEnter = key.Key == ConsoleKey.Enter;
// Get the character that was pressed
bool character = key.KeyChar;
```

Change color of text:
```c#
Console.ForegroundColor = ConsoleColor.Yellow;
Console.BackgroundColor = ConsoleColor.DarkRed;
```
