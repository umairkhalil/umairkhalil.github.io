---
layout: post
title: Capture console output to clipboard
tags: [c++,c-sharp]
---

A console application that pipes output to the Clipboard.

In C++:

```cpp
#include "stdafx.h"
 
using namespace std;
 
int copyStringToClipboard (const string& s)
{
// Open the clipboard:
BOOL b = OpenClipboard (NULL);
if (!b)
{
cerr << "Could not Open Clipboard!" << endl;
return 1;
}
EmptyClipboard ();
 
// Get the Windows memory for the clipboard:
HGLOBAL clipMem = GlobalAlloc (GMEM_MOVEABLE, s.length ()+1);
if (NULL == clipMem)
{
cerr << "Could not get memory for copy!" << endl;
return 2;
}
char* pClipMem = reinterpret_cast <char*> (GlobalLock (clipMem));
// Use the  runtime to copy the string:
strcpy (pClipMem, s.c_str ());
GlobalUnlock (clipMem);
// Close the clipboard.
SetClipboardData (CF_TEXT, clipMem);
CloseClipboard ();
return 0;
}
 
int copyStreamToClipboard (istream& inStream, ostream& o=cout)
{
// First, read the input.
string allStrs;
string str;
while (getline (inStream, str))
{
// Store it:
allStrs += str + "\r\n";
// Pipe it to the output:
o << str << endl;
}
 
return copyStringToClipboard (allStrs);
}
 
int main(int argc, char* argv[])
{
// Parse any input file name:
if (argc == 2)
{
ifstream in (argv[1]);
if (false ==in.is_open ())
{
cerr << "Input file: " << argv[1] << " not found." << endl;
return -1;
}
return copyStreamToClipboard (in);
} else
{
return copyStreamToClipboard (cin);
}
}
```

In C#:

```csharp
using System;
using System.Text;
using System.IO;
 
namespace Clipboard
{
class ClipboardWriter
{
 
static private void copyStreamToClipboard
(TextReader inStream)
{
StringBuilder allStrs = new
StringBuilder();
string str;
while (null !=
(str = inStream.ReadLine ()))
{
// Store it:
allStrs.Append (str);
allStrs.Append ("\r\n");
// Pipe it to the output:
System.Console.WriteLine (str);
}
System.Windows.Forms.Clipboard.SetDataObject
(allStrs.ToString (), true);
}
 
[STAThread]
static void Main(string[] args)
{
if (args.Length == 1)
{
try
{
TextReader inStream = new
StreamReader (args[0]);
copyStreamToClipboard (inStream);
}
catch (FileNotFoundException f)
{
System.Console.Error.WriteLine
("Input file: " + args[0] +
" not found.");
}
catch (DirectoryNotFoundException d)
{
System.Console.Error.WriteLine
("Input directory: " + args[0] +
" not found.");
}
catch (Exception i)
{
System.Console.Error.WriteLine
("Generic I/O Error");
}
}
else
{
copyStreamToClipboard (Console.In);
}
}
}
}
```
