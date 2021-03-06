# STUN协议
2014-11-19

## 格式
* 消息头 [+ 消息属性]

#### 消息头(20 Bytes)
* 消息类型(2) + 消息长度(2) + 事务ID(16)
* 消息类型(2 Bytes)
	* 0x0001: Binding Request
	* 0x0101: Binding Response
	* 0x0111: Binding Error Response
	* 0x0002: Shared Secret Request
	* 0x0102: Shared Secret Response
	* 0x0112: Shared Secret Error Response
* 消息长度(2 Bytes) － 不包含消息头
* 事务ID(16 Bytes) － 128位的标识符，用于随机请求和响应，请求与其相应的所有响应具有相同的标识符。

#### 消息属性
* [属性类型(2) + 属性长度(2) + 属性值]若干
* 属性类型
	* 0x0001: MAPPED-ADDRESS
		* 表示映射过的IP地址和端口。它包括8位的地址族，16位的端口号及长度固定的IP地址。
	* 0x0002: RESPONSE-ADDRESS
		* 表示响应的目的地址
	* 0x0003: CHANGE-REQUEST
		* 客户使用32位的CHANGE-REQUEST属性来请求服务器使用不同的地址或端口号来发送响应。
	* 0x0004: SOURCE-ADDRESS
		* 出现在捆绑响应中，它表示服务器发送响应的源IP地址和端口。
	* 0x0005: CHANGED-ADDRESS
		* 如果捆绑请求的CHANGE-REQUEST属性中的“改变IP”和“改变端口”标志设置了，则CHANGED-ADDRESS属性表示响应发出的IP地址和端口号。
	* 0x0006: USERNAME
		* 用于消息的完整性检查，用于消息完整性检查中标识共享私密。USERNAME通常出现在共享私密响应中，与PASSWORD一起。当使用消息完整性检查时，可有选择地出现在捆绑请求中。
	* 0x0007: PASSWORD
		* 用在共享私密响应中，与USERNAME一起。PASSWORD的值是变长的，用作共享私密，它的长度必须是4字节的倍数，以保证属性与边界对齐。
	* 0x0008: MESSAGE-INTEGRITY
		* 包含STUN消息的HMAC-SHA1，它可以出现在捆绑请求或捆绑响应中；MESSAGE-INTEGRITY属性必须是任何STUN消息的最后一个属性。它的内容决定了HMAC输入的Key值。
	* 0x0009: ERROR-CODE
		* 出现在捆绑错误响应或共享私密错误响应中。它的响应号数值范围从100到699。
		下面的响应号，与它们缺省的原因语句一起，目前定义如下：
			* 400（错误请求）：请求变形了。客户在修改先前的尝试前不应该重试该请求。
			* 401（未授权）：捆绑请求没有包含MESSAGE-INTERITY属性。
			* 420（未知属性）：服务器不认识请求中的强制属性。
			* 430（过期资格）：捆绑请求没有包含MESSAGE-INTEGRITY属性，但它使用过期
的共享私密。客户应该获得新的共享私密并再次重试。
			* 431（完整性检查失败）：捆绑请求包含MESSAGE-INTEGRITY属性，但HMAC验
证失败。这可能是潜在攻击的表现，或者客户端实现错误
			* 432（丢失用户名）：捆绑请求包含MESSAGE-INTEGRITY属性，但没有
USERNAME属性。完整性检查中两项都必须存在。
			* 433（使用TLS）：共享私密请求已经通过TLS（Transport Layer Security，即安全传输层协议）发送，但没有在TLS上收到。
			* 500（服务器错误）：服务器遇到临时错误，客户应该再次尝试。
			* 600（全局失败）：服务器拒绝完成请求，客户不应该重试。
	* 0x000A: UNKNOWN-ATTRIBUTES
		* 只存在于其ERROR-CODE属性中的响应号为420的捆绑错误响应或共享私密错误响应中。
	* 0x000B: REFLECTED-FROM
		* 只存在于其对应的捆绑请求包含RESPONSE-ADDRESS属性的捆绑响应中。属性包含请求发出的源IP地址，它的目的是提供跟踪能力，这样STUN就不能被用作DOS攻击的反射器。