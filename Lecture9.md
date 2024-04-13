# Security & Cryptography

## Entropy（熵)

熵是对随机性的一种度量，以bit为单位

$log_2(\#possibility)$

coin flip：$log_22=1bit$

dice flip：$log_26=2.6bit$

## Hash Function

将人任意长度的输入数据压缩为固定长度的输出

例如：SHA-1 接收一定量的bytes，输出160bits

```shell
printf 'hello' | sha1sum
aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d  -
```

哈希函数有许多重要属性

- non-invertible（不可逆）

- collision-resistant（抗碰撞性）

  __很难找到两个不同的输入产生相同的输出__

## Particular Construction（承诺机制）

承诺是指发送方向接收方保证某个信息的真实性，但不立即揭示该信息。在之后的某个时间点，发送方揭示原始信息，并允许接收方验证该信息是否与之前的承诺相匹配。

## Key Derivation Fuctions（KDFs / 密钥生成函数）

- 计算速度较慢

  - Why？

    一方面是在实际使用进行密码验证时，只需做一次检查，所以慢一些也无所谓。另一方面，当有人尝试暴力破解密码时，通过是这个函数变慢，可以大大减缓攻击者的破解速度

例如：

__PBKDF2（Password-Based Key Derivation Function 2）__

## Symmetic Key cryptography（对称加密）

- __Key generation function（密钥生成函数）__是一个随机生成函数，生成__key__（密钥）
- __Encrypt function（加密函数）__需要__plaintext__（明文）作为输入，同时需要输入一个密钥，产生一个__ciphertext__（密文）
- __Decrypt function（解密函数）__ 是与加密函数相反的函数，输入密文和密钥，输出明文

### 属性

1. 只有拥有密钥的情况下，才能通过密文找出明文
2. 如果使用密钥加密，然后使用相同的密钥解密，将得到相同的信息

但是可能会出现密钥丢失的情况，为了避免这种情况，可以：

- 使用一个口令，并将其传递给密钥生成函数从而得到密钥，再进行加密解密操作

### Openssl

```shell
openssl aes-256-cbc -slat -in foo.bar -out foo.enc.bar
enter aes-256-cbc encryption password:
Verifying - enter aes-256-cbc encryption password:
#加密
openssl aes-256-cbc -d -in foo.enc.bar -out foo.dec.bar
enter aes-256-cbc decryption password:
#解密
```

### Cryptographic Salt（加密盐）

为了针对黑客建立的彩虹表，在数据库不仅要存储密码的哈希值，还要计算一个__盐值__（salt value）。盐值是一个长的随机字符串，对于每个用户来说是随机唯一的。所以如果一个人在两个网站上使用相同的密码且两个网站使用了相同的哈希函数，那么存储的哈希值在两个服务器上是相同的，但是由于添加了随机的盐值，使得哥哥数据库里存储的密码看起来也不同，能够有效无效化彩虹表

## Asymmetric Key Cryptography（非对称加密）

- __Key generation function__生成随机密钥，但不同于对称加密，需要生成一对不同的密钥，其中一个称为__public key__（公钥），另一个称为__private key__（私钥）
- __Encrypt function__接受明文和公钥，然后输出密文
- __Decrypt function__接受密文和私钥，还原出明文

### Signing and Verifying（签名与验证）

__sign__函数接收消息与私钥生成签名，另外一个函数__verify__接收消息和签名以及公钥，返回一个布尔值告诉签名是否正确。

如果没有私钥，很难为任何信息产生一个签名，就无法将信息和公钥一起提供给verify函数

- Hard to forge（难以伪造）

例如：__RSA__

常用于电子邮件，私人通信，软件分发

### Key Distribution

如何知道你找到的公钥实际上是我的公钥？

1. 线下交流（显然不太优雅）
2. PGP
3. keybase

## Hybrid Encryption

结合对称加密和非对称加密

