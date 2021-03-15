* `certtool(1)`, `leaks(1)`, `pluginkit(8)`

* `help`: Show all commands, or show usage for a command.

* `list-keychains`: Display or manipulate the keychain search list. 显示或设置钥匙串搜索列表

* `default-keychain`: Display or set the default keychain. 显示或设置默认的钥匙串 
* `login-keychain`: Display or set the login keychain. 显示或设置登录钥匙串
* `create-keychain`: Create keychains. 创建钥匙串并加入搜索列表
* `delete-keychain`: Delete keychains and remove them from the search list. 删除钥匙串并从搜索列表移除

* `lock-keychain`: Lock the specified keychain. 锁定指定的钥匙串
* `unlock-keychain`: Unlock the specified keychain. 解锁指定的钥匙串

* `set-keychain-settings`: Set settings for a keychain. 设置钥匙串配置
* `set-keychain-password`: Set password for a keychain. 设置钥匙串密码

* `show-keychain-info`: Show the settings for keychain.
* `dump-keychain`: Dump the contents of one or more keychains. 显示一个或多个钥匙串的内容
* `create-keypair`: Create an asymmetric key pair. 创建非对称密钥对

* `add-generic-password`: Add a generic password item. 向钥匙串中添加通用密码项
* `find-generic-password`: Find a generic password item. 查找通用密码项
* `delete-generic-password`: Delete a generic password item. 删除通用密码项
* `set-generic-password-partition-list`: Set the partition list of a generic password item.

* `add-internet-password`: Add an internet password item. 向钥匙串中添加网络密码项
* `find-internet-password`: Find an internet password item. 查找网络密码项
* `delete-internet-password`: Delete an internet password item. 删除网络密码项
* `set-internet-password-partition-list`: Set the partition list of a internet password item.

* `find-key`: Find keys in the keychain
* `set-key-partition-list`: Set the partition list of a key.

* `add-certificates`: Add certificates to a keychain. 向钥匙串种添加证书
* `find-certificate`: Find a certificate item. 查找证书
* `delete-certificate`: Delete a certificate from a keychain. 从钥匙串种删除证书

* `find-identity`: Find an identity (certificate + private key). 查找认证实体（证书+私钥）
* `delete-identity`: Delete a certificate and its private key from a keychain.
* `set-identity-preference`: Set the preferred identity to use for a service.
* `get-identity-preference`: Get the preferred identity to use for a service.

* `create-db`: Create a db using the DL.
* `install-mds`: Install (or re-install) the MDS database. 安装/重装MDS 数据库

* `export`: Export items from a keychain. 
* `import`: Import items into a keychain. 

* `cms`: Encode or decode CMS messages. 编码或解码CMS信息（PKCS#7）

* `add-trusted-cert`: Add trusted certificate(s). 添加可信证书（只包含公钥，无私钥）
* `remove-trusted-cert`: Remove trusted certificate(s). 删除可信证书
* `verify-cert`: Verify certificate(s). 验证证书

* `dump-trust-settings`: Display contents of trust settings.
* `user-trust-settings-enable`: Display or manipulate user-level trust settings.

* `trust-settings-export`: Export trust settings.
* `trust-settings-import`: Import trust settings.

* `authorize`: Perform authorization operations. 授权操作
* `authorizationdb`: Make changes to the authorization policy database. 变更授权策略数据库

* `execute-with-privileges`: Execute tool with privileges.
* `leaks`: Run `/usr/bin/leaks` on this process.

* `smartcards`: Enable, disable or list disabled smartcard tokens.
* `list-smartcards`: Display available smartcards.
* `export-smartcard`: Export/display items from a smartcard.

* `error`: Display a descriptive message for the given error code(s).

## create-keychain

```sh
Usage: create-keychain [-P] [-p password] [keychains...]
    -p  Use "password" as the password for the keychains being created
    -P  Prompt the user for a password using the SecurityAgent
Use of the -p option is insecure
        Create keychains and add them to the search list.
```

