## WriteUp

本次解谜红包共有3个 Puzzle、1个彩蛋、1个隐藏问题。由于难度较为平和，故此 WriteUp 将侧重于 Puzzle 实现方法等方面的叙述。本次 Puzzle 结束后不会立刻删除，会保留一段时间供有兴趣的参与者验证。

### Puzzles

原本 Puzzle 是设计成串行结构，但是考虑到参与者的实际体验，故改为了并行，同时取消了各个 Puzzle 之前的联系。

#### Puzzle 1

tip的设置原因是 Flag 是手写的，担心比较抽象，不易辨认出。

打开 Puzzle后，发现并不是如描述中的 “a beautiful song” ，但是确实音频存在一定规律，怀疑是经过了某种处理的音频。

首先考虑的处理方法是倒放，可以通过使用“ Adobe Audition的 效果-反向 ”处理还原。

<img src="https://s2.loli.net/2023/01/18/5EyAzFZD2kQ7NKf.png" alt="image-20230117185752762" style="zoom:33%;" />

还原后确实是一首正常的音乐，但是在 00:56 左右出现了一阵不明意义的噪音，结合 “appreciate with ur eyes”考虑

很大可能是通过某种方式将音乐图像化。检查 频谱音调显示器 和 频谱频率显示器 发现在 频谱频率显示器 出现了一个英文单词。

<img src="https://s2.loli.net/2023/01/18/aAIbxpecHR3BCQy.png" alt="image-20230117190155259" style="zoom: 33%;" /><img src="https://s2.loli.net/2023/01/18/SieuN2EL5yFGmP4.png" alt="image-20230117190209352" style="zoom: 33%;" />

将 summer 输入到 MD5 函数中，取其32位小写哈希值的前8位输入到支付宝口令红包中即可。

##### 实现

Adobe Audition确实是一款很强大的音频处理工具，但是它并不能实现频谱编辑。

Puzzle 1 对频谱的编辑是使用 SpectraLayers 的 Frenquency Pencil 实现d的。

