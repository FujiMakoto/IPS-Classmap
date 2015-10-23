# Classmap
## Introduction
Classmap is a PHP script that maps unique ID numbers to your applications class files. These ID numbers are then maintained for use in error codes.

By maintaining and actively using a proper error code scheme, you can easily and transparently map the location of errors that are triggered in production.

This script was designed to be (mostly) compatible with Invison Power Services' own error code schema.

## Installation
To install and utilize this script, just upload classmap.php to your applications root directory. For example, */srv/http/ips/applications/yourappdir/classmap.php*

Then just execute the script from the command line. A map of class ID's should be outputted to your terminal and saved in your applications data directory.

## Error Formatting
The recommended format for error codes (which is the format used by IPS) is **ABC/D**

**A** is a number 1-5 indicating the severity,

| Severity | Description                                                                                               | Examples                                                                                         |
| -------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| 1        | User does something that is not allowed. Can happen in normal use.                                        | User did not fill in a required form element.                                                    |
| 2        | Action cannot be performed. Will not happen in normal clicking around, but may happen if a URL is shared. | User does not have permission to access requested page; Page doesn't exist.                      |
| 3        | Action cannot be performed. Will not happen in normal use.                                                | Secure key doesn't match; User submitted a value for a select box that wasn't in the select box. |
| 4        | Configuration error that may happen if the admin hasn't set things up properly.                           | Uploads directory isn't writable; Facebook application data was rejected.                        |
| 5        | Any error that should **never** happen.                                                                     | No login modules exist; Module doesn't have a defined default section.                         |

**B** is a short string indicating the application. You should try and make this unique, but limit it to roughly 3-5 characters.

**C** is a 3-digit number indicating the class in which the error occurred. ID numbers are split into range groups of 100 depending on the class type,

| Type       | Min | Max |
| ---------- | --- | --- |
| Sources    | 100 | 199 |
| Modules    | 200 | 299 |
| Extensions | 300 | 399 |
| Hooks      | 400 | 499 |
| Widgets    | 500 | 599 |
| Tasks      | 600 | 699 |
| Interface  | 700 | 799 |
| Setup      | 800 | 899 |
| Misc       | 900 | 999 |
If more than 100 files are present in any type, the ID number will reset with a suffix of A, B, C, and so on.

**D** is then an identifier error within the class. The first error code in the class is given 1, the second is 2, and so on.

### General Tips
* Use descriptive error messages and make use of the ability to show a different error message to admins where appropriate. If it's a severity 4 error, you probably want to show an admin message.
* The HTTP status code you use is important so make sure you set that properly. Don't use HTTP 500 for an error code with severity 1 or 2 or a HTTP 4xx error for a code with severity 4 or 5. HTTP 404 and 403 will usually be severity 2, HTTP 429 and 503 will usually be severity 1.

## License
```
The MIT License (MIT)

Copyright (c) 2015 Makoto Fujimoto

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```