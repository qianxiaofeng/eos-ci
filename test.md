MVP Version 1 check list

findex plugin
1. 智能合约支持
  1. deposit/withdraw
  2. create/delete trade pair
  3. create buy/sell order
  4. trade

2. OME
  1. 自动周期性执行order match
  2. 简单粗暴的撮合算法 (基础功能，要求每单的数量一至， 价格一至时，成交固定数量)
  3. 为data server 提供查询order book的接口

3. DL ( Deal Log)
  1. 向kafka发送成交记录
    1. 数据格式
    2. 能够用命令行查询发送成功

findex data server  
1. Websocket （扮演api gateway的角色）  
	1. 给前端的数据格式, kline.query, kline.subscribe  
	2. mock 数据  
2. Price Engine （生成价格信息，为websocket 提供price数据）  
	1. 打出consume kafka log 日志  
3. Orderbook Engine （调用plugin api，为websoket 提供orderbook数据）  


findex front  
1. Web Sever  
  1. nginx 配置
2. Web UI
	1. kline 图
 		1. kline query request/response
		2. kline subscribe request/response
  2. 调用 deposit/withdraw
  3. 调用 create buy/sell order
  4. 调用 get trade pair
  5. order book （数据来源：调用nodeos api）
    1. 展示订单，只展示价格， 数量
    2. sell order ， buy order 排序需要按照要求
    3. 每秒更新
  6. trade pair 展示
3. Scatter 集成
  1. 单个账号集成
  2. 支持发送挂单等操作


集成测试 userstory：
1. 完整流程体验
  1. 两个用户A， B 分别打开网页 findex.dev.eosio.sg
  2. A , B 使用scatter
  3. A, B 完成deposit操作
  4. A 在界面上操作挂 买单 价格：10， 数量 100
     B 在界面上操作挂 卖单 价格：12， 数量 100
     双方都能看到order book的挂单， 但并不能够成交
  5. B 操作挂 卖单 10， 数量100
     1-2秒后， 撮合完成，买单和卖单消失
  6. A B 可以完成withdraw操作

2. 价格mock data测试
  1. 刷新页面 findex.dev.eosio.sg， kline可以展示，并且持续更新
  2. 切换粒度， kline可以展示，并且也可以持续更新

3. 测试机器人:
  1. FDT / EOS 初始价格 0.1
  2. model-A, 对平台币FDT随机生成买单，挂单数量0.01-10.00随机，挂单价格 =random_price(range(previous_random_price ± 0.001))
  3. model-B, 对平台币FDT随机生成卖单，其他和A一致。
  4. 0.2秒挂一个单，持续性挂单，撮合。看运行效果如何。
