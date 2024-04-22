##### 本地启动
```
npm i
npm run init 
npm run dev:<子包名>
```

##### 子包安装依赖
进入子包目录 npm i <包名>

##### 代码校验
```
formula lint -p // 校验
formula lint -p --fix // 校验 + 修复
```

##### ark请求服务端接口设计

在package.json中 执行指令上更改服务器名 实现请求后端接口
```
"dev:app-item": "LOGAN_PROXY_KEY=ark-zhengyang1-sit lerna run --parallel --scope='ark-app-{root,layout,basics,item}' dev"

LOGAN_PROXY_KEY=ark-zhengyang1-sit // 设置后端接口
```

##### 切换启动子包服务
ark 想要运行另一个子包需要停止当前子包服务, 不然端口会被占用

--scope='ark-app-{root,layout,basics,item}' dev" // app-{ 子包名 } // 填入子包名编译时就可以将该子包打包编译

##### 切换后端代理地址beta环境，本地开发

1.cookie替换

用beta的token替换local的
![[Pasted image 20240422193221.png]]2.接口屏蔽

/ark-sit-yiyou/api/rocking/tob/resource/plan/batch/get

![[Pasted image 20240422193242.png]]

#### ark 本地无法执行
```
npm run clear // 删除本地所有依赖包(包括子包)
npm run init
```