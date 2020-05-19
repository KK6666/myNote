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

# axios同时发送多个或不定量的请求，并等待所有请求完毕后操作
```
//同时发送多个请求，Axios.spread中的函数，请求全部完成后会调用，并且请求数据会一一对应参数。
 Axios.all([request1, request2, request3])
      .then(
        Axios.spread((res1, res2, res3) => {
          console.log('全部加载完成')
        })
      )
      .catch(err => {
        console.log(err.response)
      });
//发送的请求数不确定时，使用map结合Axios.all，arr是会灵活变化的数组，根据map方法返回多个promise。
 Axios.all(arr.map(function (data)=>{
  	return this.axios.post(....)
  }))
      .then(
        Axios.spread((...a) => {
          console.log('全部加载完成')
        })
      )
      .catch(err => {
        console.log(err.response)
      });
```
