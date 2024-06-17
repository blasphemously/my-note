# 投放计划
  入口 鹰眼电商运营平台-> 营销 -> 策略营销中心 -> 投放计划
## 新建计划  新页面
## 投放计划列表 (原本页面)
version
0 或者 1
用于分别出新老投放计划
区分新投放计划 老投放计划, 列表 查看 复制功能 根据新老投放计划, 区分页面
strategyGroupNum
现在改成
strategyNum

/api/usergrowth/launchplan
get
### 新投放计划  列表 查看 复制
### 老投放计划 列表 查看 复制 不变


## 投放计划详情(查看计划) 新页面

### 新投放计划详情
根据旧投放计划下方改为新表单
0 老投放计划
不动

1 新投放计划
查询投放计划下的策略list
入参数带上launchPlanId
https://edith.devops.xiaohongshu.com/pages/manage/search/viewer?id=13577

下面新策略列表(添加请求参数launchPlanId)
https://hawkeye.devops.xiaohongshu.com/api/marketing/strategy_list?nameLike=&status=ONLINE&maxStartAt=1716867415&minEndAt=1716867415&page=1&pageSize=10

#### 新建策略 (新页面)
##### 创建策略页面, 策略条件频控组件复用,  触点权益池调用后端接口select
### 旧投放计划 
不变

# 某个列表改变字段(询问)
  入口 鹰眼电商运营平台-> 营销 -> 策略营销中心 -> ?

策略条数 改为 策略组

# edit 接口
![[企业微信截图_6ea3b197-a499-4a6d-b5d6-f5333cd5b157.png]]
v vision 版本 0 old    1 new

![[Pasted image 20240530194123.png]]

新增策略![[企业微信截图_b9af4884-8e27-46b2-8001-e5e98b56823b.png]]![[企业微信截图_62038c53-4068-48e2-bfae-cab727bf7f02.png]]![[企业微信截图_5c8c0dab-2c59-4a6c-83a4-971e375e9dd9.png]]![[企业微信截图_12a9667d-8f75-4f82-b030-f61bd9a5ed0c.png]]![[企业微信截图_efae8633-0b82-4e10-b008-ddb407c72983.png]]![[企业微信截图_994a484d-d2bb-4937-a09d-dbdd2c13e07d.png]]


1. 投放计划基础信息, 在策略预览上添加投放计划基础信息模块
2. 投放计划详情-> 策略列表 ![[Pasted image 20240612174818.png]]
3. 优先级管理-> 详情 跳转策略预览页面
4. 新建场景列表页面 可基本复用