```sh
security create-keychain -p 1234 "${HOME}/Library/Keychains/acrchive.keychain-db"
```

## default-keychain

```sh
security default-keychain [-d user|system|common|dynamic] [-s [keychain]]
    -d  Use the specified preference domain
    -s  Set the default keychain to the specified keychain
With no parameters, display the default keychain.
        Display or set the default keychain.
```

## export

```sh
Usage: export [-k keychain] [-t type] [-f format] [-w] [-p] [-P passphrase] [-o outfile]
    -k  keychain to export items from
    -t  Type = certs|allKeys|pubKeys|privKeys|identities|all  (Default: all)
    -f  Format = openssl|openssh1|openssh2|bsafe|pkcs7|pkcs8|pkcs12|pemseq|x509 ...default format is pemseq for aggregate, openssl for single
    -w  Private keys are wrapped
    -p  PEM encode the output
    -P  Specify wrapping passphrase immediately (default is secure passphrase via GUI)
    -o  Specify output file (default is stdout)
```

```sh
security default-keychain
# "/Users/shanliu/Library/Keychains/login.keychain-db"
security export -k /Users/shanliu/Library/Keychains/login.keychain-db -t certs -o ~/Desktop/security_test/certs.pem
security export -k newcert.keychain -t identities -f pkcs12 -o /tmp/mycerts.p12
```

## import

```sh
Usage: import inputfile [-k keychain] [-t type] [-f format] [-w] [-P passphrase] [options...]
    -k  Target keychain to import into 指定要导入项目到哪个钥匙串中
    -t  Type = pub|priv|session|cert|agg 指定要导入的项目类型
    -f  Format = openssl|openssh1|openssh2|bsafe|raw|pkcs7|pkcs8|pkcs12|netscape|pemseq
    -w  Specify that private keys are wrapped and must be unwrapped on import 标明包装了私钥，导入时要解开
    -x  Specify that private keys are non-extractable after being imported 标明导入后，私钥无法提取私钥
    -P  Specify wrapping passphrase immediately (default is secure passphrase via GUI) 直接输入导入项目密码，默认会使用GUI输入密码
    -a  Specify name and value of extended attribute (can be used multiple times) 指定键值对属性，可以重复出现多次
    -A  Allow any application to access the imported key without warning (insecure, not recommended!) 所有程序可以使用导入的项目
    -T  Specify an application which may access the imported key (multiple -T options are allowed) 指定可以使用导入项目的程序，可以重复出现多次
Use of the -P option is insecure

        Import items into a keychain.
```

```sh
security import /tmp/certs.pem -k
security import /tmp/mycerts.p12 -t agg -k newcert.keychain
security import /tmp/mycerts.p12 -f pkcs12 -k newcert.keychain
```

## find-identity

```sh
Usage: find-identity [-p policy] [-s string] [-v] [keychain...]
    -p  Specify policy to evaluate (multiple -p options are allowed)Supported policies: basic, ssl-client, ssl-server, smime, eap,ipsec, ichat, codesigning, sys-default, sys-kerberos-kdc, macappstore, appleID
    -s  Specify optional policy-specific string (e.g. DNS hostname for SSL,or RFC822 email address for S/MIME)
    -v  Show valid identities only (default is to show all identities)
If no keychains are specified to search, the default search list is used.
        Find an identity (certificate + private key).
```

