# base16

__Base16 Encoding Utility__ is a small but useful console application for MS Windows operating system that allows you to convert your data using Base16 encoding and provides additional tools to manage the formatting of the hex dump output. Application input can be obtained from files, command line, or by redirecting output from another console application.  
The Base16 algorithm provides encoding in such a way that each byte of data is split into two 4-bit values ​​and represented by two hexadecimal digits.  
Also, this utility can restore the original data from text containing a hex dump.

## System requirements.
Microsoft Windows XP (or later) operating system with Microsoft .NET Framework 4.0 installed.


## Features.

:heavy_check_mark: Encode data.  
:heavy_check_mark: Decode hex dumps.  
:heavy_check_mark: Read files, text input or redirected output from console apps.  

# Usage.

This utility must be called from the command line with the specified arguments:  

base16 [-e|-d] [-s] [-delimiter char] [-prefix prefixstr] [-postfix postfixstr] [-l] [-w width] [-sfx] [-c] [-o outfile] [-f] file1 [file2...] [-t text]

## Program operation mode.

 __-e__                      Encode data. This is default choise.

 __-d__                      Decode data.


## Parameters that are used only for encoding.

 __-s|-space__               Group bytes in the output with spaces.

 __-delimiter {char}__       Use the specified delimiter char instead spaces. Used only with the -s key.

 __-prefix {string}__        Use the specified prefix string for every byte.

 __-postfix {string}__       Use the specified postfix string for every byte except the last item.

 __-l|-lcase__               Convert output to lowercase.

 __-w|-wrap {width}__        Split the specified number of characters into lines. A value of this parameter less than 2 will be ignored. By default, the output will not wrap.

 __-sfx__                    Write a special command lines before the encoded data to create a self-extracting batch file. Items such as -s, -prefix, -postfix and -delimiter will be ignored.

 __-c__                      Create an array declaration for a C-like language. Items such as -s, -prefix, -postfix and -delimiter will be ignored.



## Configuring input and output.

 __-o|-output {outfile}__    Set output to file {outfile}. If parameter is omitted, program's output will be redirected to the console window.

 __{file1} {file2} ...__     Input files containing data to be encoded.

 __-f|-file {value as file name}__        Force use value as input filename (to escape parameters). If input files is omitted, program's input will be redirected to the standard input.

 __-t|-text {text for encoding/decoding}__        Use typed text value instead of input.



## Examples of using.

 __base16 file1.txt__  
Will display encoded data of file "file1.txt".
____
 __base16 file1.txt > file2.txt__  
Will save encoded data from "file1.txt" to "file2.txt". 
____
 __base16 -s file1.txt -o file2.txt__  
Saves encoded data from file "file1.txt" to output "file2.txt", separated by bytes.
____
 __echo Foo | base16 -s__  
Will display: _46 6F 6F 20 0D 0A_.
____
 __echo Bar | base16 -s -prefix 0x -postfix ,__  
Will display: _0x42, 0x61, 0x72, 0x20, 0x0D, 0x0A_.
____
 __base16 -t Helo, world!__  
Will display: _48656C6C6F2C20776F726C6421_.
____
 __echo 42 61 72 | base16 -d__  
Will display: _Bar_.
____
 __base16 -s -w 16 -l -delimiter ; test.txt__  
Will display the encoded content of the file "test.txt" with a custom separator ";" between bytes.
____
 __type encoded.txt | base16 -d -o original.txt__  
Output the decoded content of the file "encoded.txt" to a new file "original.txt".  
____
 __base16 -c -t Hello, world__  
Will display:  
_{  
0x48, 0x65, 0x6C, 0x6C, 0x6F, 0x2C, 0x77, 0x6F, 0x72, 0x6C, 0x64  
}_  
____
 __base16 -sfx -t Hello, world__  
Output encoded text into self-extracting batch file on the screen.  
The content on the screen of batch file looks like this:

```cmd
  :BEGIN  
  @ECHO OFF  
  SET /P filename="Enter filename: "  
  SET tmpfile=%~d0%~p0%RANDOM%.tmp  
  SET outfile=%~d0%~p0%filename%  
  ECHO tmpfile = %tmpfile%  
  ECHO outfile = %outfile%  
  FINDSTR "^[0-9A-F][0-9A-F][^\s]" %0 > "%tmpfile%"  
  certutil -decodehex "%tmpfile%" "%outfile%"  
  TIMEOUT 3  
  DEL /F /Q "%tmpfile%" %0  
  EXIT  
  
48 65 6C 6C 6F 2C 20 77 6F 72 6C 64
```
____
