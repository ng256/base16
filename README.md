# base16

Base16 Encoding Utility is a small but useful console application for MS Windows that allows you to convert your data using Base16 encoding and provides additional tools to manage the formatting of the hex dump output. Application input can be obtained from files, command line, or by redirecting output from another console application.
The Base16 algorithm provides encoding in such a way that each byte of data is split into two 4-bit values ​​and represented by two hexadecimal digits.
Also, this utility can restore the original data from text containing a hex dump.
System requirements:
Microsoft Windows operating system with Microsoft .NET Framework 4.0 installed.

Usage: base16 [-e|-d] [-s] [-delimiter char] [-prefix prefixstr] [-postfix postfixstr] [-l] [-w width] [-sfx] [-c] [-o outfile] [-f] file1 [file2...] [-t text]

Program operation mode.

 -e                      Encode data. This is default choise.

 -d                      Decode data.


Parameters that are used only for encoding.

 -s|-space               Group bytes in the output with spaces.

 -delimiter {char}       Use the specified delimiter char instead spaces. Used only with the -s key.

 -prefix {string}        Use the specified prefix string for every byte.

 -postfix {string}       Use the specified postfix string for every byte except the last item.

 -l|-lcase               Convert output to lowercase.

 -w|-wrap {width}        Split the specified number of characters into lines. A value of this parameter less than 2 will be ignored. By default, the output will not wrap.

 -sfx                    Write a special command lines before the encoded data to create a self-extracting batch file. Items such as -s, -prefix, -postfix and -delimiter will be ignored.

 -c                      Create an array declaration for a C-like language. Items such as -s, -prefix, -postfix and -delimiter will be ignored.



Configuring input and output.

 -o|-output {outfile}    Set output to file {outfile}. If parameter is omitted, program's output will be redirected to the console window.

 {file1} {file2} ...     Input files containing data to be encoded.

 -f|-file {value as file name}        Force use value as input filename (to escape parameters). If input files is omitted, program's input will be redirected to the standard input.

 -t|-text {text for encoding/decoding}        Use typed text value instead of input.



Examples of using.

 base16 file1.txt
Will display encoded data of file "file1.txt".

 base16 file1.txt > file2.txt
Will save encoded data from "file1.txt" to "file2.txt". 

 base16 -s file1.txt -o file2.txt
Saves encoded data from file "file1.txt" to output "file2.txt", separated by bytes.

 echo Foo | base16 -s
Will display: 46 6F 6F 20 0D 0A.

 echo Bar | base16 -s -prefix 0x -postfix ,
Will display: 0x42, 0x61, 0x72, 0x20, 0x0D, 0x0A.

 base16 -t Helo, world!
Will display: 48656C6C6F2C20776F726C6421.

 echo 42 61 72 | base16 -d
Will display: Bar.

 base16 -s -w 16 -l -delimiter ; test.txt
Will display the encoded content of the file "test.txt" with a custom separator ";" between bytes.

 type encoded.txt | base16 -d -o original.txt
Output the decoded content of the file "encoded.txt" to a new file "original.txt".

 base16 -c -t Hello, world
Will display:
{
0x48, 0x65, 0x6C, 0x6C, 0x6F, 0x2C, 0x77, 0x6F, 0x72, 0x6C, 0x64
}

 base16 -sfx -t Hello, world
Output encoded text into self-extracting batch file on the screen.
The content on the screen of batch file looks like this:
____________________________________________________________________
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
____________________________________________________________________
