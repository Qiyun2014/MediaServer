# SIP通信原理及协议
        SIP协议是一个基于应用层的会话控制协议。它可以创建、修改、终止多媒体会话(会议)，也可以邀请参与者加入到一个现有的会话。
        
## SIP介绍

        SIP(Session Initiation Protocol, 会话发起协议)是用于Voip(Voice over IP，国际协议通化技术)最主要的信令协议之一，承载了SDP(Session Description Protocol, 会话描述协议)内容，SDP协议描述了会话所使用流媒体的细节，如：使用的IP端口，音视频编码等。

        SIP发起的通话呼叫，可以获取到通话的响铃时长、接听时长、挂断时间点、未接听原因、未接通原因、通话时输入的按键等通信元信息。
        
        SIP是一个基于应用层的协议，所以它不是一套完整的通讯系统方案，它需要和其它的方案或者协议结合起来实现整套系统。例如，实时传输协议(RTP)(RFC1889)用来传输音视频等实时的流媒体数据。实时流协议(RTSP)(RFC2326)用来控制媒体流的传递。媒体网关控制协议(MEGACO)(RFC3015)用来控制PSTN网关。
        
## SIP协议栈

| 名称 | 介绍 |
| ------ | ------ |
| [NIST SIP](http://snad.ncsl.nist.gov/proj/iptel/) | Java SIP stack as reference implementation of JAIN API, so it's has good API and documentation. It also has reference implementation for servers and user agent. |
| [minisip](http://www.minisip.org/) | GUI user agent and SIP stack with focus on security, and is portable acro ss Win32, Windows Mobile, Linux, and Linux iPaq. Supports audio and video. [LGPL]. |
| [mjsip](http://www.mjsip.org/) | SIP stack written i n Java, with layered API approach. Applications include se rvers and user agent with audio and video capabilities. [G PL] |
| [opensipstack](http://www.opensipstack.org/) | SIP s tack with media, written in C++ and depends on PWLIB. The website has reference implementation for SBC and also an A ctiveX component for building SIP user agents.Footprint is quite large (megs?), but it has media stack implementatio n (with video?) too. [MPL] |
| [osip2 and exosip2](http://www.antisip.com/) | One o f the earliest open source SIP stack, so it enjoys good nu mber of adopters.Footprint is quite small (several hundred s KBs), and supported ports include Windows, Linux/*nix, V xWorks, MacOS X, and many embedded targets. Media stack is available via commercial license only. [GPL or commercial ]. |
| [PhAPI](http://dev.openwengo.com/trac/openwengo/trac.cgi/wiki/PhApi/) | PhoneAPI from OpenWengo project, writte n in C. It says to provide easy to use high level API for developing SIP audio and video user agents.Ports include Linux, Windows, and MacOS X. Media stack included. (GPL) | 
| [PJSIP](http://www.pjsip.org/) | The SIP (and media) stack features in this website.PJSIP SIP stack is written in C and is mainly targetted for small footprint, feature rich user agents, although it has attracted some server s ide developments too.Footprint is very small (<200KB), per formance is very good (thousands of calls per second), and it is very very portable (Windows, Windows Mobile (WinCE, PDA, SmartPhone), Linux, nix, MacOS X, BSD (FreeBSD), RTE MS, Symbian OS, etc.), which makes it good candidate for e mbedded developments. Complete media framework is available with PJMEDIA. [GPL or other] | 
|[resiprocate](http://www.sipfoundry.org/reSIProcate/) | C++ stack from SIPFoundry non-profit SIP community proje ct. The community is backed by many commercial companies a nd it hosts health number of active developers. (Vocal) |
|[Sofia SIP](http://sofia-sip.sourceforge.net/)| A SIP library written in C for building SIP clients. Supports m any SIP call, presence, instant messaging features, as wel l as TCP, IPv6, TLS, and STUN support.Footprint is quite s mall (several hundreds KBs), and there is user agent refer ence implementation which uses gstreamer for the media. (LGPL) |

## SIP消息

       SIP消息是SIP客户端与服务端之间通信的基本信息单元。SIP消息基于文本，采用UTF-8编码(RFC 2279)中ISO 10646字符集。 SIP协议支持UDP传输协议。
    
       SIP消息类型分为两类，请求消息(Request)和响应(Response)消息。
  
  * 请求消息： 客户端为了激活特定操作向服务端发送的消息，包括INVITE、ACK、OPTIONS、BYE、CANCEL和REGISTER消息。UA(用户代理)到UAS(用户代理服务器)。
  * 响应消息：服务器向客户端反馈对应请求处理结果的SIP消息，包括1xx、2xx、3xx、4xx、5xx、6xx响应信息。
  
  ##### 消息格式和结构
  
   **SIP消息由==三个部分组成==：**
   标识消息类型和目的地址的==起始行==、携带消息类型的==头部==、承载任意附加消息的==消息体==。
 ```
 消息体重传输最重要信息是由SDP(Session Description Protocol)协议描述的媒体控制信息，供终端协商并建立媒体信道。
 ```
  
  **SIP消息格式：**
  一个起始行(==Start-line==)、一个或多个字段(==header fields==)组成的消息头、一个标志消息头结束的空行(==CRLF==)、作为可选项的消息体(==Message body==)组成。

  ```
  起始行分为请求行(Request-Line)和状态行(Status-Line)两种。
  ```
  
  ```
  请求消息的起始行，由请求消息类型，请求目的发送地址Request-URI，SIP协议的版本号，之间用空格隔开。
  请求行的6种Request Method 
  ```
  
  | RequestLine | 介绍 |
  | ------ | ------ |
  | INVITE | 用于发起呼叫请求。INVITE消息包括消息头和数据区两部分。INVITE 消息头包含主、被呼叫的地址，呼叫主题和呼叫优先级等信息。数据区则是关于会话媒体的信息，可由会话描述协议SDP 来实现。|
  | BYE | 当一个用户决定中止会话时，可以使用BYE 来结束会话。 |
  | OPTIONS | 用于询问被叫端的能力信息，但OPTIONS 本身并不能发起呼叫。 |
  | ACK | 对已收到的消息进行确认应答。 |
  | REGISTER | 用于用户向SIP服务器传送位置信息或地址信息。 |
  | CANCEL | 取消当前的请求，但它并不能中止已经建立的连接。 |
  
  ```
     响应消息的起始行，SIP应答消息的Status-Line由SIP-Version开始，接着是一个数字编码的状态码Status-Code，最后是一个与状态码相关的描述性短语Reason-Phrase，然后由一个CRLF行结束符结束Status-Line。
  ```
  
  | Status Line | 介绍 |
  | ------ | ------ |
  | 1xx: 临时消息 | 表示表示请求消息已经收到，后面将继续处理该请求。 |
  | 2xx: 成功消息 | 表示请求已经被成功的理解、接受或执行。|
  | 3xx: 重定向消息 | 表示为了完成请求还需采取更进一步的动作。|
  | 4xx: 客户机错误 | 表示该请求含有语法错误或在这个服务器上不能被满足。|
  | 5xx: 服务器错误 | 表示该服务器不能处理一个明显有效的请求。|
  | 6xx：全局性故障 | 表示该请求在任何服务器上都不能被实现。|
  
  
  ##### 消息头
  
  ```
     消息头的作用是进一步提供消息的扩展信息，方便代理服务器或客户代理服务器更好的对信息进行处理。
  
     消息头分为四类：通用头(general header)、请求头(request header)、响应头(response header)、实体头(entity header)。
  ```
  
  * General header：Call-ID、From、To、Via、Contact、CSeq、Encryption、Expires、Record-Route、Timestamp、Date、Accept、Accept-Encording、Accept-Language。
  * Request header：Subject、User-Agent、Organization、Contact、Authorization、Proxy-Authorization、Proxy-Require、Response-Key、Require、Priority、Hide、Route、Max-Forwards。
  * Response header：Proxy-Authenticate、WWW-Authenticate、Retry-After、Server、Warning、Allow、Unsupported。
  * Entity header：Content-Encoding、Content-Length、Content-Type。
  
  请求应答的过程：
  ![](https://ws1.sinaimg.cn/large/006tNc79gy1g4frek0rn5j30fe0c4gm0.jpg)
  
  INVITE请求报文内容信息如下：
  ![](https://ws1.sinaimg.cn/large/006tNc79gy1g4frhf9bebj30fe06omxh.jpg)
  