```sh
# 发现可用的证书
security find-identity -p codesigning
#Policy: Code Signing
#  Matching identities
#  1) 91CE15DE382D35B0A9AC07F07EE53BB915AE030A "iPhone Developer: Tu Hong (9BEKSZP7YX)" (CSSMERR_TP_CERT_EXPIRED)
#  2) D841BC21DFB8546A1AA9B951F3FF92D081D7C1C5 "iPhone Developer: crlandpm@163.com (J32N8XTS9Q)" (CSSMERR_TP_CERT_REVOKED)
#  3) 47A74056396EFBCA3CE9DB17F45E2AE6B116E5B9 "iPhone Developer: 刘 山 (J9HD7DBXX5)" (CSSMERR_TP_CERT_EXPIRED)
#  4) DA69E38A76604B5D17DE7B2C336F52DE50199E2E "iPhone Developer: xianghua zhou (F4EHA96CDG)" (CSSMERR_TP_CERT_EXPIRED)
#  5) E6EC078D98BF879C5C1AFBC0DC23656EF18D978C "iPhone Developer: xianghua zhou (F4EHA96CDG)" (CSSMERR_TP_CERT_EXPIRED)
```

## delete-identity

```sh
Usage: delete-identity [-c name] [-Z hash] [-t] [keychain...]
    -c  Specify certificate to delete by its common name
    -Z  Specify certificate to delete by its SHA-256 (or SHA-1) hash value
    -t  Also delete user trust settings for this identity certificate
The identity to be deleted must be uniquely specified either by a string found in its common name, or by its SHA-256 (or SHA-1) hash.
If no keychains are specified to search, the default search list is used.
        Delete an identity (certificate + private key) from a keychain.
```

* `keychain` :本地数据库DB，用于存储用户信息。
* `.certSigningRequest` :本地公钥信息。
* `.cer` :公钥信息
* `.p12` :公钥+私钥

## cms 

```
Usage: cms [-C|-D|-E|-S] [<options>]
  -C           create a CMS encrypted message
  -D           decode a CMS message decode 解码
  -E           create a CMS enveloped message
  -S           create a CMS signed message

Decoding options:
  -c content   use this detached content file
  -h level     generate email headers with info about CMS message
                 (output level >= 0)
  -n           suppress output of content

Encoding options:
  -r id,...    create envelope for these recipients,
               where id can be a certificate nickname or email address
  -G           include a signing time attribute
  -H hash      hash = MD2|MD4|MD5|SHA1|SHA256|SHA384|SHA512 (default: SHA1)
  -N nick      use certificate named "nick" for signing
  -P           include a SMIMECapabilities attribute
  -T           do not include content in CMS message
  -Y nick      include an EncryptionKeyPreference attribute with certificate
                 (use "NONE" to omit)
  -Z hash      find a certificate by subject key ID

Common options:
  -e envelope  specify envelope file (valid with -D or -E)
  -k keychain  specify keychain to use
  -i infile    use infile as source of data (default: stdin)
  -o outfile   use outfile as destination of data (default: stdout)
  -p password  use password as key db password (default: prompt). Using -p is insecure
  -s           pass data a single byte at a time to CMS
  -u certusage set type of certificate usage (default: certUsageEmailSigner)
  -v           print debugging information

Cert usage codes:
                  0 - certUsageSSLClient
                  1 - certUsageSSLServer
                  2 - certUsageSSLServerWithStepUp
                  3 - certUsageSSLCA
                  4 - certUsageEmailSigner
                  5 - certUsageEmailRecipient
                  6 - certUsageObjectSigner
                  7 - certUsageUserCertImport
                  8 - certUsageVerifyCA
                  9 - certUsageProtectedObjectSigner
                 10 - certUsageStatusResponder
                 11 - certUsageAnyCA
        Encode or decode CMS messages.
```

```sh
# mobileprovision 解密
security  cms -D -i match_AppStore_comfunmore.mobileprovision > a.xml
```

## unlock-keychain

```sh
Usage: unlock-keychain [-u] [-p password] [keychain]
    -p  Use "password" as the password to unlock the keychain
    -u  Do not use the password
Use of the -p option is insecure
        Unlock the specified keychain.
```

