- 1. 类似与Vue的``<keep-alive>``标签，创建动态组件，在第一次创建可以缓存下来，避免二次渲染浪费资源。
- 2.Angular本身没有类似与此的API，但提供了官方的路由服务``RouteReuseStrategy``，其中主要包括了几个APi来判断路由重复使用的方案
	- ``shouldDetach(route: ActivatedRouteSnapshot): boolean``，判断路由是否能重复使用，如果返回true，代表所有路由都接受服用；可以通过``route.routeConfig.path``路由内容添加规则。
	- ```
	  public shouldDetach(route: ActivatedRouteSnapshot): boolean {
	  return route.routeConfig.path == 'oneMap';
	  }
	  ```
	- ``store(route: ActivatedRouteSnapshot, handle: DetachedRouteHandle): void``当满足上方的规则，路由离开时触发该方法
	- ```
	  public store(route: ActivatedRouteSnapshot,handle: DetachedRouteHandle): void {
	      SimpleReuseStrategy.handlers[route.routeConfig.path] = handle;
	    }
	  ```
	-
	- ``shouldAttach(route: ActivatedRouteSnapshot): boolean``当路由进入时，可判断是否还原路由的暂存内容,判断之前是否在缓存中存入该部分缓存
	- ```
	  public shouldAttach(route: ActivatedRouteSnapshot): boolean {
	      return (
	        !!route.routeConfig && !!SimpleReuseStrategy.handlers[route.routeConfig.path]
	      );
	    }
	  ```
	- `` retrieve(route: ActivatedRouteSnapshot): DetachedRouteHandle``检索以前存储的路由并返回
	- ```
	  public retrieve(route: ActivatedRouteSnapshot): DetachedRouteHandle {
	      if (!route.routeConfig) {
	        return null;
	      }
	      return SimpleReuseStrategy.handlers[route.routeConfig.path];
	    }
	  ```
	- ``shouldReuseRoute(future: ActivatedRouteSnapshot,current: ActivatedRouteSnapshot): boolean``确定是否应复用路由,其中参数future、current是从路由树中逐步获取，future是上一步，current是下一步，判断其实是发生跳转（两个路由都对应路由树中）
	- ```
	  public shouldReuseRoute(future: ActivatedRouteSnapshot,current: ActivatedRouteSnapshot): boolean {
	      return future.routeConfig === current.routeConfig;
	    }
	  ```
- 3.注意适用范围，防止内存浪费问题，这也是Angular和React为什么一直没使用类似的原因，