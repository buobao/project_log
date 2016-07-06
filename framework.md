# Base
> Activity、Fragment、Adapter抽象基类定义
---
#### AbstractActivity
- **REQ_PERMISSION** 权限请求
- **TITLE_ACTIVITY** 页面标题
- **toolbar**
- **loadingDialog**
- **getResources()** 设置默认资源，不跟随系统字体大小
- **setLayoutId()** 设置布局Id 抽象
- **preInit()** 初始化view前的内容
- **findViews()** 获取View findViewById
- **initViews()** view初始化
- **setUniversalToolbar()** 设置通用的toolbar，设置标题、默认添加返回按钮,setUniversalToolbar只需调用一次
- **setToolbarTitle()** 设置ToolbarTitle
- **snack()** snackbar调用
- **toast()** toast调用
- **netWorkUnable()** 网络不可用
- **startPermissionSetting()** 打开权限设置页面
- **hideKeyboard()** 隐藏软件盘
- **showLoadingDialog()** 显示进度dialog
- **cancelLoadingDialog()** 取消进度dialog

#### AbstractFragment
- **iSnack** snackbar接口，用于显示snackbar
- **isPrepared** 标志初始化是否完成
- **isVisible** 页面是否可见
- **loadingDialog** 加载进度框
- **setUserVisibleHint()** 这里复写fragment的该方法，1.获取当前是否可见，2.实现控制数据加载（当前fragment可见时加载数据）
- **onVisible()** 可见时处理，加载数据
- **onInvisible()** 不可见时处理
- **lazyLoad()，loadDate()** 加载数据

#### AbstractWrapAdapter
> 处理两件事：1.item点击事件；2.是否可加载更多。

#### BaseAdapter1
> 单ViewType adapter封装

# Glide
> Glide基本封装、自定义Module、log等
---
#### GloadLoad
- **loadWithWifi** 只在wifi下加载
- **isWifi** 是否wifi网络
- **init()/initUri()** init for Activity/Fragment SDK<21不设置内存缓存
- **initRound()** 初始化圆形加载
- **needLoad()** 是否需要加载
- **load()/loadBig()/loadTransformation()** 图片加载

#### GlobalGlideModule
> glide自定义设置，这里设置内容：1.设置磁盘缓存大小，2.设置内存缓存区，3.指定默认into View。

#### LogListener
> glide加载日志监听

#### RoundImageViewTarget
> 圆形图片加载

# Model
> 几个基本Gson实体模型定义：1.CoreResponse 基本Response模型，2.UploadModel 数据上传json模型，3.AppUpdateProgress app升级进度
---

# Network
> 网络请求库
---
#### OkHttpStack
> OkHttp请求栈，用于创建volley请求队列实例：requestQueue = Volley.newRequestQueue(context, new OkHttpStack());

#### VolleyRequestQueue
> volley请求队列，单例

#### VolleyMethodToString
> 请求码转换：0-Get，1-Post，2-Put，3-Delete，4-Head，5-Options，6-Trace，7-Patch

#### GsonRequest
> 自定义Gson Request ，配置GSON解析请求数据

#### AbstractFrameApi
> 请求接口抽象类
- **startRequest()** 这里发送一个gsonresquest到请求queue 

#### JsonEncode
> json格式请求参数格式化
- **encode()** 将object转换为json字符并返回制定charset的字节数组

#### ProgressRequestBody
> 上传进度，继承自OkHttp RequestBody

#### ProgressResponseBody
> 下载进度

#### AbsJsonRequest
> 功能与GsonRequest类似，尚不知额外用途

#### AbsApi
> api抽象基类

#### ArrayMapEncode
> 参数格式化工具
- **encode()** 以指定字符集格式化arrayMap，返回byte[]

#### CoreApi
> 继承自AbsApi api抽象基类

#### JsonApi
> json格式参数请求,请求方式：POST

#### UniversalApi
> 通用请求Api,抽象基类,继承自AbstractFrameApi

# Rx
> 几个Subscribe定义，FolderSizeOnSubcribe：文件大小计算，ClearGlideCacheOnSubscribe：清除glide缓存

# Service
> android service组件定义，ApkDownloadService：App下载安装；ImageDownloadService：图片下载；UploadService：文件上传

- **ApkDownloadService** 
```java
//下载app
private void checkAndDownload() {
        //包名
        String apkName = AppUpdateUtil.getApkName(this);
        //app 下载url
        String url = AppUpdateUtil.getUpdateUrl(this);
        //更新版本号
        int versionCode = AppUpdateUtil.getUpdateVerisonCode(this);
        // SD卡是否可用
        if (TextUtils.isEmpty(cacheDir)) {//SD卡不可用
            builder.setContentText(getString(R.string.sd_unable))
                    .setAutoCancel(true);
            notification();
            stopSelf();
            return;
        }
        //加载地址是否可用、版本号是否正确
        if (TextUtils.isEmpty(url) || versionCode <= 1) {
            Logger.t(TAG).e("下载地址为空 or VersionCode<=1");
            stopSelf();
            return;
        }
        final File file = new File(new File(cacheDir), apkName + "-" + versionCode + ".apk");
        if (file.exists()) {//已经下载到本地
            successAndInstall(file);
            EventBus.getDefault().post(new AppUpdateProgress(0, 100));
            return;
        }
        downloadTask = new DownloadTask(ProgressHelper.addProgressResponseListener(okHttpClient, new IProgressResponseListener() {
            @Override
            public void onResponseProgress(long bytesRead, long contentLength, boolean done) {
                Logger.t(TAG).d("progress read : %d - sum : %d - progrss : %d - done : %b", bytesRead, contentLength, bytesRead * 100 / contentLength, done);
                if (done) {//下载完成
                    successAndInstall(file);
                    EventBus.getDefault().post(new AppUpdateProgress(0, 100));
                } else {//正在下载
                    final int current = (int) (bytesRead * 100 / contentLength);
                    if (current > progress || progress == 0) {
                        progress = current;
                        builder.setContentText(getString(R.string.app_update_progress))
                                .setContentInfo(progress + "%")
                                .setProgress(100, progress, false)
                                .setAutoCancel(false);
                        notification();
                        EventBus.getDefault().post(new AppUpdateProgress(1, progress));
                    }
                }
            }
        }), url, file, new IDownloadResult() {
            @Override
            public void result(boolean error) {
                if (error) {//下载失败
                    if (file.exists()) {
                        file.delete();
                    }
                    builder.setContentText(getString(R.string.app_update_error))
                            .setContentInfo("")
                            .setProgress(0, 0, false)
                            .setContentIntent(PendingIntent.getService(ApkDownloadService.this, 0, new Intent(getApplicationContext(), ApkDownloadService.class), PendingIntent.FLAG_CANCEL_CURRENT))
                            .setAutoCancel(true);
                    notification();
                    EventBus.getDefault().post(new AppUpdateProgress(2, 0));
                    stopSelf();
                }
            }
        });
        downloadTask.execute();
    }
```













































