```sh
security list-keychains
# "/Users/shanliu/Library/Keychains/login.keychain-db"
# "/Library/Keychains/System.keychain"
echo $HOME
# /Users/shanliu
security unlock-keychain -p 1234 ${HOME}/Library/Keychains/login.keychain-db
```

security unlock-keychain -p 123 ${HOME}/Library/Keychains/login.keychain-db

```sh
security unlock-keychain -p $MAC_PASS_WORD "${HOME}/Library/Keychains/login.keychain-db" 2>/dev/null
if [[ $? -ne 0 ]]; then
	echo "Not Equal"
else
	echo "Equal"
fi
```

## set-keychain-settings

```
Usage: set-keychain-settings [-lu] [-t timeout] [keychain]
    -l  Lock keychain when the system sleeps
    -u  Lock keychain after timeout interval
    -t  Timeout in seconds (omitting this option specifies "no timeout")

        Set settings for a keychain.
```

## set-keychain-password

```
Usage: set-keychain-password [-o oldPassword] [-p newPassword] [keychain]
    -o  Old keychain password (if not provided, will prompt)
    -p  New keychain password (if not provided, will prompt)
Use of either the -o or -p options is insecure
```

## show-keychain-info

## dump-keychain

```sh
Usage: dump-keychain [-adir] [keychain...]
    -a  Dump access control list of items
    -d  Dump (decrypted) data of items
    -i  Interactive access control list editing mode
    -r  Dump the raw (encrypted) data of items
        Dump the contents of one or more keychains.
```

## create-keypair

> 创建非对称密钥

```sh
Usage: create-keypair [-a alg] [-s size] [-f date] [-t date] [-d days] [-k keychain] [-A|-T appPath] description
    -a  Use alg as the algorithm, can be rsa, dh, dsa or fee (default rsa)
    -s  Specify the keysize in bits (default 512)
    -f  Make a key valid from the specified date
    -t  Make a key valid to the specified date
    -d  Make a key valid for the number of days specified from today
    -k  Use the specified keychain rather than the default
    -A  Allow any application to access this key without warning (insecure, not recommended!)
    -T  Specify an application which may access this key (multiple -T options are allowed)
If no options are provided, ask the user interactively.
        Create an asymmetric key pair.
```


## add-generic-password

```sh
Usage: add-generic-password [-a account] [-s service] [-w password] [options...] [-A|-T appPath] [keychain]
    -a  Specify account name (required)
    -c  Specify item creator (optional four-character code)
    -C  Specify item type (optional four-character code)
    -D  Specify kind (default is "application password")
    -G  Specify generic attribute (optional)
    -j  Specify comment string (optional)
    -l  Specify label (if omitted, service name is used as default label)
    -s  Specify service name (required)
    -p  Specify password to be added (legacy option, equivalent to -w)
    -w  Specify password to be added
    -X  Specify password data to be added as a hexadecimal string
    -A  Allow any application to access this item without warning (insecure, not recommended!)
    -T  Specify an application which may access this item (multiple -T options are allowed)
    -U  Update item if it already exists (if omitted, the item cannot already exist)

By default, the application which creates an item is trusted to access its data without warning.
You can remove this default access by explicitly specifying an empty app pathname: -T ""
If no keychain is specified, the password is added to the default keychain.
Use of the -p or -w options is insecure. Specify -w as the last option to be prompted.

        Add a generic password item.    
```
## add-internet-password

