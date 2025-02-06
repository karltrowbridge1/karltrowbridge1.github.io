---
title: 'John the Ripper'
description: "A writeup on how I used John the Ripper in a CTF to crack a zip file."
date: 2024-08-27T17:51:26-04:00
cover:
    image: images/john-the-ripper/passwordcrack.jpg
---


# What is John the Ripper

John the Ripper is a password cracker that supports many types of hashes or ciphers. The flexible software is able to run on several different kinds of operating systems utilizing CPU power or GPU power in some cases. A link to the GitHub repo for John the Ripper can be found [here](https://github.com/openwall/john).

When testing the strength or security of passwords during a penetration test/security audit a password cracker can identify weak credentials to highlight vulnerabilities within an organization and its infrastructure. Tools like John the Ripper can also prove useful in a forensic investigation where an unknown password may hold investigators from important data.

In my case, during a CTF for the PJPT I discovered a zip file that was password protected. A lot less exciting, but still fun and educational!

# Using John

John the Ripper has a unique but simple way of documenting its functionality. After downloading and unzipping the app, there are 3 directories; `doc`, `etc`, and `run`.

---
`doc` contains the documentation related to John the Ripper and how to use it.

`etc` holds vendor files  - I didn't touch these.

and `run` has the executables files, wordlists and other resources.

---

First, I needed to get the hash from the file. In the documentation there is a file called `README-ZIP.txt` which contains all we need to know about cracking zip files with this software, and it is *dirt simple*

To extract the hash for John to use, we run a program called `zip2john` in the `run` directory. The first argument is the zip file, and we can redirect this output into a new file like so:

![zip2john-command](/blog/john/zip2john.png)

With this file I can run John and feed it the hash contents for it to crack.

![password-found](/blog/john/password.png)

And success! The password is `java101`. Onto the next step...