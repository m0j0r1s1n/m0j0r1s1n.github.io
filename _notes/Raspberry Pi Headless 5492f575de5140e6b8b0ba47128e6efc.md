# Raspberry Pi Headless

## Installation with no monitor:

Firstly I have a Pi 4 with 8GB of RAM that I have barely used so today I decided to make use out off it by creating it headless on my old laptop with Pi imager and enabling SSH so hopefully I can just plug and play.

- Step one m0j0 is to burn an image with Pi Imager on a 123GB SD card.
- Step 2 is enable SSH with Pi Imager extended OS custom feature.
- Step 3 Double check the first 2 steps are fully completed and finish a project.
- Step 4 consider a NAS (Network Allocated Storage)

### Let’s begin:

I fire up Pi imager on my Ubuntu laptop.

My settings:

![Untitled](Raspberry%20Pi%20Headless%205492f575de5140e6b8b0ba47128e6efc/Untitled.png)

Pressing next I get to add custom features which will be SSH enabled by adding some of my network info. Look at the new options when I select to edit:

![Untitled](Raspberry%20Pi%20Headless%205492f575de5140e6b8b0ba47128e6efc/Untitled%201.png)

Add some configuration needed such as passwords and SSID, Timezones, User and Password.

![Untitled](Raspberry%20Pi%20Headless%205492f575de5140e6b8b0ba47128e6efc/Untitled%202.png)

SSH enabled:

![Untitled](Raspberry%20Pi%20Headless%205492f575de5140e6b8b0ba47128e6efc/Untitled%203.png)

Final chance to check configuration and hope there are no typos and I am on my way to some fun (:

![Untitled](Raspberry%20Pi%20Headless%205492f575de5140e6b8b0ba47128e6efc/Untitled%204.png)

And we are burning the new image. Smoke break (Yes it’s bad but my only vice).

And I got a winner (:

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

Now I need to go over my steps again to see my next step of this project!!