# 交易接口

## 如何连接

### VeighNa Station加载

启动登录VeighNa Station后，点击【交易】按钮，在配置对话框中的【交易接口】栏勾选想要交易的接口。

&nbsp;

### 加载需要用的接口

加载接口示例在根目录"tests\trader"文件夹的run.py文件中。
- 从gateway文件夹引入接口类文件，如from vnpy.gateway.ctp import CtpGateway;
- 创建事件引擎对象并且通过add_gateway()函数添加接口程序;
- 创建图形化对象main_window，以VN Trader操作界面展示出来。


```
from vnpy.gateway.ctp import CtpGateway

def main():
    """"""
    qapp = create_qapp()
    main_engine = MainEngine(event_engine)
    main_engine.add_gateway(CtpGateway)
    main_window = MainWindow(main_engine, event_engine)
    main_window.showMaximized()
    qapp.exec()
```

&nbsp;


在图形化操作界面VeighNa Trader上的菜单栏中点击【系统】->【连接CTP】，会弹出账号配置窗口，如下图所示：

打开cmd窗口，使用命令“Python run.py"即可进入VN Trader操作界面。在左上方的菜单栏中点击"系统"->"连接CTP”按钮会弹出账号配置窗口，输入账号、密码等相关信息即连接接口。

连接接口的流程首先是初始化账户信息，然后调用connet()函数来连接交易端口和行情端口。
- 交易端口：查询用户相关信息（如账户资金、持仓、委托记录、成交记录）、查询可交易合约信息、挂撤单操作；
- 行情端口：接收订阅的行情信息推送、接收用户相关信息（如账户资金更新、持仓更新、委托推送、成交推送）更新的回调推送。


&nbsp;

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/gateway/5.png)

### 修改json配置文件

接口配置相关信息保存在json文件中，放置在用户目录下的.vntrader文件夹内，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/gateway/3.png)

如果需要修改接口配置文件，用户既可以在图形化界面VeighNa Trader内修改，也可以直接在.vntrader文件夹下修改对应的json文件。

所以要修改接口配置文件，用户即可以在图形化界面VN Trader内修改，也可以直接在.vntrader修改json文件。
另外将json配置文件分离于vnpy的好处在于：避免每次升级都要重新配置json文件。


&nbsp;


### 查看可交易的合约

先登录接口，然后在菜单栏中点击"帮助"->"查询合约”按钮会空白的“查询合约”窗口。点击“查询”按钮后才会显示查询结果，如图。

