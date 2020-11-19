
* `/usr/bin/codesign`
* csreq(1), xcodebuild(1), codesign_allocate(1)

> codesign 创建和操作 代码签名。

```
codesign -s identity [-i identifier] [-r requirements] [-fv] [path ...]
codesign -v [-R requirement] [-v] [path|pid ...]
codesign -d [-v] [path|pid ...]
codesign -h [-v] [pid ...]
```

## -s

```sh
# 发现可用的证书
security find-identity -p codesigning
# sign the app
# codesign -s "Identifier Name"
codesign -s "Apple Development: Xiang  peng Lan (3H7K5HCB5N)" ~/Desktop/test.app
```

## -v

<img src="/assets/images/automatic/01.png"/>

```sh
# Digital Signature 数字签名
codesign -v --verbose=5 test.ipa 
# test.ipa: valid on disk
# test.ipa: satisfies its Designated Requirement

# 代码重签
unzip -q test_old.ipa
rm -rf Payload/test_old.app/_CodeSignature
cd Payload
cd new_provision_prifle.mobileprovision Payload/test_old.app/embedded.mobileprovision
codesign -d --entitlements - test_old.app > entitlements.plist
codesign -f -s "Apple Development: Xiang  peng Lan (3H7K5HCB5N)" '--entitlements' 'entitlements.plist' test_old.app
```

```
codesign 想要访问您的钥匙串中的密钥
```

* `--all-architectures`
* `-s, --sign identity`


```sh
security import /tmp/MyCertificates.p12 -k $HOME /Library/Keychains/login.keychain -P MyPassword -T /usr/bin/codesign
```

CSR(Certificate Signing Request):证书注册请求

```sh
# kn007.net.csr
openssl req -nodes -newkey rsa:2048 -keyout CertificateSigningRequest.key -out CertificateSigningRequest.certSigningRequest
# /C 国家
# /ST 省份
# /L 城市
# /O 名称
# /OU 类别
# /CN 域名
# /emailAddress 邮箱
openssl req -new -newkey rsa:2048 -nodes -out CertificateSigningRequest.csr -keyout CertificateSigningRequest.key -subj "/C=CN/ST=GD/L=DG/O=kn007's blog/OU=BLOG/CN=kn007.net/emailAddress=kn007@126.com"
```

