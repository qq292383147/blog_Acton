---
title: React Native 集成分享第三方登录功能分享第三方登录模块开发(Android)
date: 2019-12-20 13:28:22
tags: React & React Native
categories: React & React Native

---

在我们常用的App中经常会看到分享与第三方登录的功能，可以说分享与第三方登录已经成为了各大APP的必备功能。对于产品运行与推广来说，分享与第三方登录不仅能加强用户粘性，增加流量及新用户，也能提升用户存、留优化产品质量等。

![8](H:\上传到git里面的md文件图片\picture\React\8.gif)

各大平台都有对应的开发平台来提供分享与第三方登录的服务，比如微信开发平台/腾讯开发平台、新浪开发者平台等。因为各大平台及相关SDK存在很大的差异，单独集成起来比较繁琐，为了快速集成分享与第三方登录我们可以使用相应统一的服务提供商，常用的分享与登录的提供商有umeng与shareSdk。

截止目前，但各大平台与集成服务的提供方都只提供了Native版本的SDK，没有对React Native做支持，为此要在React Native应用中添加分享与第三方登录我们需要开发出能供React Native应用使用的分享与登录模块。

在这篇文章中我会向大家分享，在React Native中集分享第与三方登录功能的流程以及分享与第三方登录模块开发。(在本文中我将以umeng为例来进行讲解)



## 第一步：集成准备

