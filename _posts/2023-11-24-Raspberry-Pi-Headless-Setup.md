---
layout: default
author: m0j0
---

Raspberry Pi Headless Installation with no monitor:

Firstly I have a Pi 4 with 8GB of RAM that I have barely used so today I decided to make use out off it by creating it headless on my old laptop with Pi imager and enabling SSH so hopefully I can just plug and play.

- Step one m0j0 is to burn an image with Pi Imager on a 128GB SD card.
- Step 2 is enable SSH with Pi Imager extended OS custom feature.
- Step 3 Double check the first 2 steps are fully completed and finish a project.
- Step 4 consider a NAS (Network Allocated Storage)

### Let’s begin:

I fire up Pi imager on my Ubuntu laptop.

My settings:

![pi_imager](/assets/images/pi_imager.png)

Pressing next I get to add custom features which will be SSH enabled by adding some of my network info. Look at the new options when I select to edit:


![custom_os](/assets/images/os_custom.png)

Add some configuration needed such as passwords and SSID, Timezones, User and Password.

![settings](/assets/images/settings.png)

SSH enabled:

![ssh](/assets/images/ssh.png)

Final chance to check configuration and hope there are no typos and I am on my way to some fun (:

![Untitled](/assets/images/final_burn.png)

And we are burning the new image. Smoke break (Yes it’s bad but my only vice).

And I got a winner (:

------------------------------------------------------------------------------

```bash
m0j0@r1s1n: ~
$ ssh m0j0@raspberrypi                                                                                                                     [17:18:39]
The authenticity of host 'raspberrypi (192.168.0.112)' can't be established.
ED25519 key fingerprint is SHA256:GoAAyyG9nIm9loP0Xq7Z8NeqUotuczHqizGi/tCtkFQ.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'raspberrypi' (ED25519) to the list of known hosts.
m0j0@raspberrypi's password: 
Linux raspberrypi 6.1.0-rpi4-rpi-v8 #1 SMP PREEMPT Debian 1:6.1.54-1+rpt2 (2023-10-05) aarch64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Oct 10 05:07:17 2023
m0j0@raspberrypi:~ $ uname -a
Linux raspberrypi 6.1.0-rpi4-rpi-v8 #1 SMP PREEMPT Debian 1:6.1.54-1+rpt2 (2023-10-05) aarch64 GNU/Linux
m0j0@raspberrypi:~ $ id
uid=1000(m0j0) gid=1000(m0j0) groups=1000(m0j0),4(adm),20(dialout),24(cdrom),27(sudo),29(audio),44(video),46(plugdev),60(games),100(users),102(input),105(render),106(netdev),115(lpadmin),993(gpio),994(i2c),995(spi)
m0j0@raspberrypi:~ $ hostname
raspberrypi
m0j0@raspberrypi:~ $
```
------------------------------------------------------------------------------

Now I need to go over my steps again to see my next step of this project!!

Well as discussed in our [Discord](https://discord.gg/8cTtGrq27y) I went off on a tangent with my thought train
and ended up testing the m0j0Pi with Hashcat against my hacktop.

I knew the results but it was fun and not much between them, maybe I can use this Pi for some work with HTB boxes.

### The Results

Pi 4 8GB vs Hacktop 16GB  hashcat cracking:

```
m0j0@r1s1nPi:~ $ echo -n "123456" | openssl dgst -sha512 | awk '{print $2}' > hashed.txt
m0j0@r1s1nPi:~ $ cat hashed.txt 
ba3253876aed6bc22d4a6ff53d8406c6ad864195ed144ab5c87621b6c233b548baeae6956df346ec8c17f5ea10f35ee3cbc514797ed7ddd3145464e2a0bab413
m0j0@r1s1nPi:~ $ hashcat -m 1700 -a 0 -r /usr/share/hashcat/rules/combinator.rule hashed.txt /opt/SecLists/Passwords/Leaked-Databases/rockyou.txt
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 3.1+debian  Linux, None+Asserts, RELOC, SPIR, LLVM 15.0.6, SLEEF, POCL_DEBUG) - Platform #1 [The pocl project]
==========================================================================================================================================
* Device #1: pthread--cortex-a72, 2849/5763 MB (1024 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 51

Optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash
* Uses-64-Bit

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 1 MB

Dictionary cache built:
* Filename..: /opt/SecLists/Passwords/Leaked-Databases/rockyou.txt
* Passwords.: 14344391
* Bytes.....: 139921497
* Keyspace..: 731563584
* Runtime...: 3 secs

ba3253876aed6bc22d4a6ff53d8406c6ad864195ed144ab5c87621b6c233b548baeae6956df346ec8c17f5ea10f35ee3cbc514797ed7ddd3145464e2a0bab413:123456
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1700 (SHA2-512)
Hash.Target......: ba3253876aed6bc22d4a6ff53d8406c6ad864195ed144ab5c87...bab413
Time.Started.....: Fri Nov 24 19:43:45 2023 (0 secs)
Time.Estimated...: Fri Nov 24 19:43:45 2023 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/opt/SecLists/Passwords/Leaked-Databases/rockyou.txt)
Guess.Mod........: Rules (/usr/share/hashcat/rules/combinator.rule)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   248.1 kH/s (9.90ms) @ Accel:256 Loops:51 Thr:1 Vec:2
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 52224/731563584 (0.01%)
Rejected.........: 0/52224 (0.00%)
Restore.Point....: 0/14344384 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-51 Iteration:0-51
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> bethany.
Hardware.Mon.#1..: Util: 26%

Started: Fri Nov 24 19:41:38 2023
Stopped: Fri Nov 24 19:43:48 2023
```

**Hacktop**

```
m0j0@r1s1n: /tmp
$ hashcat -m 1700 -a 0 -r /usr/share/hashcat/rules/combinator.rule hashed.txt /opt/SecLists/Passwords/Leaked-Databases/rockyou.txt         [19:48:07]
hashcat (v6.2.5) starting

OpenCL API (OpenCL 2.0 pocl 1.8  Linux, None+Asserts, RELOC, LLVM 11.1.0, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=====================================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-7200U CPU @ 2.50GHz, 6870/13805 MB (2048 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 43

Optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash
* Uses-64-Bit

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 1 MB

Dictionary cache hit:
* Filename..: /opt/SecLists/Passwords/Leaked-Databases/rockyou.txt
* Passwords.: 14344384
* Bytes.....: 139921497
* Keyspace..: 616808512

ba3253876aed6bc22d4a6ff53d8406c6ad864195ed144ab5c87621b6c233b548baeae6956df346ec8c17f5ea10f35ee3cbc514797ed7ddd3145464e2a0bab413:123456
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1700 (SHA2-512)
Hash.Target......: ba3253876aed6bc22d4a6ff53d8406c6ad864195ed144ab5c87...bab413
Time.Started.....: Fri Nov 24 19:49:17 2023 (0 secs)
Time.Estimated...: Fri Nov 24 19:49:17 2023 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/opt/SecLists/Passwords/Leaked-Databases/rockyou.txt)
Guess.Mod........: Rules (/usr/share/hashcat/rules/combinator.rule)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   395.4 kH/s (33.36ms) @ Accel:256 Loops:43 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests
Progress.........: 44032/616808512 (0.01%)
Rejected.........: 0/44032 (0.00%)
Restore.Point....: 0/14344384 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-43 Iteration:0-43
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> bethany.
Hardware.Mon.#1..: Temp: 53c Util: 23%

Started: Fri Nov 24 19:48:29 2023
Stopped: Fri Nov 24 19:49:19 2023
```

Clear winner the Hacktop took 50 secs and the m0j0Pi took 2 mins 10 secs. This makes me think can I clock the Pi (:

This is a test as my auth has failed.
