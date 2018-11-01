# JDDownload

适用于应用市场、多文件下载、大文件下载场景使用。

### 引入
1. 项目根目录build.gradle添加
```
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
        

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()
        //添加这句代码↓↓↓↓↓↓↓↓↓↓↓↓
        maven{
            url 'https://raw.githubusercontent.com/xintanggithub/maven/master'
        }
        //↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```
2. 对应module下build.gradle添加

```
implementation 'com.jd.download:lib:1.0.0'
```
### 使用
1. 在application的onCreate中初始化，以及配置

```
public class MyApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        JDDownLoadManager.getInstance().init(this)
                .setDebugLog(true) //开始debug日志
                .setMaxThreadCount(3)//并发任务3个
                .setRetryCount(2);//重试次数2次
    }

}
```
2. 绑定下载监听

```
//绑定，一般在activity的onCreate （view初始化的地方）
JDDownLoadManager.bindChangeListener(downloadListener);
//解绑，一般是activity的onDestroy (销毁的)
JDDownLoadManager.unBindChangeListener(downloadListener);


//监听器
DownloadListener downloadListener=new DownloadListener() {
    @Override
    public void completed(BaseDownloadTask task) {
        //下载完成  
    }
};

```
其中completed为必须实现的回调方法，还有其他的下载状态变化回调方法可根据需求使用:

```
    //已准备好，等待下载，此时已经在下载队列   soFarBytes 已下载的byte数，totalBytes总大小
    void pending(BaseDownloadTask task, int soFarBytes, int totalBytes);
    //下载进度
    void progress(BaseDownloadTask task, int soFarBytes, int totalBytes);
    //暂停
    void paused(BaseDownloadTask task, int soFarBytes, int totalBytes);
    //错误
    void error(BaseDownloadTask task, Throwable e);
    //下载完成
    void completed(BaseDownloadTask task);
    //存在开始了一个在下载队列里已存在的任务(默认继续下载，不创建新的下载任务)
    void warn(BaseDownloadTask task);
    //重试  retryingTimes 重试次数  ex异常信息  soFarBytes 已下载大小
    void retry(BaseDownloadTask task, Throwable ex, int retryingTimes, int soFarBytes);
    
```
BaseDownloadTask里包含了当前下载任务的信息，包括：下载任务的ID、下载的url，保存在本地的路径等等。下载任务的ID是下载任务的标识，可以根据ID查询下载状态、大小等。


3. 下载任务的操作

```

//开始（创建/继续）
JDDownLoadManager.getJDDownloader().start(url);
//如果没有设置监听器，这里也可以设置
JDDownLoadManager.getJDDownloader().start(url,listener);

//暂停 id为BaseDownloadTask返回的任务ID
JDDownLoadManager.getJDDownloader().pause(id);
//暂停所有
JDDownLoadManager.getJDDownloader().pauseAll();

//清除单条下载任务数据 id为BaseDownloadTask返回的任务ID
//path为该文件本地存储的路径
JDDownLoadManager.getJDDownloader().clear(id,path);
//清除所有下载任务数据
JDDownLoadManager.getJDDownloader().clearAll();

//根据ID获取下载进度
JDDownLoadManager.getJDDownloader().getSoFar(id);
//根据ID获取下载任务对象总大小
JDDownLoadManager.getJDDownloader().getTotal(id);

```
