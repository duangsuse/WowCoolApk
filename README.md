# WowCoolApk

😃 Compute [api.coolapk.com](https://api.coolapk.com/v6/main/init) API `X-App-Token` HTTP Request header with Scala CLI Application

## Why？How can I use that API？

酷安的 HTTP 接口会利用 `X-App-Token` HTTP 请求头参数校验客户端的身份，这就是所谓的『接口验证』

酷安的客户端内部肯定要有这么一个生成算法，不过为了『100% 保密安全』是用 C 程序设计语言编写的，一般人看不懂也不知道怎么 RE

不过很可惜的是，虽然酷安的高级技术人员们，有心炫技弄了这个 native 的访问令牌生成算法，可是不知道把某知名 <abbr title="从调试符号里泄漏出来的，别担心，酷安没有”被“开源">CoolLibrary</abbr> 项目怎么就弄成了 Debug Release 😂

令人笑破肚皮了，这是生怕别人逆向工程不出来整个算法啊！__于是酷安的 liba.so 到这里彻底爆破翻车__，一个晚上就被人还原了所有逻辑，还被几个某安基佬 C, Ruby, Python, Java, Scala 五国语言重写了一遍，真是酷安史上最大发布翻车。

实在是属于项目构建打包发布管理严重纰漏，要不然就是开发部水准不过关啊！（暴论）

如果接口验证不能通过并且需要的 HTTP 头没有提供，则接口（2019.5.5 止）会返回 __404 Not Found__
也加了令牌无效和令牌过期验证，不过很可惜，现在都作废了.... 🤔

这个故事告诉我们，不要以 Debug 模式发布任何应用程序。
另外也提醒了我们，用 C 写，编译成机器码，也未必就是所谓的『完全黑盒』了。

```http
X-Sdk-Int: 25
X-Sdk-Locale: zh-CN
X-App-Id: com.coolapk.market
X-App-Version: 9.0.2
X-App-Code: 1902151
X-App-Token: getCoolToken()
```

## DMCA Takedown

欢迎，不过 v6 沿用至今的算法怕是不能用了，要不然，得强迫所有 v8 的用户更新

## Compiling

Just compile WowCoolApk.scala with `scalac`:

```bash
scalac -optimise -target:jvm-1.7 WowCoolApk.scala
```

The default output classpath is `.`

## Running

With `scala` command:

```bash
$ scala WowCoolApk
Run WowCoolApk help to show help
```

with `java` command and __scala-library.jar__:

```bash
$ java -cp /usr/share/java/scala/scala-library.jar:. WowCoolApk gen
91e0857e3de6b1dff2bcfa4bd31f051900000000-0000-0000-0000-0000000000000x5ccdbac9
```

## Command-line Usage

```bash
$ scala WowCoolApk help
Generate X-App-Token HTTP request header field for com.coolapk.market
Usage:
WowCoolApk <help -help -h --help> Get help
WowCoolApk <uuid> 
WowCoolApk <uuid> <date>
WowCoolApk <uuid> <date> <apk>
WowCoolApk <flag: _ or q or v> rest... [! to use defaults]
WowCoolApk <get gen> generate one with default options:
 0*-0*-0*-0*-0* as UUID,
 $PACKAGE as market,
 current time as time
```

+ WowCoolApk: How to get help?
+ WowCoolApk <get|gen>: Get token with default parameters
+ WowCoolApk: <help -help -h --help> Get help
+ WowCoolApk \[uuid\] \[date\] \[apk\]: Generate with options override
+ WowCoolApk \[qv\_\] \[!\]: Generate with flags (q -> be quiet, v -> be verbose) 
+ WowCoolApk \[qv\_\]\[uuid\] \[date\] \[apk\]: Generate with flags and options override
  
## Scala library

```scala
$ scala -i WowCoolApk.scala 
Loading WowCoolApk.scala...
import java.util.Base64
import java.util.Date
import java.text.SimpleDateFormat
import java.text.ParseException
import java.security.MessageDigest
import java.security.NoSuchAlgorithmException
defined module WowCoolApk

Welcome to Scala version 2.10.6 (OpenJDK 64-Bit Server VM, Java 1.8.0_201).
Type in expressions to have them evaluated.
Type :help for more information.

scala> WowCoolApk.getCoolToken("com.coolapk.market", new Date, "|uuid|")
res0: String = 555a63b38d01e2c6d02191e2caa927fa|uuid|0x5ccdbe03
```
  
## See also

+ [PinkD/CoolApkApiTokenGenerator](https://github.com/PinkD/CoolApkApiTokenGenerator/blob/master/CoolApkApiTokenGenerator.java) (java program)
+ [bjzhou/Coolapk-kotlin](https://github.com/bjzhou/Coolapk-kotlin)
