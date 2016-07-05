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











































