```sh
Usage: add-internet-password [-a account] [-s server] [-w password] [options...] [-A|-T appPath] [keychain]
    -a  Specify account name (required)
    -c  Specify item creator (optional four-character code)
    -C  Specify item type (optional four-character code)
    -d  Specify security domain string (optional)
    -D  Specify kind (default is "Internet password")
    -j  Specify comment string (optional)
    -l  Specify label (if omitted, server name is used as default label)
    -p  Specify path string (optional)
    -P  Specify port number (optional)
    -r  Specify protocol (optional four-character SecProtocolType, e.g. "http", "ftp ")
    -s  Specify server name (required)
    -t  Specify authentication type (as a four-character SecAuthenticationType, default is "dflt")
    -w  Specify password to be added
    -X  Specify password data to be added as a hexadecimal string
    -A  Allow any application to access this item without warning (insecure, not recommended!)
    -T  Specify an application which may access this item (multiple -T options are allowed)
    -U  Update item if it already exists (if omitted, the item cannot already exist)

By default, the application which creates an item is trusted to access its data without warning.
You can remove this default access by explicitly specifying an empty app pathname: -T ""
If no keychain is specified, the password is added to the default keychain.
Use of the -p or -w options is insecure. Specify -w as the last option to be prompted.

        Add an internet password item.
```
## add-certificates 


## find-generic-password

```sh
Usage: find-generic-password [-a account] [-s service] [options...] [-g] [keychain...]
    -a  Match "account" string
    -c  Match "creator" (four-character code)
    -C  Match "type" (four-character code)
    -D  Match "kind" string
    -G  Match "value" string (generic attribute)
    -j  Match "comment" string
    -l  Match "label" string
    -s  Match "service" string
    -g  Display the password for the item found
    -w  Display only the password on stdout
If no keychains are specified to search, the default search list is used.
        Find a generic password item.
```

## delete-generic-password

```sh
Usage: delete-generic-password [-a account] [-s service] [options...] [keychain...]
    -a  Match "account" string
    -c  Match "creator" (four-character code)
    -C  Match "type" (four-character code)
    -D  Match "kind" string
    -G  Match "value" string (generic attribute)
    -j  Match "comment" string
    -l  Match "label" string
    -s  Match "service" string
If no keychains are specified to search, the default search list is used.
        Delete a generic password item.
```

## set-generic-password-partition-list

```sh
Usage: set-generic-password-partition-list [-a account] [-s service] [-S partition-list] [-k keychain password] [options...] [keychain]
    -a  Match "account" string
    -c  Match "creator" (four-character code)
    -C  Match "type" (four-character code)
    -D  Match "kind" string
    -G  Match "value" string (generic attribute)
    -j  Match "comment" string
    -l  Match "label" string
    -s  Match "service" string
    -S  Comma-separated list of allowed partition IDs
    -k  The password for the keychain (required)
If no keychains are specified to search, the default search list is used.
Use of the -k option is insecure. Omit it to be prompted.

        Set the partition list of a generic password item.
```

## find-internet-password
## delete-internet-password
## set-internet-password-partition-list

## find-key

```sh
find-key: illegal option -- h
Usage: find-key [options...] [keychain...]
    -a  Match "application label" string
    -c  Match "creator" (four-character code)
    -d  Match keys that can decrypt
    -D  Match "description" string
    -e  Match keys that can encrypt
    -j  Match "comment" string
    -l  Match "label" string
    -r  Match keys that can derive
    -s  Match keys that can sign
    -t  Type of key to find: one of "symmetric", "public", or "private"
    -u  Match keys that can unwrap
    -v  Match keys that can verify
    -w  Match keys that can wrap
If no keychains are specified to search, the default search list is used.
        Find keys in the keychain
```

## find-certificate

```sh
Usage: find-certificate [-a] [-c name] [-e emailAddress] [-m] [-p] [-Z] [keychain...]
    -a  Find all matching certificates, not just the first one
    -c  Match on "name" when searching (optional)
    -e  Match on "emailAddress" when searching (optional)
    -m  Show the email addresses in the certificate
    -p  Output certificate in pem format # pem文件是服务器向苹果服务器做推送时候需要的文件，主要是给php向苹果服务器验证时使用，下面介绍一下pem文件的生成。
    -Z  Print SHA-256 (and SHA-1) hash of the certificate
If no keychains are specified to search, the default search list is used.
        Find a certificate item.
```

