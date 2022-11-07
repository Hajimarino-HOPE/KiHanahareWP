# [SWPU 2019] 部分wp

## Reverse

### ThousandYearsAgo

拿到题目，是一个apk，条件反射扔进jadx，发现打不开

尝试直接解压，发现有东西

![image-20221107084827407](https://nssctf.wdf.ink/img/yxy/image-20221107084827407.png)

原来你被骗了T_T，把app2扔到jadx里，分析程序逻辑，发现有一块始终无法执行，猜测是flag

考点：**JNI**

- 直接找 **native-lib**

![image-20221107085047629](https://nssctf.wdf.ink/img/yxy/image-20221107085047629.png)

JNI？什么JNI？？？倒回去看看

![image-20221107085117008](https://nssctf.wdf.ink/img/yxy/image-20221107085117008.png)

原来是JNI啊，JNI（Java Native Interface）即通过使用 Java 本地接口书写程序，可以确保代码在不同的平台上方便移植，现在的目标就是找的 **native-lib** ，liib开冲

![image-20221107085419307](https://nssctf.wdf.ink/img/yxy/image-20221107085419307.png)

接下来直接从压缩软件把这个lib提取出来，扔IDA开始分析，这么大一串直接上来找main函数

![image-20221107085649173](https://nssctf.wdf.ink/img/yxy/image-20221107085649173.png)

然后看到了flag

![image-20221107085719119](https://nssctf.wdf.ink/img/yxy/image-20221107085719119.png)

但事实上这个是个flag密文，也就是假的flag，我们需要通过以下逻辑来decode

可以看到最后return的是 **v1** ，前面的v3和MD5Final都是打幌子，追踪 **decode** 函数

![image-20221107085958987](https://nssctf.wdf.ink/img/yxy/image-20221107085958987.png)

main函数得知，参数分别是v1和5，拖下来魔改一波，编译运行得到flag

```c
#include<stdio.h>
#include<string.h>

int main(void)
{
  int i; // [rsp+10h] [rbp-10h]
  char a1[128]="flag{WeLcome_to-SWPU}}";
  for ( i = 0; i < strlen(a1); ++i )
  {
    if ( a1[i] < 65 || a1[i] > 90 )
    {
      if ( a1[i] >= 97 && a1[i] <= 122 )
        a1[i] = (5 + a1[i] - 97) % 26 + 97;
    }
    else
    {
      a1[i] = (5 + a1[i] - 65) % 26 + 65;
    }
  }
  printf("%s\n",a1);
  return 0;
//   return a1;
}
```

![image-20221107090108136](https://nssctf.wdf.ink/img/yxy/image-20221107090108136.png)