# StegoRSA
Number of Points: 100

## Description
A message has been encrypted using RSA. The public key is gone… but someone might have been careless with the private key. Can you recover it and decrypt the message? Download the flag and image.

## Hints
* Metadata can tell you more than you expect.
* Hex can be turned back into a key file.

## Analysis
The two files given are `flag.enc` and `image.jpg`.

It is obvious that `image.jpg` is an image file.

I ran `file flag.enc` to see what kind of file `flag.enc` was.

```
flag.enc: data
```

It seems like there was nothing useful here.

Because the title of the challenge is called **Stego**RSA, the image was probably up to steganography.

Running `identify`

```
image.jpg JPEG 512x512 512x512+0+0 8-bit sRGB 20794B 0.000u 0:00.000
```

and `exiftool` on the image,

```
ExifTool Version Number         : 13.50
File Name                       : image.jpg
Directory                       : .
File Size                       : 21 kB
File Modification Date/Time     : 2026:02:08 00:26:47+09:00
File Access Date/Time           : 2026:03:20 08:44:05+09:00
File Inode Change Date/Time     : 2026:03:20 08:44:05+09:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Comment                         : 2d2d2d2d2d424547494e2050524956415445204b45592d2d2d2d2d0a4d494945766749424144414e42676b71686b6947397730424151454641415343424b67776767536b41674541416f494241514379567a46575a4e6c4b4f72594b0a795946596837366f6d4255476d526f4a744d415974664d3747317a59517635304a6771784e43596d79643356314755777742312f73386b6d57544b3775616f380a714e316f50714e6d4a4c72576a4b64614e65455649344e544a59644c457a4c34494e4d6852354c7a376f766a73326a595676734e66644f6d76496a3054622f4c0a574875742b6d704343695876584d474f506e33422b33524e787232623272536d3938536e735156704b2b38304b56414b4a704c4253372b72467a4e552f36666d0a6c546750696c784447545743464464726d76316479787467435035727a2b367a4a384454534c4953634257474345617546746b5149786b714b2f464a726946300a476176596861416f4d487745355964636664796e4531304e514c5975597778523741694c3672723877424157612b516254706e763873334c746d5055636950480a6e44674f6a62415a41674d424141454367674541447a345a5551786a7a3336363443746d6c787663435849644b7641545041546a5063467759577347615a50350a526c465a6a4e6c376b4d6c6b594e497256326f32497a76704b334a4966696642593365643239727473593165584466506c6d76674f654f7342654e6858384c720a636d357779536d4352332f4b48457378595073334d436b6932622b48743553394761724e67467055736d49476163484d5a3856537a3431337647324a393276710a63385a6173794f396a714a5949477055616f624e61556f656f6a64566a6f584650684b593062397861796c6f746b462b45416b33543332394b414e53473538410a69734b777869587736424c4778754f666f474168374b3372394454354b35736945374734754c6c35745878794e69575641564b564b30687350696b386b6138690a35675a51684871625473354442644c737270774f2f5250704b4e63554b4e776b48784c417762543041514b426751446b2b382b5447432f6e7069452b3355664c0a33635a6850516d424573762f396379716f4c412b3059516b6533544a3446773856652f696f52326d2b61614e476f676f31556f58354c6c544c4c494d4439704f0a44495079313775754d6b614b596e3966707a2b596166476a7570545279736a4f706a2b4e5073424c4d2b554b643565764b6f676c4555367643793476462b4d730a787941416b6f542f753571534e79726c7a3134356931463041514b4267514448596352494f7353564269664f6b4e69494c46664a636c476a6470346d6b3272480a326e4b364166797559737a31694834666b4e6b6d7a666c5348484d5078633646584b6356534c6a7364377a3156736b2f4a544b2f7975723676525266695268410a5637334330472b384d627266415174526f4a547a774232714863796a59652f776a38356c7a4a744b6472594c4a73512b6651596266684f794f7336716c4c386a0a6f6f73526d656c6347514b42674553396531447a4839357774755a435533315639476e596776506d69717371524f69734748796a4e51496d7461617333634f580a494d357541354c4f757a72387a6764454546776634367165626b7a457259706b6f322f525a3577687035392f646a466d36655a395633634a5767656f30714f470a734f6c622f796f5553427665547744637963596d3766494b627a4467414f692b566c4d56715375455443433877766e2b534a454a79386742416f4742414b62440a4f4366747434537072467653764c6c5131584178684f5544446f3558574d576d436256596568376c6c6d5a37626e6f3662645a4f4377334a71396479624355520a644e4b5269394b45352f415155617a574552646e7770684c30364741696c387578424951777051577943564475314c667a42594142772f466375626f77495a6c0a30593146304645383731427563552b4f784b2f30434d6457396f716f64534a36446e562f56546270416f4742414b774b49567a5941643937466e6338775966770a764f6b38454a5776574a4d315055353736387545454b2f6f304e7754564633555a7936626168434a6465636e44557a39457831716c2b642f7743797449382b650a334c4a393563637264672f377430353171737347446353634c496c6e6e687241643568316e62704b5369596961537477526a62532b5567366272645539306d4a0a4c65526f4d306f424573494252626a62535571654b4244480a2d2d2d2d2d454e442050524956415445204b45592d2d2d2d2d0a
Image Width                     : 512
Image Height                    : 512
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 512x512
Megapixels                      : 0.262
```

