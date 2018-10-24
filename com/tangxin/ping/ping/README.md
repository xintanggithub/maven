## PingUtils

#### 一、引用

在app下的buid.gradle添加如下代码
```
buildscript {
    
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
    }
}

allprojects {
    repositories {
        google()
        jcenter()
        //添加这一部分 ↓↓↓↓↓↓↓↓↓↓↓↓
        maven{
            //这是maven仓库地址
            url 'https://raw.githubusercontent.com/xintanggithub/maven/master'
        }
        //↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

```

然后在需要使用的module对应的build.gradle内添加依赖

```
implementation 'com.tangxin.ping:ping:1.0.1'
```
#### 二、使用


```
    override fun pingStatus(pingResult: PingResult) {
         //pingResult.pingMessage  ping成功之后的信息，失败返回空
         //pingResult.pingStatus 0 失败  1成功
         }
    }, "需要ping的域名", "重试次数(不填默认1)", "重试间隔时间，毫秒（不填默认3000ms）").execute()
```

#### 三、注意

如果在可以ping通的情况下，无法ping通，请检查相关权限。

android下自带ping功能system权限即可。

[Runtime权限参考这里](https://blog.csdn.net/u011319826/article/details/49784623)



