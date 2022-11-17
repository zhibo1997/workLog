- ## 问题发现：
	- 同样的系统发现在不同手机上显示效果不同，发现在安卓和无刘海屏苹果手机上显示正常；但在iphone10版本及以上手机系统上发现底部的支付栏被遮挡住了部分
	- ![image.png](../assets/image_1665740964142_0.png)
- ## 问题原因：
	- iphoneX版本及以上，苹果都使用了刘海屏作为手机屏幕的主要方式；在Safari浏览器上，为了给网站更多的空间，会给浏览器流出一部分安全距离，导致其高度比平常会更大，所以100vh不是真正的窗口高度。
- ## 解决方法
	- ```
	  height: calc(100vh - 60px - constant(safe-area-inset-bottom)); 
	  /* 兼容 iOS < 11.2 */
	  height: calc(100vh - 60px - env(safe-area-inset-bottom));
	  /* 兼容 iOS >= 11.2 */
	  ```
-