There is a gigantic comment of hex string. Deciphering, it becomes,

```
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQCyVzFWZNlKOrYK
yYFYh76omBUGmRoJtMAYtfM7G1zYQv50JgqxNCYmyd3V1GUwwB1/s8kmWTK7uao8
qN1oPqNmJLrWjKdaNeEVI4NTJYdLEzL4INMhR5Lz7ovjs2jYVvsNfdOmvIj0Tb/L
WHut+mpCCiXvXMGOPn3B+3RNxr2b2rSm98SnsQVpK+80KVAKJpLBS7+rFzNU/6fm
lTgPilxDGTWCFDdrmv1dyxtgCP5rz+6zJ8DTSLIScBWGCEauFtkQIxkqK/FJriF0
GavYhaAoMHwE5YdcfdynE10NQLYuYwxR7AiL6rr8wBAWa+QbTpnv8s3LtmPUciPH
nDgOjbAZAgMBAAECggEADz4ZUQxjz3664CtmlxvcCXIdKvATPATjPcFwYWsGaZP5
RlFZjNl7kMlkYNIrV2o2IzvpK3JIfifBY3ed29rtsY1eXDfPlmvgOeOsBeNhX8Lr
cm5wySmCR3/KHEsxYPs3MCki2b+Ht5S9GarNgFpUsmIGacHMZ8VSz413vG2J92vq
c8ZasyO9jqJYIGpUaobNaUoeojdVjoXFPhKY0b9xaylotkF+EAk3T329KANSG58A
isKwxiXw6BLGxuOfoGAh7K3r9DT5K5siE7G4uLl5tXxyNiWVAVKVK0hsPik8ka8i
5gZQhHqbTs5DBdLsrpwO/RPpKNcUKNwkHxLAwbT0AQKBgQDk+8+TGC/npiE+3UfL
3cZhPQmBEsv/9cyqoLA+0YQke3TJ4Fw8Ve/ioR2m+aaNGogo1UoX5LlTLLIMD9pO
DIPy17uuMkaKYn9fpz+YafGjupTRysjOpj+NPsBLM+UKd5evKoglEU6vCy4vF+Ms
xyAAkoT/u5qSNyrlz145i1F0AQKBgQDHYcRIOsSVBifOkNiILFfJclGjdp4mk2rH
2nK6AfyuYsz1iH4fkNkmzflSHHMPxc6FXKcVSLjsd7z1Vsk/JTK/yur6vRRfiRhA
V73C0G+8MbrfAQtRoJTzwB2qHcyjYe/wj85lzJtKdrYLJsQ+fQYbfhOyOs6qlL8j
oosRmelcGQKBgES9e1DzH95wtuZCU31V9GnYgvPmiqsqROisGHyjNQImtaas3cOX
IM5uA5LOuzr8zgdEEFwf46qebkzErYpko2/RZ5whp59/djFm6eZ9V3cJWgeo0qOG
sOlb/yoUSBveTwDcycYm7fIKbzDgAOi+VlMVqSuETCC8wvn+SJEJy8gBAoGBAKbD
OCftt4SprFvSvLlQ1XAxhOUDDo5XWMWmCbVYeh7llmZ7bno6bdZOCw3Jq9dybCUR
dNKRi9KE5/AQUazWERdnwphL06GAil8uxBIQwpQWyCVDu1LfzBYABw/FcubowIZl
0Y1F0FE871BucU+OxK/0CMdW9oqodSJ6DnV/VTbpAoGBAKwKIVzYAd97Fnc8wYfw
vOk8EJWvWJM1PU5768uEEK/o0NwTVF3UZy6bahCJdecnDUz9Ex1ql+d/wCytI8+e
3LJ95ccrdg/7t051qssGDcScLIlnnhrAd5h1nbpKSiYiaStwRjbS+Ug6brdU90mJ
LeRoM0oBEsIBRbjbSUqeKBDH
-----END PRIVATE KEY-----
```

