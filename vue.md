# vue设置代理
```
  //vue-cli3.0 里面的 vue.config.js做配置
devServer: {
    proxy: {
        '/rng': {     //这里最好有一个 /
            target: 'http://45.105.124.130:8081',  // 后台接口域名
            ws: true,        //如果要代理 websockets，配置这个参数
            secure: false,  // 如果是https接口，需要配置这个参数
            changeOrigin: true,  //是否跨域
            pathRewrite:{
                '^/rng':''
            }
        }
    }
  }
 ```
  我的 api='/rng'
我的请求地址 ${api}/xxxx/xxx ，请求地址就为 '/rng/xxxx/xxx'
当node服务器 遇到 以 '/rng' 开头的请求，就会把 target 字段加上，那么我的请求地址就为 http://45.105.124.130:8081/rng/xxxx/xxx
下面的 pathRewrite 表示的意思是 把/rng 替换为 空，那么我的请求地址就为 http://45.105.124.130:8081/xxxx/xxx（用在如果你的实际请求地址没有 rng 的情况）


# $emit 多个参数
```
子组件(参数以数组内多个子项形式发送)
this.$emit('sendData',[...args])
父组件
<Child @sendData="receiveData"></Child>

receiveData(args){
  console.log(args)
}
```

# store多个module
```
//state
  computed: {
    ...mapState({
      a: state => state.module1.a,
      b: state => state.module1.b
    })
  },
  
//mutations
   this.$store.commit('module1/changeState',data)
```

# axios 并发
```

```