![image-20230117190914932](https://s2.loli.net/2023/01/18/rznfejwPm5Y6yIk.png)

#### Puzzle 2

Puzzle 2 唯一的文本描述是 a manifesto 意为 一份宣言。

打开 02.docx 后，发现这是一篇全英文的宣言，也就是 1993 年的 “密码朋克宣言”。

如果你是一个擅长英语阅读的人，读完全篇你会发现这和目前广为流传的版本一模一样，文中设置的超链接也和 Puzzle 没有直接联系。

但是，在复制的过程中发现，部分段落在复制到其它地方去的时候(如：Google translation )，末尾会莫名奇妙多出几个字母。

此时检查发现，段末有一个字号非常小的字母![image-20230117191743682](https://s2.loli.net/2023/01/18/xVqzRKmQYwhDjlG.png)

这就是复制过程中 ，“凭空”出现字母的来源。

将所有的字母连起来后，得到了一句话： Check The Hex

Hex 一词本意为 16进制。这里使用 WinHex 来以 16 进制的方式打开这个文档。

<img src="https://s2.loli.net/2023/01/18/PDgxhtIu9SNeQcT.png" alt="image-20230117192155575" style="zoom:50%;" />

很显然 Flag 白给了。

##### 实现

WinHex 除了可以以十六进制查看文件以外，同样可以进行编辑。

可以通过修改不影响文件正常读取的段来进行隐写。

#### Puzzle 3

Puzzle 3 大概是本次解谜红包中 ~~最烂~~ 脑洞最大的 Puzzle 了。

打开 Puzzle 3 后，发现跳转到了一张图片。

<img src="https://s2.loli.net/2023/01/18/gd5aB4cmGf1pue9.png" alt="image-20230117192915271" style="zoom:50%;" />

在解谜红包的主页，提到了解谜红包是通过 Github Pages 托管的，并给出了 Github 的链接。

打开链接发现，仓库里并没有 nwafu-lab.txt 这个文件。

<img src="https://s2.loli.net/2023/01/18/C932M1YmXj5LeW4.png" alt="image-20230117193032210" style="zoom:50%;" />

将 check_txt.jpg 下载下来后，发现并没有什么异常。

但是可以肯定这里面一定隐藏了什么东西，此时可以通过各种脚本和检测工具对图片进行检查。

结果一定是相同了，这个图片使用了 “图种” 的方法，附加了一个压缩包。

通过将 .jpg 的后缀名修改为 .zip 后，压缩包可以顺利被打开了，但是需要一个密码。

<img src="https://s2.loli.net/2023/01/18/t2qPXahv9pRC67I.png" alt="image-20230117193548898" style="zoom:50%;" />

此时图片上的那个txt与一个叫做 pri.key 的文件出现在了加密的压缩包中。

此时就来到了本体最 ~~傻逼~~ 迷惑的地方，密码究竟在哪里？

题目中给的提示是 Where is txt? 与 What is txt?

首先可以排除这个文本文档，因为它已经被加密了，肯定不可能是密码。

其次通过尝试可以发现，这个压缩包使用的不是伪加密 。

本WriteUp写于 1.17 20:00 到目前为止仍未有人联想到 txt 究竟是什么

txt 出现的地方很多，Windows 的后缀名、图片上的txt ······ 以及 DNS

Update: 为了避免p3成为竞速 Puzzle 后来加入了一句新的谜语

![image-20230118133934748](https://s2.loli.net/2023/01/18/Iw7KzuDVaP9LmBg.png)

导航系统在这里指 DNS，纸条指 TXT

是的，这个 txt 指的是DNS中的 record type。

查询 nwafu.studio 的 DNS 可以发现，根域名下存在一个 txt 的记录值。

![image-20230118131829242](https://s2.loli.net/2023/01/18/GpNThraisZ7xoXk.png)

2iP1o@55vv0RD3164b522e7466837af64aceefe9766e9

签名几个字符比较抽象，大概翻译一下就是压缩包的密码了。

Zip Password 3164b522e7466837af64aceefe9766e9

打开压缩包可以发现，nwafu-lab.txt 是乱码形式

![__OU_XZ5_`W4_5YWU3YPS_7.png](https://s2.loli.net/2023/01/18/KSDC21oXrjwqd3p.png)

其中的另一个文件，pri.key 似乎是一个GPG私钥

![_YXROG41JU_____IW~RPZ06.png](https://s2.loli.net/2023/01/18/yLNqM4fkPj8iWSh.png)

充分怀疑这个 txt 被 GPG 非对称加密过。

解密的方法也很简单，导入密钥，解密即可。

``$ gpg --import pri.key`
`gpg: key 06A77D1829D6D191: "nwafu-lab <admin@nwafu.studio>" not changed`
`gpg: key 06A77D1829D6D191: secret key imported`
`gpg: Total number processed: 1`
`gpg:              unchanged: 1`
`gpg:       secret keys read: 1`
`gpg:   secret keys imported: 1`

> $ gpg -o ans.txt -d nwafu-lab.txt
>
> gpg: encrypted with 3072-bit RSA key, ID F265C7B702F1D183, created 2023-01-15"nwafu-lab <admin@nwafu.studio>"

![image.png](https://s2.loli.net/2023/01/18/QTBbu3CJ9ZwfMop.png)

至此p3就结束了。

##### 实现

p3的实现更为简单，本次解谜域名使用 CloudFlare 进行托管，在控制台中添加 Record 即可。

![P8Y0K6M__QF62@D5A_I___0.png](https://s2.loli.net/2023/01/18/lsF65fSYR8kwyTb.png)

关于 GPG 的使用部分，这里不加赘述，有兴趣的参与者可以自己了解。

本次私钥为临时生成，公钥未上传至公钥服务器，无其他意义。

### 隐藏问题

在 puzzle.html 页面时，按下 F12 或者打开开发者工具，会发现在 Console 中有一行文字。

![0RIXXORPM3LWZ@@0WA~AL_2.png](https://s2.loli.net/2023/01/18/ImMStDBikfLzjW2.png)

如果完成了P1，任意使用一款带 “听歌识曲” 功能的播放器即可完成。

答案是:不存在的夏天。

### 彩蛋红包

本次解谜红包中所有作为样例的口令均对应一个红包。

比如 b9a11987  b9a119872023。

## 数据分析

### 网页访问情况

~~写不完了，先发了，等会再写~~

### 红包领取情况

~~写不完了，先发了，等会再写~~

## 总结

### 原因

关于我为什么要开设这次解谜，原因肯定不是撒币。

~~写不完了，先发了，等会再写~~

### 关于未领取红包的处理办法

未领取的红包将会在 GMT+8 2023.1.18 18:00 ~GMT+8 2023.1.21 18:00 内分为若干个子红包，子红包的金额由随机数产生，数量为 群成员/1.5 向上取整，直接公布红包口令。

### 尾声

本次解谜红包由我一人设计、部署、维护，红包与隐藏奖励也由我一人提供。希望能在新年即将来临之际，能为看到这里的你带了一些乐趣。如有不爽，欢迎加群来找我真人PVP。
