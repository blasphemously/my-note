#### 环境解释
个人beta: 个人beta(staging) 环境, 上线前环境, 就是部署泳道我这一个模块的泳道

staging.  // 就是beta
测试环境分为 大beta  个人beta  //  大   意为 统一  不是个人的泳道部署
            大sit   个人sit


##### 个人代理
通过配置个人proxy[[https://logan.devops.xiaohongshu.com/project-proxy/list]]
proxy地址可以指向后端项目部署url

##### 发布流程
部署流程: 在小红书前端平台  一个需求部署一个流水线,  要发布的时候选择一个发布环境(sit 等) 进行发布 
