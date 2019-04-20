# 异步流水线处理



## useage

```
// step 1
// 定义一个全局种子
globleSeed := int32(0)

// step 2
// 通过原子操作，生成一个唯一ID
id := atomic.AddUint32(&globleSeed, 1)

// step 3
/*
* todo 异步执行, 在等待消息回来前，会暂停当前gorotine
* 注意：此流水线只适用于一来一回。如果一次请求，有多个响应，请选择其他方案
* @param id 唯一标识id
* @param timeout 设置超时的时长
* @func() 执行前需要处理的方法
* @func(interface) 异步消息回来之后，需要执行的内容同时传入返回的数据 (判空和类型断言一定要先处理),
* @func() 超时需要处理的方法
*/
go flow.GetAnsyFlowInst().Call(id,
				time.Second*2, // 设置流水线超时时间
				func() {
					// 开始回调方法处理
				}, func(data interface{}) {
					// 返回回调方法处理
				}, func() {
					// 超时回调方法处理
				})

// step 4
/*
* todo 异步执行的消息返回
* @param id 唯一标识id
* @param data 返回需要处理的数据
* @return 是否响应数据成功，对应pip不存在则失败
*/
flow.GetAnsyFlowInst().OnAnsyBack(id, data)
```



