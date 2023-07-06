# How to change default fan speed by editing the vBIOS file (for AMD Polaris-series GPU)

## Tools:
- [HEX Editor](https://mh-nexus.de/en/hxd/)
- [AMD/ATIVBFlash](https://www.guru3d.com/files-details/amdvbflash-download.html)
- [PolarisBiosEditor](http://polaris-bios-editor.eu/)
- [AMD/ATI Pixel Clock Patcher](https://sourceforge.net/projects/amd-ati-pixel-clock-patcher/)
- [GOP Updater](https://e1.pcloud.link/publink/show?code=kZUzB8ZLrwnBMPHdzh9ibVNRd8uU01GcX77#returl=https%3A//e1.pcloud.link/publink/show%3Fcode%3DkZUzB8ZLrwnBMPHdzh9ibVNRd8uU01GcX77&page=login)
- Time and patience!

## Getting started:
**WARNING: USING MODDED BIOS WILL VOID YOUR WARRANTY. I ACCEPT NO RESPONSIBILITY FOR DAMAGE! PLEASE DO IT AT YOUR OWN RISK!**

**IMPORTANT: DUMP YOUR ORIGINAL vBIOS FILE AND CREATE A COPY BEFORE MODIFY IT!**

So I'm using my PowerColor RX 570 for the hackintosh rig, but the temperature is too high at idle because the fans aren't spinning (Zero Fan sth sth). I decided to modify my fan settings at BIOS level, since the macOS doesn't have any kind of program that can adjust the fan speed manually.

Now let's get to the real work:
- Dump your vBIOS file using GPU-Z, the button is on the right side of the BIOS version.

![g1](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/db1c6b1b-5bd6-4ed3-b49c-265d9cd46501)

- Create a copy of the original BIOS file, it should look like this:

![g2](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/ca07d70c-ca81-4479-8618-2645eb5a7b9d)

## The hardest, time-consuming: 
- Open vBIOS file (.rom file) using HEX Editor:

![g3](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/45072dcd-f8b2-46dc-bcc2-4daa7a982e4e)


- Click **Search**, then **Find**, change to Hex-values and type this value:
01 17 00 00 02

![image](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/a55e80d5-3f45-4f81-b9cd-ea6566f63bdf)

- After that, compare the value from this image with your vBIOS. If it's the same, then keep editing the vBIOS file.

  ![fan](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/fdd2b35d-ad90-4f66-b03d-874f140a4195)

- Now we know all the values are the same. Use the [Hexadecimal to Decimal converter](https://www.rapidtables.com/convert/number/hex-to-decimal.html) to calculate the value for each setting according to the values table.

> I recommend to using these value for the best result:

| Max Fan Speed | Min. Temp     | Med. Temp     | Max. Temp   | Min. PWM  | Med. PWM  | Max. PWM  |
| ------------- | ------------- | ------------- |------------- | ------------- | ------------- | ------------- |
|22   | D0 07  | 88 13  | 4C 1D | A0 0F | 70 17 | 1C 25 |

- Explaination:
  - Max Fan Speed: 22 -> 34 -> 34 x 100 = 3400  (it's the max RPM the fan is capable of spinning at, depends on your GPU's fan max RPM)
  - Min. Temp: D0 07 -> 7D0 -> 2000 -> 20° x 100
  - Med. Temp: 88 13 -> 1388 -> 5000 -> 50° x 100
  - Max. Temp: 4C 1D -> 1D4C -> 7500 -> 75° x 100
  - Min. PWM: A0 0F -> FA0 -> 4000 -> 40% x 100
  - Med. PWM: 70 17 -> 1770 -> 6000 -> 60% x 100
  - Max. PWM: 1C 25 -> 251C -> 9500 -> 95% x 100

- More explaination about the fan max RPM: Well, don't change that value to be honest, because the original BIOS from the manufacturer done it already.

- Well done! Now save your edited vBIOS file, and fire the PolarisBiosEditor up! After that, load the modified vBIOS file into it, it will say that checksum error, just click "OK" then click "SAVE AS"

![image](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/98a74ae8-3c43-480f-9295-8a500d876f03)

 **DON'T FORGET TO CHECK EVERY VALUE YOU CHANGED!!!**

 ![image](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/f7b86783-c905-493f-ac06-2fa062de2602)

- Extract the GOP Updater, then replace the amd_gop.efirom with the "magic GOP". Simply rename the file in archive RAR to amd_gop, then drag & drop:

[AMD GOP 1.69.0.15.50_magic_3B62F1AF_compr.efirom.zip](https://github.com/Pangorin/polaris-fan-speed/files/11966614/AMD.GOP.1.69.0.15.50_magic_3B62F1AF_compr.efirom.zip)

![image](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/bf63df14-4452-4c03-922c-b91ab59940e6)

- Almost there, now drag your modified vBIOS file to the GOPupd.bat (DRAG & DROP, NOT RUN!). And this table will show up, type "y", enter. Done! It's time to flash your modified BIOS.

![image](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/571e523d-ad24-4269-8046-dfec1dd3df8e)

![image](https://github.com/Pangorin/polaris-fan-speed/assets/68218885/ffe7c48b-37a1-4c91-914a-2d0486bb6c63)

- Run AMDVbFlash, then load your updated vBIOS file in the GOP Updater folder, click Program and wait. When it finished, this table will appeared. Click "No".

** EXTRACT THE `atikmdag-patcher`, drag & drop the `atikmdag-patcher.exe` to desktop, THEN RENAME IT TO `atikmdag-bios-patcher` AND RUN IT, CLICK YES ON WHATEVER THE PROGRAM SAID**

- Reboot your PC and see the result :D

**If you messed everything up, don't worry, your GPU isn't useless yet. Plug it into a PC that has integrated graphics, and flash the original BIOS back!**
