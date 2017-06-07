# AndroidGradleSimpleConfig
Android使用Gradle的一些简单配置让开发更方便

##开发、准生产、生产多服务器，需要不同的 URL，打包时需要切换
###第一步：
首先打开 module 的 build.gradle在 android 根中添加下面的代码
```
productFlavors {
        dev {
        resValue "string", "app_name", "sumile_dev"
        }
 
        userTest {
        resValue "string", "app_name", "sumile_test"
        }
 
        online {
        resValue "string", "app_name", "sumile_online"
        }
 }
```  

其中 dev、userTest、online 等名字以及个数都可以改变，大小写不限（但是不可以使用 test）
productFlavors 通常是指同一软件的不同版本，比如收费版，免费版。这个版本的逻辑肯定是不一样的。它会自动将不同版本的代码分别和通用代码进行组合，根据上面的配置打包成不同的包。关于 productFlavors 详细的介绍，参考这里的 [官方文档](https://developer.android.com/studio/build/build-variants.html#product-flavors) 或我保存的 [这份 (product-flavors)](https://sumile.cn/wp-content/uploads/2017/06/product-flavors.pdf)（没法翻墙的话）  

根据上面的介绍，其实可以想到，我们要做的切换 url 打包，其实就类似于多个不同的版本，版本与版本之间请求的服务器的地址是不同的
![](http://upload-images.jianshu.io/upload_images/2406260-1cbc774d6b191e77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
###第二步：  
首先打开 src 目录，在 src 目录下按照第一步中的三个版本的名字新建文件夹 ![](http://upload-images.jianshu.io/upload_images/2406260-46f9f074e33a6dd4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
之后在其中一个文件夹下面新建一个文件夹 ,java 下面新建包，包名可以和你 main 下面的不同
![](http://upload-images.jianshu.io/upload_images/2406260-73041942623a5b5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
然后复制 java 目录到其他文件夹下面 (本例中为 dev 和 online)  
最终结果如下：  
![](http://upload-images.jianshu.io/upload_images/2406260-b854a197614bfc67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
从图中可以看到，userTest 文件夹下面的代码被识别成了 java, 而其他的两个没有被识别到。
你连着手机点运行的时候，这里被识别成 java 的代码就会是当前使用的代码，另外两个不会被使用到，如果你想要切换使用的代码，只要在 Build Variants 中切换就好了  
![](http://upload-images.jianshu.io/upload_images/2406260-c2adf612c82ef25d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
另外要着重注意的是：  
如果 main 文件夹下的 package 名和你新建的 package 名相同的话，同目录下不要有相同的文件  
![](http://upload-images.jianshu.io/upload_images/2406260-e08c94dbd68b3937.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
接下来在每个 Config.java 中定义它们不相同的地方，比如下面我把它们每个 url 都加上了它们的版本的名字  
![](http://upload-images.jianshu.io/upload_images/2406260-9c7fc69a5980f9eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
然后就正常使用就可以了  
![](http://upload-images.jianshu.io/upload_images/2406260-20767a4726248fc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
接下来打包  
![](http://upload-images.jianshu.io/upload_images/2406260-04d33cb94f563d44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
按住 shift 可以都选择了，finish 完成，就可以打出来三个包了  
![](http://upload-images.jianshu.io/upload_images/2406260-749107b5ac5bc78e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##调试微信—每次必须打包再测的烦恼
module 的 build.gradle 的 android 下面添加如下代码：   


	signingConfigs {  
	 debug {
	        storeFile file("../keystore.jks")
	        storePassword "123456"
	        keyAlias "sumile"
	        keyPassword "123456"
	 }
	}


路径可以根据相对或绝对路径去匹配，推荐还是相对路径（使用 Mac 和 Windows）这样配置的话，以 debug 模式运行的时候用的就是配置的 keystore 了  

详细介绍:[我的博客](https://sumile.cn/archives/1788.html "我的博客")   [简书](http://www.jianshu.com/p/640a97d28676 "简书")