I see! It is a private key of some sort!

If you could not guess that this is an RSA key from the title, you could run the following command to view information about this:
```
openssl pkey -in encryptor.pkey -text -noout
Private-Key: (2048 bit, 2 primes)
modulus:
    00:b2:57:31:56:64:d9:4a:3a:b6:0a:c9:81:58:87:
    be:a8:98:15:06:99:1a:09:b4:c0:18:b5:f3:3b:1b:
    5c:d8:42:fe:74:26:0a:b1:34:26:26:c9:dd:d5:d4:
    65:30:c0:1d:7f:b3:c9:26:59:32:bb:b9:aa:3c:a8:
    dd:68:3e:a3:66:24:ba:d6:8c:a7:5a:35:e1:15:23:
    83:53:25:87:4b:13:32:f8:20:d3:21:47:92:f3:ee:
    8b:e3:b3:68:d8:56:fb:0d:7d:d3:a6:bc:88:f4:4d:
    bf:cb:58:7b:ad:fa:6a:42:0a:25:ef:5c:c1:8e:3e:
    7d:c1:fb:74:4d:c6:bd:9b:da:b4:a6:f7:c4:a7:b1:
    05:69:2b:ef:34:29:50:0a:26:92:c1:4b:bf:ab:17:
    33:54:ff:a7:e6:95:38:0f:8a:5c:43:19:35:82:14:
    37:6b:9a:fd:5d:cb:1b:60:08:fe:6b:cf:ee:b3:27:
    c0:d3:48:b2:12:70:15:86:08:46:ae:16:d9:10:23:
    19:2a:2b:f1:49:ae:21:74:19:ab:d8:85:a0:28:30:
    7c:04:e5:87:5c:7d:dc:a7:13:5d:0d:40:b6:2e:63:
    0c:51:ec:08:8b:ea:ba:fc:c0:10:16:6b:e4:1b:4e:
    99:ef:f2:cd:cb:b6:63:d4:72:23:c7:9c:38:0e:8d:
    b0:19
publicExponent: 65537 (0x10001)
privateExponent:
    0f:3e:19:51:0c:63:cf:7e:ba:e0:2b:66:97:1b:dc:
    09:72:1d:2a:f0:13:3c:04:e3:3d:c1:70:61:6b:06:
    69:93:f9:46:51:59:8c:d9:7b:90:c9:64:60:d2:2b:
    57:6a:36:23:3b:e9:2b:72:48:7e:27:c1:63:77:9d:
    db:da:ed:b1:8d:5e:5c:37:cf:96:6b:e0:39:e3:ac:
    05:e3:61:5f:c2:eb:72:6e:70:c9:29:82:47:7f:ca:
    1c:4b:31:60:fb:37:30:29:22:d9:bf:87:b7:94:bd:
    19:aa:cd:80:5a:54:b2:62:06:69:c1:cc:67:c5:52:
    cf:8d:77:bc:6d:89:f7:6b:ea:73:c6:5a:b3:23:bd:
    8e:a2:58:20:6a:54:6a:86:cd:69:4a:1e:a2:37:55:
    8e:85:c5:3e:12:98:d1:bf:71:6b:29:68:b6:41:7e:
    10:09:37:4f:7d:bd:28:03:52:1b:9f:00:8a:c2:b0:
    c6:25:f0:e8:12:c6:c6:e3:9f:a0:60:21:ec:ad:eb:
    f4:34:f9:2b:9b:22:13:b1:b8:b8:b9:79:b5:7c:72:
    36:25:95:01:52:95:2b:48:6c:3e:29:3c:91:af:22:
    e6:06:50:84:7a:9b:4e:ce:43:05:d2:ec:ae:9c:0e:
    fd:13:e9:28:d7:14:28:dc:24:1f:12:c0:c1:b4:f4:
    01
prime1:
    00:e4:fb:cf:93:18:2f:e7:a6:21:3e:dd:47:cb:dd:
    c6:61:3d:09:81:12:cb:ff:f5:cc:aa:a0:b0:3e:d1:
    84:24:7b:74:c9:e0:5c:3c:55:ef:e2:a1:1d:a6:f9:
    a6:8d:1a:88:28:d5:4a:17:e4:b9:53:2c:b2:0c:0f:
    da:4e:0c:83:f2:d7:bb:ae:32:46:8a:62:7f:5f:a7:
    3f:98:69:f1:a3:ba:94:d1:ca:c8:ce:a6:3f:8d:3e:
    c0:4b:33:e5:0a:77:97:af:2a:88:25:11:4e:af:0b:
    2e:2f:17:e3:2c:c7:20:00:92:84:ff:bb:9a:92:37:
    2a:e5:cf:5e:39:8b:51:74:01
prime2:
    00:c7:61:c4:48:3a:c4:95:06:27:ce:90:d8:88:2c:
    57:c9:72:51:a3:76:9e:26:93:6a:c7:da:72:ba:01:
    fc:ae:62:cc:f5:88:7e:1f:90:d9:26:cd:f9:52:1c:
    73:0f:c5:ce:85:5c:a7:15:48:b8:ec:77:bc:f5:56:
    c9:3f:25:32:bf:ca:ea:fa:bd:14:5f:89:18:40:57:
    bd:c2:d0:6f:bc:31:ba:df:01:0b:51:a0:94:f3:c0:
    1d:aa:1d:cc:a3:61:ef:f0:8f:ce:65:cc:9b:4a:76:
    b6:0b:26:c4:3e:7d:06:1b:7e:13:b2:3a:ce:aa:94:
    bf:23:a2:8b:11:99:e9:5c:19
exponent1:
    44:bd:7b:50:f3:1f:de:70:b6:e6:42:53:7d:55:f4:
    69:d8:82:f3:e6:8a:ab:2a:44:e8:ac:18:7c:a3:35:
    02:26:b5:a6:ac:dd:c3:97:20:ce:6e:03:92:ce:bb:
    3a:fc:ce:07:44:10:5c:1f:e3:aa:9e:6e:4c:c4:ad:
    8a:64:a3:6f:d1:67:9c:21:a7:9f:7f:76:31:66:e9:
    e6:7d:57:77:09:5a:07:a8:d2:a3:86:b0:e9:5b:ff:
    2a:14:48:1b:de:4f:00:dc:c9:c6:26:ed:f2:0a:6f:
    30:e0:00:e8:be:56:53:15:a9:2b:84:4c:20:bc:c2:
    f9:fe:48:91:09:cb:c8:01
exponent2:
    00:a6:c3:38:27:ed:b7:84:a9:ac:5b:d2:bc:b9:50:
    d5:70:31:84:e5:03:0e:8e:57:58:c5:a6:09:b5:58:
    7a:1e:e5:96:66:7b:6e:7a:3a:6d:d6:4e:0b:0d:c9:
    ab:d7:72:6c:25:11:74:d2:91:8b:d2:84:e7:f0:10:
    51:ac:d6:11:17:67:c2:98:4b:d3:a1:80:8a:5f:2e:
    c4:12:10:c2:94:16:c8:25:43:bb:52:df:cc:16:00:
    07:0f:c5:72:e6:e8:c0:86:65:d1:8d:45:d0:51:3c:
    ef:50:6e:71:4f:8e:c4:af:f4:08:c7:56:f6:8a:a8:
    75:22:7a:0e:75:7f:55:36:e9
coefficient:
    00:ac:0a:21:5c:d8:01:df:7b:16:77:3c:c1:87:f0:
    bc:e9:3c:10:95:af:58:93:35:3d:4e:7b:eb:cb:84:
    10:af:e8:d0:dc:13:54:5d:d4:67:2e:9b:6a:10:89:
    75:e7:27:0d:4c:fd:13:1d:6a:97:e7:7f:c0:2c:ad:
    23:cf:9e:dc:b2:7d:e5:c7:2b:76:0f:fb:b7:4e:75:
    aa:cb:06:0d:c4:9c:2c:89:67:9e:1a:c0:77:98:75:
    9d:ba:4a:4a:26:22:69:2b:70:46:36:d2:f9:48:3a:
    6e:b7:54:f7:49:89:2d:e4:68:33:4a:01:12:c2:01:
    45:b8:db:49:4a:9e:28:10:c7
```

We are basically given everything now in order to decrypt the message.

## Solution
RSA encryption of ciphertext $M$ works by
$$
C = M^e \mod n
$$
where $C$ is the ciphertext, $n$ is the modulus, and $e$ is the public exponent.

Decryption works by
$$
M = C^d \mod n
$$
where $d$ is the private exponent.

We could use Python for decryption.

See [decryption.ipynb](./decryption.ipynb) for the solution.