```sh
# Exports all certificates from all keychains into a pem file called allcerts.pem.
security find-certificate -a -p > ~/Desktop/allcerts.pem

# Exports all certificates from all keychains with the email address me@foo.com into a pem file called certs.pem.
security find-certificate -a -e me@foo.com -p > certs.pem

# Print the SHA-256 hash of every certificate in 'login.keychain' whose common name includes 'MyName'
# security find-certificate -a -c MyName -Z login.keychain | grep ^SHA-256
```

## find-identity

```sh
Usage: find-identity [-p policy] [-s string] [-v] [keychain...]
    -p  Specify policy to evaluate (multiple -p options are allowed) Supported policies: basic, ssl-client, ssl-server, smime, eap,ipsec, ichat, codesigning, sys-default, sys-kerberos-kdc, macappstore, appleID
    -s  Specify optional policy-specific string (e.g. DNS hostname for SSL,or RFC822 email address for S/MIME)
    -v  Show valid identities only (default is to show all identities)
If no keychains are specified to search, the default search list is used.
        Find an identity (certificate + private key).
```

```sh
security find-identity

# Policy: X.509 Basic
#   Matching identities
#   1) 91CE15DE382D35B0A9AC07F07EE53BB915AE030A "iPhone Developer: Tu Hong (9BEKSZP7YX)" (CSSMERR_TP_CERT_EXPIRED)
#   2) 31C403B6288E8E541271BBC35437AA78D153D3B4 "Apple Development IOS Push Services: com.cmhk.uhome" (CSSMERR_TP_CERT_EXPIRED)
#   3) 78BE7058DBB19DC1BB8F531346DC2A5A41E9D57E "Apple Push Services: com.cmhk.uhome" (CSSMERR_TP_CERT_EXPIRED)
#   4) 5BE46B56EAD05E4A16F02345945F3624AD0203F2 "Xcode Server Builder (2018/10/16 下午2:01:39)" (CSSMERR_TP_NOT_TRUSTED)
#   5) D841BC21DFB8546A1AA9B951F3FF92D081D7C1C5 "iPhone Developer: crlandpm@163.com (J32N8XTS9Q)" (CSSMERR_TP_CERT_REVOKED)
#   6) 47A74056396EFBCA3CE9DB17F45E2AE6B116E5B9 "iPhone Developer: 刘 山 (J9HD7DBXX5)" (CSSMERR_TP_CERT_EXPIRED)
#   7) 9112C82C020733BDC5A6E46FAA81521CF8914446 "ExpressVPN Client" (CSSMERR_TP_NOT_TRUSTED)
#   8) DA69E38A76604B5D17DE7B2C336F52DE50199E2E "iPhone Developer: xianghua zhou (F4EHA96CDG)" (CSSMERR_TP_CERT_EXPIRED)
#   9) E6EC078D98BF879C5C1AFBC0DC23656EF18D978C "iPhone Developer: xianghua zhou (F4EHA96CDG)" (CSSMERR_TP_CERT_EXPIRED)
#  10) 7AED41190FC02B0E8580D15428F3BA14CBD8F5FD "iPhone Distribution: China Merchants Property Management Co., Ltd (J5K2TT55WE)" (CSSMERR_TP_CERT_EXPIRED)
#  11) 429AB419FA3BCAE572951D3AAB8EC9A82065FAF2 "iPhone Developer: djwxxy@cmhk.com (9K662RER7L)" (CSSMERR_TP_CERT_EXPIRED)
#  12) A40DF4849FDE33449555F03A7229DD66D8790D6E "iPhone Developer: JiaSheng Ye (4MWHJ6ZN99)" (CSSMERR_TP_CERT_EXPIRED)
#  13) 5C98EC68C37BC821758E26BD965C90584D7B4829 "iPhone Developer: Mengjun Zhang (8DWMXQZF8T)" (CSSMERR_TP_CERT_EXPIRED)
#  14) 543C62CC846688CD2A0CEF727C243764B64B6E47 "Apple Development IOS Push Services: com.sany.ydcustomer" (CSSMERR_TP_CERT_EXPIRED)
#  15) A467A84157F8E469AD5EC07E5695D3373373D247 "Apple Push Services: com.sany.ydcustomer" (CSSMERR_TP_CERT_EXPIRED)
```

