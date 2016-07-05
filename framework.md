# Base
> Activity、Fragment、Adapter抽象基类定义
---
#### AbstractActivity
- **REQ_PERMISSION** 权限请求
- **TITLE_ACTIVITY** 页面标题
- **toolbar**
- **loadingDialog**
- **getResources** 设置默认资源，不跟随系统字体大小
- **setLayoutId** 设置布局Id 抽象
- **preInit** 初始化view前的内容
- **findViews** 获取View findViewById
- **initViews** view初始化
- **setUniversalToolbar** 设置通用的toolbar，设置标题、默认添加返回按钮,setUniversalToolbar只需调用一次
- **setToolbarTitle** 设置ToolbarTitle
-**snack** snackbar调用
-**toast** toast调用
-**netWorkUnable** 网络不可用
-**startPermissionSetting** 打开权限设置页面
-**hideKeyboard** 隐藏软件盘

