
# 第三方库
## framework
- volley:1.0.0 网络请求库 项目地址：[Github](https://github.com/mcxiaoke/android-volley)

> volley使用简介：
 ![工作原理图](http://static.open-open.com/lib/uploadImg/20151227/20151227213950_7.png)
上图是volley的工作原理图。
发送请求的3个步骤 ：1.创建RequestQueue对象，定义网络请求队列；2.创建XXXRequest对象(XXX代表String,JSON,Image等等)，定义网络数据请求的详细过程；3.把XXXRequest对象添加到RequestQueue中，开始执行网络请求。以下是一个代码示例:[参考](http://www.open-open.com/lib/view/open1451223702339.html)
``` java
        RequestQueue queue = Volley.newRequestQueue(getApplicationContext());
        //Volley提供了JsonObjectRequest、JsonArrayRequest、StringRequest等Request形式
        //1.Get
        String url = "http://www.baidu.com/newslist/";
        StringRequest request = new StringRequest(Request.Method.GET, url, new Response.Listener<String>() {
            @Override
            public void onResponse(String s) {
                // 打印出GET请求返回的字符串
                Toast.makeText(MainActivity.this, s, Toast.LENGTH_LONG).show();
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError volleyError) {
            }
        });
        
        // Post
        String url = "http://ce.sysu.edu.cn/hope/";
        // 创建StringRequest，定义字符串请求的请求方式为POST，
        StringRequest request = new StringRequest(Request.Method.POST, url, new Response.Listener<String>() {
            // 请求成功后执行的函数
            @Override
            public void onResponse(String s) {
                // 打印出POST请求返回的字符串
                Toast.makeText(MainActivity.this, "POST: " + s, Toast.LENGTH_LONG).show();
            }
        }, new Response.ErrorListener() {
            // 请求失败时执行的函数
            @Override
            public void onErrorResponse(VolleyError volleyError) {
            }
        }){
            // 定义请求数据
            @Override
            protected Map<String, String> getParams() throws AuthFailureError {
                Map<String, String> hashMap = new HashMap<String, String>();
                hashMap.put("phone", "11111");
                return hashMap;
            }
        };
        // 设置该请求的标签
        request.setTag("abcPost");
        // 将请求添加到队列中
        queue.add(request);
        
        //取消请求
        queue.cancelAll("abcPost"); //取消标签为abcPost的请求
        queue.cancelAll(this); //取消所有请求
```

- gson:2.4
- okhttp:2.5.0
- glide:3.7.0
- logger:1.11
- rxandroid:1.2.0
- eventbus:3.0.0

> 基本使用：
```java
    //注册
    EventBus.getDefault().register(this);
    //注册一个消息处理对象
    //EventBus.getDefault().register(new MyClass());  
```

- rxbinding:0.4.0
- rxjava:proguard-rules:1.1.5.0  rx混淆配置
- rxbinding:0.4.0
- rxlifecycle:0.6.1 和 rxlifecycle-components:0.6.1

> rxlifecycle用来严格控制由于发布了一个订阅后，由于没有及时取消，导致Activity/Fragment无法销毁导致的内存泄露。使用方式：Activity/Fragment需继承RxAppCompatActivity/RxFragment，目前支持的有RxAppCompatActivity、RxFragment、RxDialogFragment、RxFragmentActivity。

## App
- litepal:1.3.0  SQlite操作 项目地址：[Github](https://github.com/LitePalFramework/LitePal)
- com.android.support:percent:23.4.0 layout百分比布局库 
- ca.barrenechea.header-decor:header-decor:0.2.2 一个RecyclerView header库 项目地址：[Github](https://github.com/edubarr/header-decor)
- com.alexvasilkov:gesture-views:2.1.1 一个手势库 项目地址：[Github](https://github.com/alexvasilkov/GestureViews)
- MPAndroidChart:v2.2.4 开源图标库 项目地址：[Github](https://github.com/PhilJay/MPAndroidChart)
- advrecyclerview:0.9.1@aar 扩展RecyclerView 项目地址：[Github](https://github.com/h6ah4i/android-advancedrecyclerview)







