### 初始化
  1.容器必须有宽度/高度
  2.创建代码需要写在容器节点生成后，如mounted，this.$nextTick()
### 响应式
```
  window.onresize = () => {
    //  根据窗口大小调整曲线大小
     myChart.resize();
  };
```
