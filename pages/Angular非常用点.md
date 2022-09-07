- 问：如何获取请求返回头（Response Headers）时间(或其他信息)？
	- 在访问请求头中加入*observe: 'response' as 'body'*;
	- ```
	      const header = {
	        observe: 'response' as 'body'
	      }
	      HTTP.GET(***).subscript(res=>{
	      res.headers.get('Date')
	      })
	  ```