![](https://vnpy-community.oss-cn-shanghai.aliyuncs.com/forum_experience/yazhang/gateway/query_contract.png)


## 接口分类

| 接口                 |                    类型                         |
| ---------------------| :--------------------------------------------: |
| CTP                  |           期货、期货期权（实盘6.5.1）            |
| CTP测试              |           期货、期货期权（测试6.5.1）            |
| CTP Mini            |            期货、期货期权（实盘1.4）             |
| 飞马                 |                    期货                         |
| CTP期权              |             ETF期权（实盘20190802）              |
| 顶点飞创             |                    ETF期权                       |
| 顶点HTS              |                    ETF期权                       |
| 恒生UFT              |                期货、ETF期权                     |
| 易盛                 |                期货、黄金TD                      |
| 中泰XTP              |              A股、两融、ETF期权                  |
| 国泰君安统一交易网关   |                    A股                          |
| 华鑫奇点股票          |                    A股                          |
| 华鑫奇点期权          |                  ETF期权                        |
| 中亿汇达Comstar       |                银行间市场                       |
| 东方证券OST           |                    A股                          |
| 盈透证券              |                 海外多品种                      |
| 易盛9.0外盘           |                  外盘期货                       |
| 直达期货              |                  外盘期货                       |
| 融航                 |                  期货资管                        |
| TTS                  |                    期货                         |
| 飞鼠                  |                  黄金TD                         |
| 金仕达黄金            |                  黄金TD                         |


## 接口详解

### CTP

#### 如何加载

run.py文件提供了接口加载示例：先从gateway上调用ctpGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.ctp import CtpGateway
main_engine.add_gateway(CtpGateway)
```

&nbsp;

#### 相关字段

- 用户名：username
- 密码：password：
- 经纪商编号：brokerid
- 交易服务器地址：td_address
- 行情服务器地址：md_address
- 产品名称：app_id
- 授权编码：auth_code
  
&nbsp;

#### 获取账号

- 仿真账号：从SimNow网站上获取。只需输入手机号码和短信验证即可。（短信验证有时只能在工作日正常工作时段收到）SimNow的用户名(InvestorID)为6位纯数字，经纪商编号为9999，并且提供2套环境用于盘中仿真交易以及盘后的测试。
  
- 实盘账号：在期货公司开户，通过联系客户经理可以开通。用户名为纯数字，经纪商编号也是4位纯数字。（每个期货公司的经纪商编号都不同）另外，实盘账号也可以开通仿真交易功能，同样需要联系客户经理。


&nbsp;

### MINI

#### 如何加载

先从gateway上调用MiniGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.mini import MiniGateway
main_engine.add_gateway(MiniGateway)
```

&nbsp;

#### 相关字段

- 用户名：username
- 密码：password：
- 经纪商编号：brokerid
- 交易服务器地址：td_address
- 行情服务器地址：md_address
- 产品名称：product_info
- 授权编码：auth_code
  
&nbsp;

#### 获取账号

在期货公司开户，通过联系客户经理可以开通。用户名为纯数字，经纪商编号也是4位纯数字。（每个期货公司的经纪商编号都不同）另外，实盘账号也可以开通仿真交易功能，同样需要联系客户经理。

### FEMAS（飞马）

&nbsp;



### 中泰柜台(XTP)

#### 如何加载

先从gateway上调用XtpGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.xtp import XtpGateway
main_engine.add_gateway(XtpGateway)
```

&nbsp;

#### 获取账号

在期货公司开户，通过联系客户经理可以开通。用户名为纯数字，经纪商代码也是4位纯数字（每个期货公司的经纪商编号都不同）。另外，实盘账号也可以开通仿真交易功能，同样需要联系客户经理。

### SOPT（CTP期权）

#### 接口支持

- 操作系统
  - Windows
  - Ubuntu

- 交易品种
  - ETF期权

- 持仓方向
  - 只支持双向持仓

- 历史数据
  - 不提供

#### 相关字段

- 用户名：
- 密码：
- 经纪商代码：
- 交易服务器：
- 行情服务器：
- 产品名称：
- 授权编码：

#### 获取账号

在期货公司开户，通过联系客户经理可以开通。用户名为纯数字，经纪商代码也是4位纯数字（每个期货公司的经纪商代码都不同）。另外，实盘账号也可以开通仿真交易功能，同样需要联系客户经理。

### SEC（顶点飞创）

#### 接口支持

- 操作系统
  - Windows

- 交易品种
  - ETF期权

- 持仓方向
  - 股票只支持单向持仓
  - 股票期权只支持双向持仓

- 历史数据
  - 不提供

#### 相关字段

- 账号：
- 密码：
- 客户号": 1
- 行情地址：
- 行情端口": 0
- 交易地址：
- 行情协议：TCP、UDP
- 授权码：
- 产品号：
- 采集类型：顶点、恒生、金证、金仕达
- 行情压缩：N、Y

#### 获取账号

在期货公司开户，通过联系客户经理可以开通。

### HTS（顶点HTS）

#### 接口支持

- 操作系统
  - Windows

- 交易品种
  - ETF期权

- 持仓方向
  - 双向持仓

- 历史数据
  - 不提供

#### 相关字段

- 账号：
- 密码：
- 行情地址：
- 交易地址：
- 行情协议：TCP、UDP
- 授权码：
- 产品号：
- 采集类型：顶点、恒生、金证、金仕达
- 行情压缩：N、Y

#### 获取账号

在期货公司开户，通过联系客户经理可以开通。

### UFT（恒生UFT）

#### 接口支持

- 操作系统
  - Windows
  - Ubuntu

- 交易品种
  - 期货
  - ETF期权

- 持仓方向
  - 只支持双向持仓

- 历史数据
  - 不提供

#### 相关字段

- 用户名：
- 密码：
- 行情服务器：
- 交易服务器：
- 服务器类型：期货、ETF期权
- 产品名称：
- 授权编码：
- 委托类型：q

#### 获取账号

测试账号请通过恒生电子申请。

### ESUNNY（易盛）

#### 接口支持

- 操作系统
  - Windows
  - Ubuntu

- 交易品种
  - 期货
  - 黄金TD

- 持仓方向
  - 支持双向持仓

- 历史数据
  - 不支持

#### 相关字段

- 行情账号：
- 行情密码：
- 行情服务器：
- 行情端口：0
- 行情授权码：
- 交易账号：
- 交易密码：
- 交易服务器：
- 交易端口：0
- 交易产品名称：
- 交易授权编码：
- 交易系统：内盘、外盘

#### 获取账号

测试账号请通过易盛官方网站申请。

### XTP（中泰柜台）

#### 接口支持

- 操作系统
  - Windows
  - Ubuntu

- 交易品种
  - A股
  - 两融
  - ETF期权

- 持仓方向
  - 股票只支持单向持仓
  - 其余标的支持双向持仓

- 历史数据
  - 不提供

#### 相关字段

- 账号：
- 密码：
- 客户号: 1
- 行情地址：
- 行情端口: 0
- 交易地址：
- 交易端口: 0
- 行情协议: TCP、UDP
- 日志级别：FATAL、ERROR、WARNING、INFO、DEBUG、TRACE
- 授权码：

&nbsp;


#### 获取账号

测试账号请联系中泰证券申请。

#### 其他特点

XTP是首家提供融资融券的极速柜台。

### HFT（国泰君安统一交易网关）


### 宽睿柜台(OES)

#### 如何加载

先从gateway上调用OesGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.oes import OesGateway
main_engine.add_gateway(OesGateway)
```

&nbsp;


#### 相关字段

- 用户名：username
- 密码：password
- 硬盘序列号：hdd_serial
- 交易委托服务器：td_ord_server
- 交易回报服务器：td_rpt_server
- 交易查询服务器：td_qry_server
- 行情推送服务器：md_tcp_server
- 行情查询服务器：md_qry_server

&nbsp;


#### 获取账号

测试账号请联系宽睿科技申请

&nbsp;

#### 其他特点

宽睿柜台提供内网UDP低延时组播行情以及实时成交信息推送。

&nbsp;


### 华鑫奇点(TORA)

#### 如何加载

先从gateway上调用ToraGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.tota import ToraGateway
main_engine.add_gateway(ToraGateway)
```

&nbsp;

#### 相关字段

- 账号: username
- 密码: password
- 交易服务器: td_address
- 行情服务器: md_address

&nbsp;

#### 获取账号

测试账号请联系华鑫证券申请


&nbsp;

### 盈透证券(IB)

#### 如何加载

先从gateway上调用IbGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.ib import IbGateway
main_engine.add_gateway(IbGateway)
```

&nbsp;

#### 相关字段

- 账号：
- 密码：
- 行情服务器：
- 交易服务器：
- 账号类型：用户代码、资金账号
- 地址类型：前置地址、FENS地址

#### 获取账号

测试账号请通过华鑫证券申请。

### COMSTAR（中亿汇达）

#### 接口支持

- 操作系统
  - Windows

- 交易品种
  - 银行间市场

- 持仓方向
  - 无

- 历史数据
  - 不提供

#### 相关字段

- 交易服务器：
- 用户名：
- 密码：
- Key：
- routing_type：5
- valid_until_time：18:30:00.000

#### 获取账号

只有各类大型金融机构才能用（券商自营交易部、银行金融市场部等），私募或者个人都用不了。需要购买ComStar的交易接口服务之后才能使用。

### OST（东方证券）

#### 接口支持

- 操作系统
  - Windows

- 交易品种
  - A股

- 持仓方向
  - 单向持仓

- 历史数据
  - 不提供

#### 相关字段

- 用户名：
- 密码：
- 交易服务器：
- 上交所快照地址：
- 上交所快照端口: 0
- 深交所快照地址：
- 深交所快照端口: 0
- 本机ip地址：

#### 获取账号

在证券公司开户，通过联系客户经理可以开通。

### IB（盈透证券）

#### 接口支持

- 操作系统
  - Windows
  - Ubuntu
  - Mac

- 交易品种
  - 海外多品种

- 持仓方向
  - 只支持单向持仓

- 历史数据
  - 提供

#### 相关字段

- TWS地址：127.0.0.1
- TWS端口：7497
- 客户号：1


&nbsp;


#### 获取账号

在盈透证券开户并且入金后可以获得API接入权限。拥有实盘账号后才可以申请开通仿真交易账号。

&nbsp;

#### 其他特点

可交易品种覆盖诸多海外市场的股票、期权、期权；手续费相对较低。

请注意，IB接口的合约代码较为特殊，请前往官网的产品查询板块查询。VeighNa Trader中使用的是盈透证券对于每个合约在某一交易所的唯一标识符ConId来作为合约代码，而非Symbol或者LocalName。

&nbsp;


### 易盛外盘(TAP)

#### 如何加载

先从gateway上调用TapGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.tap import TapGateway
main_engine.add_gateway(TapGateway)
```

&nbsp;


#### 相关字段

- 授权码：auth code
- 行情账号：quote username
- 行情密码：quote password
- 行情地址：123.15.58.21
- 行情端口：7171



&nbsp;


#### 获取账号

在TAP开户并且入金后可以获得API接入权限。

&nbsp;


### 富途证券(FUTU)

#### 如何加载

先从gateway上调用FutuGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.futu import FutuGateway
main_engine.add_gateway(FutuGateway)
```

&nbsp;


#### 相关字段

- 地址：127.0.0.1
- 密码：
- 端口：11111
- 市场：HK 或 US
- 环境：TrdEnv.REAL 或 TrdEnv.SIMULATE


&nbsp;


#### 获取账号

在富途证券开户并且入金后可以获得API接入权限。拥有实盘账号后才可以申请开通仿真交易账号。






&nbsp;

### 老虎证券(TIGER)


#### 如何加载

先从gateway上调用TigerGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.tiger import TigerGateway
main_engine.add_gateway(TigerGateway)
```

&nbsp;


#### 相关字段

- 用户ID：tiger_id
- 环球账户：account
- 标准账户：standard_account
- 秘钥：private_key



&nbsp;


#### 获取账号

在老虎证券开户并且入金后可以获得API接入权限。拥有实盘账号后才可以申请开通仿真交易账号。

请注意，融航接口的【经纪商代码】不再是纯数字形态，而是可以包含英文和数字的字符串；VeighNa连接融航进行交易在穿透式认证中属于【中继】模式，而不再是连接柜台（CTP、恒生等）进行交易时的【直连】模式，所以在申请穿透式认证测试填表时不要选错。

&nbsp;


### ALPACA

#### 如何加载
先从gateway上调用AlpacaGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.alpaca import AlpacaGateway
main_engine.add_gateway(AlpacaGateway)
```

&nbsp;

#### 相关字段
- KEY ID: key
- Secret Key: secret
- 会话数: 10
- 服务器:["REAL", "PAPER"]
#### 获取账号
在OKEX官网开户并且入金后可以获得API接入权限。
#### 其他特点

&nbsp;


### BITMEX

#### 如何加载

先从gateway上调用BitmexGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.bitmex import BitmexGateway
main_engine.add_gateway(BitmexGateway)
```

&nbsp;


#### 相关字段

- 用户ID：ID
- 密码：Secret
- 会话数：3
- 服务器：REAL 或 TESTNET
- 代理地址：
- 代理端口：



&nbsp;


#### 获取账号

在BITMEX官网开户并且入金后可以获得API接入权限。



&nbsp;

### OKEX现货（OKEX）


#### 如何加载

先从gateway上调用OkexGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.okex import OkexGateway
main_engine.add_gateway(OkexGateway)
```

&nbsp;


#### 相关字段

- API秘钥：API Key
- 密码秘钥：Secret Key
- 会话数：3
- 密码：passphrase
- 代理地址：
- 代理端口：



&nbsp;


#### 获取账号

在OKEX官网开户并且入金后可以获得API接入权限。



&nbsp;


### OKEX期货（OKEXF）


#### 如何加载

先从gateway上调用OkexfGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.okexf import OkexfGateway
main_engine.add_gateway(OkexfGateway)
```

&nbsp;


#### 相关字段

- API秘钥：API Key
- 密码秘钥：Secret Key
- 会话数：3
- 密码：passphrase
- 杠杆：Leverage
- 代理地址：
- 代理端口：



&nbsp;


#### 获取账号

在OKEX官网开户并且入金后可以获得API接入权限。


&nbsp;

### 火币(HUOBI)

#### 如何加载

先从gateway上调用HuobiGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.huobi import HuobiGateway
main_engine.add_gateway(HuobiGateway)
```

&nbsp;


#### 相关字段

- API秘钥：API Key
- 密码秘钥：Secret Key
- 会话数：3
- 代理地址：
- 代理端口：



&nbsp;


#### 获取账号

在火币官网开户并且入金后可以获得API接入权限。


&nbsp;



### 火币合约(HUOBIF)

#### 如何加载

先从gateway上调用HuobifGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.huobif import HuobifGateway
main_engine.add_gateway(HuobifGateway)
```

&nbsp;


#### 相关字段

- API秘钥：API Key
- 密码秘钥：Secret Key
- 会话数：3
- 代理地址：
- 代理端口：



&nbsp;


#### 获取账号

在火币官网开户并且入金后可以获得API接入权限。


&nbsp;

### BITFINEX

#### 如何加载

先从gateway上调用BitFinexGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.bitfinex import BitfinexGateway
main_engine.add_gateway(BitfinexGateway)
```

&nbsp;


#### 相关字段

- 用户ID：ID
- 密码：Secret
- 会话数：3
- 代理地址：
- 代理端口：



&nbsp;


#### 获取账号

在BITFINEX官网开户并且入金后可以获得API接入权限。



&nbsp;


### ONETOKEN

#### 如何加载

先从gateway上调用OnetokenGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.onetoken import OnetokenGateway
main_engine.add_gateway(OnetokenGateway)
```

&nbsp;


#### 相关字段

- Key秘钥：OT Key
- 密码秘钥：OT Secret
- 会话数：3
- 交易所：["BINANCE", "BITMEX", "OKEX", "OKEF", "HUOBIP", "HUOBIF"]
- 账号：
- 代理地址：
- 代理端口：



&nbsp;


#### 获取账号

在Onetoken官网开户并且入金后可以获得API接入权限。



&nbsp;

&nbsp;

### BINANCE

#### 如何加载

先从gateway上调用BinanceGateway类；然后通过add_gateway()函数添加到main_engine上。
```
from vnpy.gateway.binance import BinanceGateway
main_engine.add_gateway(BinanceGateway)
```

&nbsp;


#### 相关字段

- Key秘钥
- secret
- session_number(会话数)：3
- proxy_host
- proxy_port

&nbsp;


#### 获取账号

在BINANCE官网开户并且入金后可以获得API接入权限。

&nbsp;


### RPC

#### 如何加载

RPC的加载涉及到服务端和客户端
- 服务端：运行vntrader时加载rpc_service模块
    ```
    from vnpy.app.rpc_service import RpcService
    ```
    启动vntrader后，首先连接外部交易交易如CTP，然后点击菜单栏"功能"->"RPC服务"，点击"启动"
- 客户端：运行vntrader时加载RpcGateway
    ```
    from vnpy.gateway.rpc import RpcGateway
    ```
    启动vntrader后，连接rpc接口即可。

#### 相关字段
在服务端和客户端，使用默认填好的参数即可

#### 获取账号
使用rpc无须额外申请账号，只需要一个外部接口账号

#### 其他特点
rpc服务支持同一外部接口数据在本地多进程分发，比如在服务端连接了ctp接口，订阅了rb1910后，客户端多个进程会自动订阅来自服务端分发的订阅数据# 交易接口
