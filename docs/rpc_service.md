# RpcService - RPC服务器模块

![](https://static.vnpy.com/upload/temp/3f51a477-36db-41d4-9632-75067ba24be7.png)

RpcService是用于**将VeighNa Trader进程转化为RPC服务器**的功能模块，对外提供交易路由、行情数据推送、持仓资金查询等功能。

关于RPC的具体应用场景请参考本文档结尾的【RPC的应用场景】版块。

&nbsp;

### VeighNa Station加载

启动登录VeighNa Station后，点击【交易】按钮，在配置对话框中的【应用模块】栏勾选【RpcService】。

可通过下面2种模式加载RPC模块：
- 图形模式：登录VN Station，在上层应用界面勾选RpcService，如图。
  
![](https://static.vnpy.com/upload/temp/62edff53-74d0-4cab-9041-cc209d0b394f.png)

&nbsp;

- 脚本模式：使用run.py启动Vn Trader，在导入模块时额外写下面代码：
```
from vnpy.app.rpc_service import RpcServiceApp
from vnpy.gateway.ctp import CtpGateway
main_engine.add_app(RpcServiceApp)
main_engine.add_gateway(CtpGateway)
```

### 启动模块

在启动模块之前，请先连接登录交易接口（连接方法详见基本使用篇的连接接口部分）。看到VeighNa Trader主界面【日志】栏输出“合约信息查询成功”之后再启动模块，如下图所示：  

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/market_radar/1.png) 

成功连接交易接口后，在菜单栏中点击【功能】-> 【RPC服务】，或者点击左侧按钮栏的图标：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/rpc_service/1.png) 

即可进入RPC服务模块的UI界面，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/rpc_service/2.png) 

## 配置和使用

### 配置RPC服务
RPC服务基于ZeroMQ开发，对外的通讯地址包括：

* **请求响应地址**
    * 用于被动接收客户端发送过来的请求，执行对应任务后返回结果；
    * 功能举例：
        * 行情订阅；
        * 委托下单；
        * 委托撤单；
        * 初始化信息查询（合约、持仓、资金等）；
* **事件广播地址**
    * 用于主动推送服务端收到的事件数据，到所有已连接的客户端；
    * 功能举例：
        * 行情推送；
        * 委托推送；
        * 成交推送。

以上地址均采用ZeroMQ的地址格式，由**通讯协议**（如tcp://）和**通讯地址**（如127.0.0.1:2014）两部分组成。

RPC服务支持的通讯协议包括：

* **TCP协议**
    * 协议前缀：tcp://
    * Windows和Linux系统均可使用
    * 可用于本机通讯（127.0.0.1）或者网络通讯（网络IP地址）
* **IPC协议**
    * 协议前缀：ipc://
    * 只能在Linux系统上使用（POSIX本地端口通讯）
    * 只能用于本机通讯，后缀为任意字符串内容

一般推荐直接使用TCP协议（以及默认地址），对于使用Ubuntu系统且希望追求更低通讯延时的用户可以使用IPC协议。

### 运行RPC服务

完成通讯地址的配置后，点击【启动】按钮即可启动RPC服务，日志区域会输出"RPC服务启动成功"，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/rpc_service/3.png) 

启动成功后，即可在另一VeighNa Trader进程中（客户端）使用RpcGateway来连接

如需停止RPC服务可以点击【停止】按钮，此时日志输出"RPC服务已停止"。


### 启动运行
进入Vn Trader，先连接交易接口，如CTP，然后点击菜单栏“功能”->“Rpc服务”，进入如图RPC服务点击“启动”即可。注意：RPC服务不仅支持同一物理机多进程通讯，还支持局域网内部通讯，若在同一台机器上运行，无需修改任何参数。

VeighNa提供了与RpcService配套使用的RpcGateway，作为客户端的标准接口来连接服务端并进行交易，对上层应用透明。

&nbsp;

在客户端加载RpcGateway接口后，进入VeighNa Trader主界面，点击菜单栏中【系统】->【连接RPC】，在弹出的窗口中点击【连接】即可连接使用，如下图所示。

### 加载接口
RPC客户端同样提供2种不同的加载模式：
- 图形模式：登录VN Station，在底层接口界面勾选RPC服务，如图。

![](https://static.vnpy.com/upload/temp/659a156c-2bf2-4053-bd91-2c383aff24b2.png)

&nbsp;

- 脚本模式：使用run.py启动Vn Trader，在导入模块时额外写下面代码：

```
from vnpy.gateway.rpc import RpcGateway
main_engine.add_gateway(RpcGateway)
```

&nbsp;

### 连接使用
从客户端的视角看，RpcGateway就是如CTP一般的接口，省去了额外输入账户等信息的步骤。因为统一在服务端已经完成，只需要和服务器端进行连接即可。

进入VnTrader，点击菜单栏”系统“->”连接RPC“，在如图弹出的窗口中点击”连接“即可。

![](https://static.vnpy.com/upload/temp/988fc191-2762-48cb-b0fb-77384dc543f9.png)

&nbsp;

## 参考样例
参考样例位于examples/server_client目录下，其中包括服务器进程和客户端进程。

### 服务器进程
样例提供了run_server.py文件，里面定义了main_ui和main_terminal函数，分别用于GUI模式和无界面模式启动，可以根据需要修改这两个函数，然后选择运行某一个函数即可。

- GUI模式：GUI模式启动和上面提到的run.py启动完全一致，只是run_server.py已经默认加载了RPC模块，用户只需修改加载的外部交易接口即可。

- 无界面模式：需要提前配置好连接CTP等交易接口所需的个人信息，如图：
  
![](https://static.vnpy.com/upload/temp/69010fa2-98c4-47ae-b055-d6709d744385.png)

&nbsp;

### 客户端进程
样例提供了run_client.py，和上述提到的run.py启动VnTrader方式完全一致，只是这里已经默认加载了Rpc接口。

