- 问：如何在表单初始化时触发表单的验证功能
	- 答：
	- ```
	  //将输入框设置为touched，变为已触摸状态
	  this.form.get('idcardNum').markAsDirty();
	  //将输入框
	  this.form.get('idcardNum').updateValueAndValidity();
	  ```