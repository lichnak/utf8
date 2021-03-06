﻿## UTF8 - Simple Library for Internationalization ## {#mainpage}

Functions to facilitate Windows handling of I18N problems using
the strategy advocated by [UTF-8 Everywhere Manifesto](http://utf8everywhere.org/).


Author:
Mircea Neacsu (mircea@neacsu.net)

# License #

The MIT License (MIT)
 
Copyright (c) 2014-2020 Mircea Neacsu

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

# Content #
The main function groups are:
- narrowing functions - narrow() - that go from UTF-16 or UTF-32 to UTF-8 encoding.
- widening functions - widen() - that go from UTF-8 to UTF-16
- widening functions - rune() and runes() - that go from UTF-8 to UTF-32

There are also functions for:
- character counting - length()
- string traversal - next()
- validity checking - valid()
- case folding - toupper() and tolower()

In addition to those, there are wrappings for:
- the most common file access operations: fopen(), access(), remove(), chmod(),
  rename()
- directory operations: mkdir(), rmdir(), chdir(), getcwd()
- path management: splitpath(), makepath()
- environment functions: getenv(), putenv()
- conversion of command-line arguments: get_argv() and free_argv()
- C++ I/O streams: \ref utf8::ifstream, \ref utf8::ofstream, \ref utf8::fstream
- Character classification functions: \ref charclass "is... (isdigit, isalnum, etc.)"
- A simple \ref utf8::buffer "buffer class" for handling Windows API parameters. 
- \ref utf8::IniFile - a class for handling Windows "profile files" (also
  called INI files) API".
- Frequently used Windows functions like MessageBox and LoadString.

# Usage #
Before using this library you might want to review the guidelines from the
[UTF-8 Everywhere Manifesto](http://utf8everywhere.org/). In particular:
- define UNICODE or _UNICODE in your program
- for Visual Studio users, make sure "Use Unicode Character Set" option is defined
  under "General" > "Project Defaults" tab.
- for Visual Studio users, add "/utf-8" option under "C/C++" > "All Options" >
  "Additional Options" (see https://docs.microsoft.com/en-us/cpp/build/reference/utf-8-set-source-and-executable-character-sets-to-utf-8)
- use only *std::string* and <i>char*</i> variables. Assume they all contain UTF-8
  encoded strings.
- use UTF-16 strings only in arguments to Windows API calls.

All functions and classes in this library are included in the *utf8* namespace.
It is a good idea not to have a using directive for this namespace. That makes it
more evident in the code where UTF8-aware functions are used.

This is an example of a function call:
````
  string dirname = u8"ελληνικό";
  utf8::mkdir (dirname);   //create a directory with a UTF8-encoded name
````
Most functions mimic the behavior of standard C functions. A notable exception is
the access() function. The utf8::access() function returns a bool value while the
standard library function returns 0 if the file can be accessed.
 
Wrappers for C++ I/O streams can be used for file streams with UTF-8 encoded filenames.
This is an example of a C++ stream with a weird name and content:
````
  string filename = u8"ελληνικό";
  string filetext{ u8"😃😎😛" };

  utf8::ofstream u8strm(filename);

  u8strm << filetext << endl;
  u8strm.close ();
````
Translation from UTF-16 to UTF-8 applies only to file names. C++ streams are agnostic
about their content.

Calling Windows API functions can be handled using the generic widening and
narrowing functions like this example:
````
  string filename = u8"ελληνικό";
  HANDLE f = CreateFile (utf8::widen (filename).c_str (), GENERIC_READ, 0,
    NULL, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, NULL);
````

 
# Building #
The UTF8 library doesn't have any dependencies. The test programs however uses
the [UTTP library](https://github.com/neacsum/utpp).