首先我们需要到[umeng](http://www.umeng.com/)官网申请一个开发者账号。然后创建一个应用并[获取appkey](http://mobile.umeng.com/apps/new)。 在之后呢，我们需要进行必不可少的一步就是，到各大平台申请第三方开发者账号，关于申请的流程[官网文档](http://dev.umeng.com/social/android/operation)讲解的已经很详细了，在这里我不再重复了。

> 各大平台申请服务所需要等待的时间不等，通常是1-3天就可以搞定，建议在申请的同时，就进行sdk的集成，等申请通过之后，在换成正式的账号进行调试，这样一来开发申请两不误。

## 第二步：集成SDK

获取到appkey之后呢，我们接下来就来集成集成SDK。

友盟分享目前还不支持AndroidStudio的Gradle配置，所以我们需要将分享sdk下来然后倒入到项目中。

> 前往，[SDK下载中心](http://dev.umeng.com/social/android/sdk-download)，根据提示下载SDK即可，建议下载最新的。

将下载的压缩文件解压，或会看到如下目录：

![9](H:\上传到git里面的md文件图片\picture\React\9.png)

其中，`umeng_integrate_tool.jar`为，sdk辅助集成功能，双击该文件将打开如下界面：

![10](H:\上传到git里面的md文件图片\picture\React\10.png)

然后根据需要勾选相应的平台，单击ok即可生成`umeng_integratetool_result`文件夹，然后将该文件夹中的文件导入到项目中即可，关于详细的集成说明可以参考[快速集成](http://dev.umeng.com/social/android/quick-integration)。

## 第三步：构建分享及登录模块

为了能够在React Native中使用umeng分享及登录，我们需要为刚才导出的sdk创建一个Native 模块然后通过桥接的方式供js部分进行调用.

> 为了减少我们所集成的分享与统计代码对我们项目的入侵，保持模块之间的独立性，也就是我们经常所提倡的高内聚低耦合，在这里我们将分享与登录单独封装成一个模块，如下图：

![11](H:\上传到git里面的md文件图片\picture\React\11.png)

我们通过AndroidStudio新创建了一个名为`u_share`的module，然后将`umeng_integratetool_result`文件夹中的内容复制到了`u_share`的对应位置。

**创建UShare.java**

在u_share模块中我们创建了一个UShare.java类，该类主要负责umeng分享sdk之间的通信。

```java
 /** * 分享组件 * 出自：https://www.devio.org * GitHub:https://github.com/crazycodeboy * Eamil:crazycodeboy@gmail.com */
public class UShare {
    private static WeakReference<Activity> mActivity;
    private static WeakReference<ShareModel> mShareModel;

    public static void init(Activity activity) {
        if (activity == null) return;
        mActivity = new WeakReference<>(activity);
    }
    public static void share(final String title, final String content, final String imageUrl, final String targetUrl, final Callback errorCallback, final Callback successCallback) {
        if (mActivity == null) return;
        boolean granted = true;
        if (!TextUtils.isEmpty(imageUrl)) {
            granted = ContextCompat.checkSelfPermission(mActivity.get(), Manifest.permission.WRITE_EXTERNAL_STORAGE) == PackageManager.PERMISSION_GRANTED ? true : false;
        }
        if (!granted) {
            ShareModel shareModel=new ShareModel(title,content,imageUrl,targetUrl,errorCallback,successCallback);
            mShareModel=new WeakReference<>(shareModel);
            ActivityCompat.requestPermissions(mActivity.get(),new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, Constants.RC_REQUEST_PERMISSIONS);
            return;
        }
        mActivity.get().runOnUiThread(new Runnable() {
            @Override
            public void run() {
                openShare(title, content, imageUrl, targetUrl, errorCallback, successCallback);
            }
        });

    }
    //...省略部分代码，你也可以通过视频教程(http://coding.imooc.com/class/304.html)来学习实现分享第三方登录的具体细节
    public static void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        if(mShareModel==null)return;
        if (requestCode == Constants.RC_REQUEST_PERMISSIONS) {
            for (int i = 0, j = permissions.length; i < j; i++) {
                if(TextUtils.equals(permissions[i],Manifest.permission.WRITE_EXTERNAL_STORAGE)){
                    if (grantResults[i] == PackageManager.PERMISSION_GRANTED) {
                        share(mShareModel.get());
                    }else {
                        if(mActivity==null)return;
                        Toast.makeText(mActivity.get(),"没有使用SD卡的权限，请在权限管理中为GitHubPopular开启使用SD卡的权限",Toast.LENGTH_SHORT).show();
                    }
                }
            }
        }
    }
}
```



### 代码解读：

在上述代码中有个`public static void init(Activity activity)`方法来对UShare模块进行初始化。

另外，公共方法：

```java
public static void share(
    final String title,
    final String content,
    final String imageUrl,
    final String targetUrl,
    final Callback errorCallback,
    final Callback successCallback)
```

负责调用分享sdk之前的相应权限的检查。为了适配Android6.0的动态权限，我们添加了：

```java
public static void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (mActivity == null) return;
    UMShareAPI.get(mActivity.get()).onActivityResult(requestCode, resultCode, data);
}
public static void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
    if(mShareModel==null)return;
    if (requestCode == Constants.RC_REQUEST_PERMISSIONS) {
        for (int i = 0, j = permissions.length; i < j; i++) {
            if(TextUtils.equals(permissions[i],Manifest.permission.WRITE_EXTERNAL_STORAGE)){
                if (grantResults[i] == PackageManager.PERMISSION_GRANTED) {
                    share(mShareModel.get());
                }else {
                    if(mActivity==null)return;
                    Toast.makeText(mActivity.get(),"没有使用SD卡的权限，请在权限管理中为GitHubPopular开启使用SD卡的权限",Toast.LENGTH_SHORT).show();
                }
            }
        }
    }
}
```

来进行动态权限的处理。

> 关于登录：

分享和登录采用的是同一套sdk，如果要在React Native中进第三方登录，只需要在上述代码中添加下面的代码即可，方法和调用分享是一样的，有需要的朋友可以参考[登录集成](http://dev.umeng.com/social/android/login-page)来添加一下。

> mShareAPI.getPlatformInfo(UserinfoActivity.this, SHARE_MEDIA.SINA, umAuthListener);



**创建UShareModule.java**

然后我们创建了UShareModule.java来暴露分享方法：

```java
/** * 分享组件 * 出自：https://www.devio.org * GitHub:https://github.com/crazycodeboy * Eamil:crazycodeboy@gmail.com */
public class UShareModule extends ReactContextBaseJavaModule{

    public UShareModule(ReactApplicationContext reactContext) {
        super(reactContext);
    }

    @Override
    public String getName() {
        return "UShare";
    }
    @ReactMethod
    public static void share(
        String title,
        String content,
        String imageUrl,
        String targetUrl,
        final Callback successCallback,
        final Callback errorCallback) {
        UShare.share(
            title,content,imageUrl,targetUrl,successCallback,errorCallback
        );
    }
}
```

在UShareModule.java中我们通过`UShare.share(title,content,imageUrl,targetUrl,successCallback,errorCallback);`调用了`UShare.java`的分享方法来打开分享对话框。

**创建UShareReactPackage.java**

为了向React Native注册我们刚才创建的原生模块，我们需要实现ReactPackage，ReactPackage主要为注册原生模块所存在，只有已经向React Native注册的模块才能在js模块使用。

```java
/**
 * 分享组件
 * 出自：https://www.devio.org
 * GitHub:https://github.com/crazycodeboy
 * Eamil:crazycodeboy@gmail.com
 */
public class UShareReactPackage implements ReactPackage {

    @Override
    public List<Class<? extends JavaScriptModule>> createJSModules() {
        return Collections.emptyList();
    }

    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }

    @Override
    public List<NativeModule> createNativeModules(
            ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();
        modules.add(new UShareModule(reactContext));
        return modules;
    }
}
```

**环境配置**

因为分享与登录SDK需要用到一些权限、Activity、Appkey及相关第三方key的配置，所以我呢需要在AndroidManifest.xml文件中添加如下的代码：

```java
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.jph.u_share">
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_LOCATION_EXTRA_COMMANDS" />
    <application
        android:allowBackup="true"
        android:supportsRtl="true">

        <!--weixin callback-->
        <activity
            android:name="com.jph.githubpopular.wxapi.WXEntryActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:exported="true"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />
        <!--qq callback-->
        <activity
            android:name="com.tencent.tauth.AuthActivity"
            android:launchMode="singleTask"
            android:noHistory="true">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />

                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />

                <data android:scheme="xxx" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.tencent.connect.common.AssistActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:screenOrientation="portrait"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" />

        <!--umeng：-->
        <!--分享编辑页：-->
        <activity
            android:name="com.umeng.socialize.editorpage.ShareActivity"
            android:excludeFromRecents="true"
            android:theme="@style/Theme.UMDefault" />
        <meta-data
            android:name="UMENG_APPKEY"
            android:value="UMENG_APPKEY"></meta-data>

    </application>

</manifest>
```



上述代码根据所选择的平台不同而略有差异，具体可参照[快速集成](http://dev.umeng.com/social/android/quick-integration)：

## 第四步：分享模块的使用

到目前为止呢，我们的Android分享模块已经创建好了，接下来呢我们就可以使用它了。

**添加依赖**

首先我们需要让我们的应用模块依赖`u_share`模块：

在…/xxx/android/app/build.gradle中添加：

```react
dependencies {
    + compile project(':u_share')
}
```

**初始化UShare**

接下来我们需要在`MainActivity.java`中初始化UShare：

```java
public class MainActivity extends ReactActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        +UShare.init(this);
    }
    //...省略部分代码
    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        +UShare.onRequestPermissionsResult(requestCode,permissions,grantResults);
    }
}
```



**注册UShareReactPackage** 然后我们需要在MainApplication.java中注册UShareReactPackage以及进行Umeng的一些配置。

```java
public class MainApplication extends Application implements ReactApplication {
  {
    +PlatformConfig.setWeixin(Constants.KEY_WEIXIN,Constants.SECRET_WEIXIN);
    +PlatformConfig.setSinaWeibo("xxx", "xxx");
    +PlatformConfig.setQQZone("xxx", "xxx");
  }

  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    @Override
    protected boolean getUseDeveloperSupport() {
      return BuildConfig.DEBUG;
    }
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
         + new UShareReactPackage()
      );
    }
  };
  @Override
  public void onCreate() {
    super.onCreate();
    SoLoader.init(this, /* native exopackage */ false);
    +UMShareAPI.get(this);
  }
}
```

**原生模块导出一个js模块**

我们创建一个UShare.js文件，然后添加如下代码：

```react
import { NativeModules } from 'react-native';
module.exports = NativeModules.UShare;
```

这样以来呢，我们就可以在JS模块中来使用分享以及第三方登录了：

```react
import UShare from '../common/UShare'//导入UShare.js
//...省略部分代码
UShare.share(shareApp.title, shareApp.content,
    shareApp.imgUrl,shareApp.url,()=>{},()=>{})
```