```sh
# Display valid identities that can be used for SSL client authentication
security find-identity -v -p ssl-client

# Display identities for a SSL server running on the host 'www.domain.com'
security find-identity -p ssl-server -s www.domain.com

# Display identities that can be used to sign a message from 'user@domain.com'
security find-identity -p smime -s user@domain.com
```

## add-trusted-cert

```sh
Usage: add-trusted-cert  [<options>] [certFile]
    -d                  Add to admin cert store; default is user
    -r resultType       resultType = trustRoot|trustAsRoot|deny|unspecified;default is trustRoot
    -p policy           Specify policy constraint (ssl, smime, codeSign, IPSec, iChat,basic, swUpdate, pkgSign, pkinitClient, pkinitServer, eap)
    -a appPath          Specify application constraint
    -s policyString     Specify policy-specific string
    -e allowedError     Specify allowed error (certExpired, hostnameMismatch) or integer
    -u keyUsage         Specify key usage, an integer
    -k keychain         Specify keychain to which cert is added
    -i settingsFileIn   Input trust settings file; default is user domain
    -o settingsFileOut  Output trust settings file; default is user domain
    certFile            Certificate(s)
        Add trusted certificate(s).
```

```sh
security add-trusted-cert /tmp/cert.der
security add-trusted-cert -d .tmp/cert.der
security add-trusted-cert -d -r trustAsRoot -k /Library/Keychains/System.keychain

security add-trusted-cert -d -r trustAsRoot -p 123 -k "/Library/Keychains/System.keychain" "/private/tmp/certs/certname.cer"
security add-trusted-cert -d -r trustRoot -k "/Library/Keychains/System.keychain" "/private/tmp/certs/certname.cer" srm "/private/tmp/certs/certname.cer"
```

## remove-trusted-cert

```sh
```

## create-db

```sh
Usage: create-db [-ao0] [-g dl|cspdl] [-m mode] [name]
    -a  Turn off autocommit
    -g  Attach to "guid" rather than the AppleFileDL
    -m  Set the inital mode of the created db to "mode"
    -o  Force using openparams argument
    -0  Force using version 0 openparams
If no name is provided, ask the user interactively.
        Create a db using the DL.
```

```sh
security create-db -m 0644 test.db
security create-db -g cspdl -a test2.db
```

## verify-cert

```sh
Usage: verify-cert [<options>] [<url>]
    -c certFile         Certificate to verify. Can be specified multiple times, leaf first.
    -r rootCertFile     Root Certificate. Can be specified multiple times.
    -p policy           Verify Policy (basic, ssl, smime, codeSign, IPSec, swUpdate, pkgSign,eap, appleID, macappstore, timestamping); default is basic.
    -C                  Set client policy to true. Default is server policy. (ssl, IPSec, eap)
    -d date             Set date and time to use when verifying certificate,provided in the form of YYYY-MM-DD-hh:mm:ss (time optional) in GMT.
                        e.g: 2016-04-25-15:59:59 for April 25, 2016 at 3:59:59 pm in GMT
    -k keychain         Keychain. Can be specified multiple times. Default is default search list.
    -n name             Name to be verified. (ssl, IPSec, smime)
    -N                  No keychain search list. (For backward compatibility, -n without a subsequent name argument is interpreted as equivalent to -N.)
    -L                  Local certificates only (do not try to fetch missing CA certs from net).
    -l                  Leaf cert is a CA (normally an error, unless this option is given).
    -e emailAddress     Email address for smime policy. (Deprecated; use -n instead.)
    -s sslHost          SSL host name for ssl policy. (Deprecated; use -n instead.)
    -q                  Quiet.
    -R revCheckOption   Perform revocation checking with one of the following options:
                            ocsp     Check revocation status using OCSP method.
                            crl      Check revocation status using CRL method.
                            require  Require a positive response for successful verification.
                            offline  Consult cached responses only (no network requests).
                        Can be specified multiple times; e.g. to enable revocation checking
                        via either OCSP or CRL methods and require a positive response, use
                        "-R ocsp -R crl -R require".
    -P                  Output the constructed certificate chain in PEM format.
    -t                  Output certificate contents as text.
    -v                  Specify verbose output, including per-certificate trust results.
Note: if a direct URL argument is provided, standard SSL server evaluation policy is used
and other certificates or policy options will be ignored.

        Verify certificate(s).
```