```sh
# What's in CertificateSigningRequest.certSigningRequest
shanliu@shanliudeMac-mini author_files % ls
CertificateSigningRequest.certSigningRequest	development_p.cer				distribution_p.cer
development.cer					distribution.cer
shanliu@shanliudeMac-mini author_files % openssl asn1parse -i -in CertificateSigningRequest.certSigningRequest
    0:d=0  hl=4 l= 645 cons: SEQUENCE          
    4:d=1  hl=4 l= 365 cons:  SEQUENCE          
    8:d=2  hl=2 l=   1 prim:   INTEGER           :00
   11:d=2  hl=2 l=  64 cons:   SEQUENCE          
   13:d=3  hl=2 l=  31 cons:    SET               
   15:d=4  hl=2 l=  29 cons:     SEQUENCE          
   17:d=5  hl=2 l=   9 prim:      OBJECT            :emailAddress
   28:d=5  hl=2 l=  16 prim:      IA5STRING         :870941563@qq.com
   46:d=3  hl=2 l=  16 cons:    SET               
   48:d=4  hl=2 l=  14 cons:     SEQUENCE          
   50:d=5  hl=2 l=   3 prim:      OBJECT            :commonName
   55:d=5  hl=2 l=   7 prim:      UTF8STRING        :shanliu
   64:d=3  hl=2 l=  11 cons:    SET               
   66:d=4  hl=2 l=   9 cons:     SEQUENCE          
   68:d=5  hl=2 l=   3 prim:      OBJECT            :countryName
   73:d=5  hl=2 l=   2 prim:      PRINTABLESTRING   :CN
   77:d=2  hl=4 l= 290 cons:   SEQUENCE          
   81:d=3  hl=2 l=  13 cons:    SEQUENCE          
   83:d=4  hl=2 l=   9 prim:     OBJECT            :rsaEncryption
   94:d=4  hl=2 l=   0 prim:     NULL              
   96:d=3  hl=4 l= 271 prim:    BIT STRING        
  371:d=2  hl=2 l=   0 cons:   cont [ 0 ]        
  373:d=1  hl=2 l=  13 cons:  SEQUENCE          
  375:d=2  hl=2 l=   9 prim:   OBJECT            :sha256WithRSAEncryption
  386:d=2  hl=2 l=   0 prim:   NULL              
  388:d=1  hl=4 l= 257 prim:  BIT STRING 

% openssl req -text -noout -in CertificateSigningRequest.certSigningRequest 
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: emailAddress=870941563@qq.com, CN=shanliu, C=CN
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:cb:7a:b7:8d:7b:dd:da:9e:e5:5d:3e:7a:76:d1:
                    39:a1:22:25:ce:95:f1:66:02:eb:c9:96:79:35:fd:
                    b6:8b:8c:6f:6d:19:2a:29:df:3a:de:20:e6:68:0b:
                    e8:ff:67:27:ed:fc:8c:1c:ca:f7:06:2b:ba:40:01:
                    bd:f4:82:25:99:31:83:06:3f:21:f9:e8:70:67:76:
                    e4:d1:54:10:ec:70:09:ad:7a:25:25:0a:76:27:b7:
                    fb:8f:ca:d7:7b:69:bd:7e:5c:a1:0d:4e:28:92:3f:
                    a5:80:34:9e:9f:59:c6:0c:4c:6d:97:61:63:38:85:
                    b9:92:79:78:10:73:86:9a:df:34:01:3a:01:3d:00:
                    79:08:3c:8a:63:bb:89:1f:8b:9b:79:6c:10:bf:ae:
                    fe:eb:36:37:17:d7:0c:97:ab:3f:1a:95:60:2b:39:
                    0c:74:36:e5:f9:9f:34:42:36:b1:5a:2e:9e:27:df:
                    ac:b6:34:ad:7c:93:de:33:c7:03:f7:f6:46:bc:70:
                    0e:bd:7a:bc:cf:df:7e:b7:15:1b:1d:ad:19:bc:3b:
                    0d:1a:5f:9a:91:3f:78:4a:c0:c7:20:72:5c:0a:1a:
                    8a:c5:c5:86:dc:38:f4:9b:77:62:3e:a4:d9:23:de:
                    74:3d:4e:ac:c9:1d:fb:20:0b:a1:a2:04:4b:e1:6f:
                    83:a7
                Exponent: 65537 (0x10001)
        Attributes:
            a0:00
    Signature Algorithm: sha256WithRSAEncryption
         b0:ce:71:67:7e:78:ce:be:6c:fa:8b:0a:cd:75:7b:53:0e:6b:
         28:fc:0a:5a:36:da:28:17:27:44:02:1e:d1:7f:4a:a3:97:d6:
         50:f6:18:a7:76:1f:bd:18:34:b2:17:2f:ca:df:16:86:45:92:
         7a:62:03:43:58:40:d4:59:0a:59:c4:ea:2e:2b:9a:e4:ea:86:
         cb:ee:7a:00:d5:f6:14:00:6c:4b:c3:c2:df:45:f3:b7:6b:e1:
         7d:a8:91:d5:32:aa:b4:30:d7:58:1c:9a:08:d6:9a:be:7c:a0:
         9a:55:41:e9:8a:95:ec:aa:20:3a:e9:0a:5f:c1:74:f0:c2:29:
         74:d2:a7:80:50:ec:7e:34:83:89:b5:94:23:16:69:52:93:17:
         94:16:d5:9d:b9:d4:9f:25:75:c2:29:a1:e5:b9:ef:25:22:aa:
         45:67:4e:06:40:2c:57:88:37:16:13:4a:a5:69:25:2e:e2:62:
         c3:71:01:8a:07:65:10:6b:5e:4a:de:11:33:a6:ba:bc:ac:cc:
         58:cb:7f:c1:48:9c:d5:65:6b:d2:46:fb:f9:12:d3:27:5c:b2:
         f7:0d:ab:02:67:47:d8:fc:23:ae:17:c6:6f:36:d6:ec:3c:31:
         76:3a:f3:2e:82:88:f1:25:e5:b4:82:8c:dc:b3:04:d7:4d:90:
         54:72:41:a1  
```

* [iOS builds / ipa creation no longer works from the command line](https://stackoverflow.com/questions/32763288/ios-builds-ipa-creation-no-longer-works-from-the-command-line)
* [生成Certificate Signing Request(CSR)](https://kn007.net/topics/generate-a-certificate-signing-request-csr/)
* [YouTube-Code signing internals that nobody tells you - iOS Dev Scout](https://www.youtube.com/watch?v=1FG_VvF2Xg0)
* [Apple-Support-Certificates](https://developer.apple.com/support/certificates)
* [GitHub-IPABuildShell](https://github.com/fenglh/IPABuildShell)
* [Apple-What's New in Xcode App Signing](https://developer.apple.com/videos/play/wwdc2016/401/)
* [Install a Certificate via the Command Line.](http://skabber.com/install-a-certificate-via-the-command-line/)

