# SDP协议解读
2014-11-14

SDP(Session Description Protocol)，即会话描述协议，为会话通知、会话邀请和其它形式的多媒体会话初始化等目的提供了```多媒体会话描述```，在rfc2327中定义。

* SDP至少需要包含:
	* 会话名称及意图
	* 会话有效时间
	* 媒体流信息（类型、地址、端口、格式等）

## [协议格式](http://www.ietf.org/rfc/rfc2327.txt)

* 协议以key=value形式
<pre>
v=0
o=102 2048 497 IN IP4 192.168.0.104
s=Talk
c=IN IP4 192.168.0.104
b=AS:380
t=0 0
a=rtcp-xr:rcvr-rtt=all:10000 stat-summary=loss,dup,jitt,TTL voip-metrics
m=audio 7076 RTP/AVP 120 111 110 0 8 101
a=rtpmap:120 SILK/16000
a=rtpmap:111 speex/16000
a=fmtp:111 vbr=on
a=rtpmap:110 speex/8000
a=fmtp:110 vbr=on
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-15
</pre>

## keys 包含Session，Time，Media
#### Session description
* v=  (protocol version)
	* 没有次版本
* o=  (owner/creator and session identifier).
	* o=username session-id version network—type address-type address
		* "IN" = Internet
		* "IP4" = ipv4
* s=  (session name)
* i=* (session information)
* u=* (URI of description)
* e=* (email address)
* p=* (phone number)
* c=* (connection information - not required if included in all media)
	* c=network-type address-type connection-address
* b=* (bandwidth information)
	* b=modifier:bandwidth-value(kpb/s)
* One or more time descriptions (see below)
* z=* (time zone adjustments)
* k=* (encryption key)
* a=* (zero or more session attribute lines)
	* a=attribute:value ...
* Zero or more media descriptions (see below)
#### Time description
* t=  (time the session is active)
	* t=start-time stop-time, 若为0 0，则视为永久会话
* r=* (zero or more repeat times)
	* r=repeat-interval active-duration list-of-offsets-from-start-time
#### Media description
* m=  (media name and transport address)
	* m=media port transport fmt-list
	* a=rtpmap:payload-type encoding-name/clock-rate[/encoding-parameters]
* i=* (media title)
* c=* (connection information - optional if included at session-level)
* b=* (bandwidth information)
* k=* (encryption key)
* a=* (zero or more media attribute lines)


