- ##  uniApp使用限制（what）
	- 使用地图框架问题：uniApp在打包成安卓App的时候由于本身的框架限制，不能使用绑定dom的插件，例如G2、原装ehcarts等可视化插件，Mapbox等地图插件都不直接使用.
- ## 出现原因（原理）(why)
	- 使用UniApp的renderjs模式 [uni-app中使用mapbox地图](https://blog.csdn.net/hello_yihao/article/details/116055805)；另一处坑，同时renderjs又引入UniApp中的一个底层逻辑叫 [逻辑层和视图层](https://uniapp.dcloud.net.cn/tutorial/renderjs.html#renderjs)，介绍了uniapp未解决一套代码编译不同平台的问题，使用了逻辑层和视图层的区分，其中逻辑层是用于进行基础代码操作，视图层负责页面渲染；js和css，但其实，编译时就已经将它们拆分了。
- ## 解决方法(how)
	- 开启两块`script`，一部分使用renderjs模式直接调用，这部分js代码属于**视图层**代码，可以直接绑定dom元素，但uniApp原生代码，例如uni.request、onload等api都不可以使用，并且外部使用mixins混入的全局代码被 ^^隔离^^；另一部分属于原有的**逻辑层**代码，与原来保持不变。
	- 最主要的两层的通讯逻辑，原来默认使用的是内部mixins，现在只能使用类似于父子组件传值的方式， [uniApp逻辑层与视图层通讯方式](https://www.jianshu.com/p/7ee44644c1bc)
	- ```
	  <view
	    id="mapboxmap"
	    :styleJson="styleJson"
	    :change:styleJson="mapbox.setAssetStyle"
	    :locationPoint="locationPoint"
	    :change:locationPoint="mapbox.location"
	  >
	  </view>
	  //视图层通过监听传入值的改变来更改数据值
	  <script module="mapbox" lang="renderjs">
	  export default {
	      mounted(){
	      //调用ownerInstance函数，第一个参数“showPopup”是逻辑层的函数名称，第二个参数为传入的值
	      	this.$ownerInstance.callMethod(
	            "showPopup",
	            features[0].properties.itemIndex
	          );
	      },
	  	methods:{
	  		setBounds(bounds) {
	  			this.bounds = bounds;
	  		},
	  		setAssetStyle(styleJson) {
	  			this.styleJson = styleJson;
	  		},
	      }
	  }
	  </script>
	  ```
- ## 结论
	- 最后解决了uniApp中使用mapbox的问题，但是可以看出来，最后付出的代价较大，本系统因为地图操作较少，所需要到的通讯代码较少，而且由于视图层不能使用许多方法，使用vuex仓库或是缓存都会出现问题，所以不建议使用UniApp开发地图应用（APP、小程序，H5端使用没问题）。