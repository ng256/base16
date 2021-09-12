# Description.

__Base16 Encoding Utility__ is a small but useful console application for MS Windows operating system that allows you to convert your data using Base16 encoding and provides additional tools to manage the formatting of the hex dump output. Application input can be obtained from files, command line, keyboard or by redirecting output from another console application.  
The Base16 algorithm provides encoding in such a way that each byte of data is split into two 4-bit values and represented by two hexadecimal digits.  
Also, this utility can restore the original data from text containing a hex dump.

## System requirements.
Microsoft Windows XP (or later) operating system with Microsoft .NET Framework 4.0 installed.

## Contents.
__Base16_Install.zip__       Contains the installer that copies the __base16.exe__ file to the Program Files folder and adds the installed path to %PATH% environment variable. You must restart your computer after installation for the changes to take effect.   
__Base16_Standalone.zip__    Contains only __base16.exe__ executable file and __readme.txt__ document.  
__Base16_Source.zip__        Contains __base16.cs__ source code file for you to download and modify as you see fit.  

## Features.

:heavy_check_mark: Encode data.  
:heavy_check_mark: Decode hex dumps.  
:heavy_check_mark: Read files, text input or redirected output from console apps.  
:heavy_check_mark: Write result to file or console window.

# Usage.

This utility must be called from the command line with the specified arguments:  

base16 [-e|-d] [-s] [-delimiter char] [-prefix prefixstr] [-postfix postfixstr] [-l] [-w width] [-sfx] [-c] [-o outfile] [-f] file1 [file2...] [-t text] [-i]

## Program operation mode.

 __-e__                      Encode data. This is default choise.

 __-d__                      Decode data.


## Parameters that are used only for encoding.

 __-s|-space__               Group bytes in the output with spaces.

 __-delimiter {char}__       Use the specified delimiter char instead spaces. Can be used instead -s key.

 __-prefix {string}__        Use the specified prefix string for every byte.

 __-postfix {string}__       Use the specified postfix string for every byte except the last item.

 __-l|-lcase__               Convert output to lowercase.

 __-w|-wrap {width}__        Split the specified number of characters into lines. A value of this parameter less than 2 will be ignored. By default, the output will not wrap.

 __-sfx__                    Write a special command lines before the encoded data to create a self-extracting batch file. Items such as -s, -prefix, -postfix and -delimiter will be ignored.

 __-c__                      Create an array declaration for a C-like language. Items such as -s, -prefix, -postfix and -delimiter will be ignored.



## Configuring input and output.

 __-o|-output {outfile}__    Set output to file {outfile}. If parameter is omitted, program's output will be redirected to the console window.

 __{file1} {file2} ...__     Input files containing data to be encoded.

 __-f|-file {value as file name}__        Force use value as input filename (to escape parameters).  
If input files is omitted, program's input will be redirected to the standard input. Instead of a file name, you can specify a directory and file mask to search for files.  

 __-t|-text {text for encoding/decoding}__        Use typed text value instead of input. This stuff should be after all other arguments.  
 
 __-i|-input__				           Read data from standard input device until Ctrl+C pressed. All listed files or key -t will be ignored.


## Examples of using.

 __base16 file1.txt__  

Will display encoded data of file "file1.txt".
____
 __base16 file1.txt > file2.txt__  
 or  
 __base16 file1.txt -o file2.txt__  

Will save encoded data from "file1.txt" to "file2.txt". 
____
 __base16 -s file1.txt -o file2.txt__  

Will save encoded data from file "file1.txt" to output "file2.txt", separated by bytes.
____
 __echo Foo | base16 -s__  

Will display: _46 6F 6F 20 0D 0A_.
____
 __echo Bar | base16 -s -prefix 0x -postfix ,__  

Will display: _0x42, 0x61, 0x72, 0x20, 0x0D, 0x0A_.
____
 __base16 -t Hello, world!__  

Will display: _48656C6C6F2C20776F726C6421_.
____
 __base16 -i__  
Will input data from the keyboard and encode it.
____
 __echo 42 61 72 | base16 -d__  
 or  
 __base16 -d -t 42 61 72__  

Will display: _Bar_.
____
 __base16 -s -w 16 -l -delimiter ; test.txt__  

Will display the encoded content of the file "test.txt" with a custom separator ";" between bytes.
____
 __type encoded.txt | base16 -d > original.txt__  
or  
 __base16 -d encoded.txt -o original.txt__  

Will output the decoded content of the file "encoded.txt" to a new file "original.txt".  
____
 __base16 -c -t Hello, world__  

Will display array declare:  
```c
{  
0x48, 0x65, 0x6C, 0x6C, 0x6F, 0x2C, 0x77, 0x6F, 0x72, 0x6C, 0x64  
}  
```
If you type __"base16 -c -t Hello, world | clip"__, this ad will be stored to the clipboard immediately.  
___
 __base16 -sfx -o hello.bat -t Hello, world__  

Will store encoded text into self-extracting batch file "hello.bat".  
The content of batch file "hello.bat" looks like this:

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
If you run "hello.bat", this script will ask for a filename and write "Hello, world" to it.
____