```sh
security verify-cert -c applestore0.cer -c applestore1.cer -p ssl -n store.apple.com

security verify-cert -r serverbasic.crt
# ...certificate verification successful.

security verify-cert -v https://www.apple.com
# connection to www.apple.com port 443 (tcp) opened
# connection to www.apple.com port 443 (tcp) closed
# ...certificate verification successful.
# ---
# Certificate chain
#  0: www.apple.com
#     <cert(0x7faa8400e600) s: www.apple.com i: DigiCert SHA2 Extended Validation Server CA-3>
#  1: DigiCert SHA2 Extended Validation Server CA-3
#     <cert(0x7faa8380e600) s: DigiCert SHA2 Extended Validation Server CA-3 i: DigiCert High Assurance EV Root CA>
#  2: DigiCert High Assurance EV Root CA
#     <cert(0x7faa8380ee00) s: DigiCert High Assurance EV Root CA i: DigiCert High Assurance EV Root CA>
# ---
# Extended Validation (EV) confirmed for "Apple Inc."
# Certificate Transparency (CT) status: verified
# ---
# Trust evaluation results
# {
#     Organization = "Apple Inc.";
#     TrustCertificateTransparency = 1;
#     TrustEvaluationDate = "2020-11-18 07:07:56 +0000";
#     TrustExpirationDate = "2020-11-19 21:14:55 +0000";
#     TrustExtendedValidation = 1;
#     TrustResultDetails =     (
#                 {
#         },
#                 {
#         },
#                 {
#         }
#     );
#     TrustResultValue = 4;
#     TrustRevocationChecked = 1;
# }
```

## authorize

```sh
Usage: authorize [<options>] <right(s)...>
  -u        Allow user interaction.
  -c        Use login name and prompt for password.
  -C login  Use given login name and prompt for password.
  -x        Do NOT share -c/-C explicit credentials
  -p        Allow returning partial rights.
  -d        Destroy acquired rights.
  -P        Pre-authorize rights only.
  -l        Operate authorizations in least privileged mode.
  -i        Internalize authref passed on stdin.
  -e        Externalize authref to stdout.
  -w        Wait until stdout is closed (to allow reading authref from pipe).
Extend rights flag is passed per default.
        Perform authorization operations.
```

```sh
# Basic authorization of my-right.
security authorize -ud my-right

# Authorizing a right and passing it to another command as a way to add authorization to shell scripts.
security -q authorize -uew my-right | security -q authorize -i my-right
```

## list-keychains

```sh
Usage: list-keychains [-d user|system|common|dynamic] [-s [keychain...]]
    -d  Use the specified preference domain
    -s  Set the search list to the specified keychains
With no parameters, display the search list.
        Display or manipulate the keychain search list.
```

```sh
"/Users/shanliu/Library/Keychains/login.keychain-db"
"/Library/Keychains/System.keychain"
```

# 文件

* `~/Library/Preferences/com.apple.security.plist` Property list file containing the current user's default keychain and keychain search list.
* `/Library/Preferences/com.apple.security.plist`






























