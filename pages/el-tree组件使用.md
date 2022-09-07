- 业务上使用了树结构做人员的选择功能，需要默认选择第一个数据，所以需要使用eltree的“current-node-key”属性设置默认选中节点。但发现默认参数没有作用。
- 由于默认数据是异步从数据库拿取，数据确实改变，但“current-node-key”没有生效。
- 后发现是由于更改默认id没有触发树状数据的更新，所以页面没有重新渲染。
- 解决方案：
- ```
   //el-tree组件触发选中节点的改变
   this.$nextTick(() => {
     this.treeCategory = this.treeData[0].value;
     this.$refs["treeRef"].setCurrentKey(this.treeCategory);
     this.loadTableData();
   });
  ```
-