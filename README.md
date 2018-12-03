##### Date: 27th - 28th Nov 2018
##### Event: HITB is back to DXB, the city of gold
##### Description: HITB-XCTF, 2018

Organization:
- theshepherdlab.org, a research lab by JD Security
- hackersbadge.com, a free and opensource badge making lab
- hackinthebox.org, that super hacking conference
- XCTF, XCTF League from China

We are finally back to Dubaii. The badge for Dubai this time is based on STMF103 and LoRa checkout [hackerbadge.com](hackersbadge.com) 

Once again, i received invitation to make a hardware hacking challange for XCTF. Considering CTF team might not have the right tools to mess around with. I decided to make the challange with the previous conference badge. 

more info for [previous ctf](https://github.com/xwings/ctf.jdhitb2018pek)

For this CTF, once you received the badge. You will be able to see this. (more or less, i changed abit before the challange)

![alt text](https://raw.githubusercontent.com/xwings/ctf.hitb2018dxb/master/pic/boardctf.jpg)


Previous Beijing CTF, we used LED and some firmware tweak. This time round will be buttons, gpio and bootloader.

Chllange Desc:
* cat /etc/flag by solving buttons and bootloader

To Fix:
* Buttons: GPIO 37, 37H for ON and 38L for OFF
* [Bootloader: ](https://breed.hackpascal.net/) a closesouce and free bootloader

Flag formation:
* rc.local will string the very last line of /dev/mtd0 and md5 the stings
* rc.local will md5hash /etc/shadow
* join the two md5 will be the flag


Entnter the Bootroom (Part of the tips):
```
# run the script
# power on the badge
# ping 192.168.1.1
# telnet
# hunt for btntest
# delete and line and hunt for the right gpio
# right gpio is the NON reset button

import socket
import time

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

s.bind(("192.168.1.2", 37540))
s.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)

network = '<broadcast>'

while True:
    s.sendto("BREED:ABORT", (network, 37541))
    time.sleep(0.1)
```

Capture The Flag:
* Use the python script and force the bdage enter into bootloader (aka DFU)
* run btntest, fix gpio
* Downloaded firmware from the XCTF portal
* Split squashfs backup /etc/shadow and patch it with blank password
* Marge new squashfs into firmware
* follow rc.local to get the actual flag
* Reboot and cat /etc/flag
* e053d76b66954751fce0d03e50a89486726325f078ba59944900f21440b79409

##### Firmware included in this repo

End result, ony one team able to solve this challange. Hardware CTF still not a trend and hope we can make more hardware challange in the future.
