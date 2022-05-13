# 多合约组合策略模块

## 策略开发
多合约组合策略模板提供完整的信号生成和委托管理功能，用户可以基于该模板自行开发策略。新策略可以放在用户运行的文件内（推荐），如在c:\users\administrator.vntrader目录下创建strategies文件夹；可以放在根目录下vnpy\app\portfolio_strategy\strategies文件夹内。
注意：策略文件命名是以下划线模式，如pair_trading_strategy.py；而策略类命名采用的是驼峰式，如PairTradingStrategy。

PortfolioStrategy是用于**多合约组合策略实盘**的功能模块，用户可以通过其UI界面操作来便捷完成策略初始化、策略启动、策略停止、策略参数编辑以及策略移除等任务。

### 参数设置

### VeighNa Station加载

启动登录VeighNa Station后，点击【交易】按钮，在配置对话框中的【应用模块】栏勾选【PortfolioStrategy】。

创建策略实例的方法有效地实现了一个策略跑多个品种，并且其策略参数可以通过品种的特征进行调整。
```


## 启动模块

<span id="jump">

对于用户自行开发的策略，需要放到VeighNa Trader运行时目录下的**strategies**目录中，才能被识别加载。具体的运行时目录路径，可以在VeighNa Trader主界面顶部的标题栏查看。

对于在Windows上默认安装的用户来说，放置策略的strategies目录路径通常为：

```
    C:\Users\Administrator\strategies
```

其中Administrator为当前登录Windows的系统用户名。

</span>

在启动模块之前，请先连接交易接口（连接方法详见基本使用篇的连接接口部分）。看到VeighNa Trader主界面【日志】栏输出“合约信息查询成功”之后再启动模块，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/cta_strategy/1.png)

请注意，IB接口因为登录时无法自动获取所有的合约信息，只有在用户手动订阅行情时才能获取。因此需要在主界面上先行手动订阅合约行情，再启动模块。

成功连接交易接口后，在菜单栏中点击【功能】-> 【组合策略】，或者点击左侧按钮栏的图标：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/0.png)

即可进入多合约组合策略模块的UI界面，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/1.png)

如果配置了数据服务（配置方法详见基本使用篇的全局配置部分），打开多合约组合策略模块时会自动执行数据服务登录初始化。若成功登录，则会输出“数据服务初始化成功”的日志，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/2.png)


## 添加策略

用户可以基于编写好的组合策略模板（类）来创建不同的策略实例（对象）。

在左上角的下拉框中选择要交易的策略名称，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/3.png)

请注意，显示的策略名称是**策略类**（驼峰式命名）的名字，而不是策略文件（下划线模式命名）的名字。

选择好策略类之后，点击【添加策略】，会弹出添加策略对话框，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/4.png)

在创建策略实例时，需要配置相关参数，各参数要求如下：

- 实例名称
  - 实例名称不能重名；
- 合约品种
  - 格式为vt_symbol（合约代码 + 交易所名称）；
  - 一定要是实盘交易系统中可以查到的合约名称；
  - 合约名用“,”隔开，中间不加空格；
- 参数设置
  - 显示的参数名是策略里写在parameters列表中的参数名；
  - 默认数值为策略里的参数的默认值；
  - 由上图可观察到，参数名后面<>括号中显示的是该参数的数据类型，在填写参数时应遵循相应的数据类型。其中，<class 'str'>是字符串、<class 'int'>是整数、<class 'float'>是浮点数；
  - 请注意，如果某个参数可能会调整至有小数位的数值，而默认参数值是整数（比如1）。请在编写策略时，把默认参数值设为浮点数（比如1.0）。否则策略会默认该项参数为整数，在后续【编辑】策略实例参数时，会只允许填进整数。

参数配置完成后，点击【添加】按钮，则开始创建策略实例。创建成功后可在左侧的策略监控组件中看到该策略实例，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/5.png)

策略监控组件顶部显示的是策略实例名、策略类名以及策略作者名（在策略里定义的author）。顶部按钮用于控制和管理策略实例，第一行表格显示了策略内部的参数信息（参数名需要写在策略的parameters列表中图形界面才会显示），第二行表格则显示了策略运行过程中的变量信息（变量名需要写在策略的variables列表中图形界面才会显示）。【inited】字段表示当前策略的初始化状态（是否已经完成了历史数据回放），【trading】字段表示策略当前是否能够开始交易。

从上图可观察到，此时该策略实例的【inited】和【trading】状态都为【False】。说明该策略实例还没有初始化，也还不能发出交易信号。

策略实例创建成功后，该策略实例的配置信息会被保存到.vntrader文件夹下的portfolio_strategy_setting.json文件中。

请注意，如果添加了同名的策略实例，则会创建失败，图形界面输出“创建策略失败，存在重名”的日志信息，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/8.png)


## 管理策略

### 初始化

策略实例创建成功后，就可以对该实例进行初始化了。点击该策略实例下的【初始化】按钮，若初始化成功，则如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/6.png)

初始化完成后，可观察到此时该策略实例的【inited】状态已经为【True】。说明该策略实例已经加载过历史数据并完成初始化了。【trading】状态还是为【False】，说明此时该策略实例还不能开始自动交易。

请注意，与CTA策略不同，如果创建实例时输入错误的vt_symbol，多合约组合策略模块会在初始化时报错，而不是在创建策略实例时报错，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/7.png)

### 启动

策略实例初始化成功，【inited】状态为【True】时，才能启动该策略的自动交易功能。点击该策略实例下的【启动】按钮，即可启动该策略实例。成功后如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/10.png)

可观察到此时该策略实例的【inited】和【trading】状态都为【True】。说明此时该策略实例已经完成了历史数据回放，而且此时策略内部的交易请求类函数（buy/sell/short/cover/cancel_order等），以及信息输出类函数（send_email/put_event等），才会真正执行并发出对应的请求指令到底层接口中（真正执行交易）。

在上一步策略初始化的过程中，尽管策略同样在接收（历史）数据，并调用对应的功能函数，但因为【trading】状态为【False】，所以并不会有任何真正的委托下单操作或者交易相关的日志信息输出。

如果启动之后，策略发出了委托，可以去VeighNa Trader主界面【委托】栏查看委托订单细节，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/9.png)

请注意，与CTA策略模块不同，多合约组合策略**不提供本地停止单功能**，所以UI界面上不会有停止单的显示区域了。


### 停止

如果启动策略之后，由于某些情况（如到了市场收盘时间，或盘中遇到紧急情况）想要停止、编辑或者移除策略，可以点击策略实例下的【停止】按钮，即可停止该策略实例的自动交易。成功后如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/11.png)

组合策略引擎会自动将该策略之前发出的所有活动委托全部撤销，以保证在策略停止后不会有失去控制的委托存在。同时该策略实例最新的变量信息会被保存到.vntrader文件夹下的portfolio_strategy_data.json文件中。

此时可观察到该策略实例的【trading】状态已变为【False】，说明此时该策略实例已经停止自动交易了。

在多合约组合策略的实盘交易过程中，正常情况应该让策略在整个交易时段中都自动运行，尽量不要有额外的暂停重启类操作。对于国内期货市场来说，应该在交易时段开始前，启动策略的自动交易，然后直到收盘后，再关闭自动交易。因为现在CTP夜盘收盘后也会关闭系统，早上开盘前重启，所以夜盘收盘后也需要停止策略，关闭VeighNa Trader了。

### 编辑

如果创建策略实例之后，想要编辑某个策略实例的参数（若已启动策略，需要先点击策略实例下的【停止】按钮，停止策略），可以点击该策略实例下的【编辑】按钮，会弹出参数编辑对话框，以供修改策略参数。如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/12.png)

编辑完策略参数之后，点击下方的【确定】按钮，相应的修改会立即更新在参数表格中。

但是策略实例的交易合约代码无法修改，同时修改完后也不会重新执行初始化操作。也请注意，此时修改的只是.vntrader文件夹下porfolio_strategy_setting.json文件中该策略实例的参数值，并没有修改原策略文件下的参数。

修改前，json文件如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/18.png)


修改后，json文件如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/17.png)

若盘中编辑后想要再次启动策略，点击策略实例下的【启动】按钮即可再次启动该策略实例。

### 移除

如果创建策略实例之后，想要移除某个策略实例（若已启动策略，需要先点击策略实例下的【停止】按钮，停止策略），可以点击该策略实例下的【移除】按钮。移除成功后，图形界面左侧的策略监控组件中将不会再显示该策略实例的信息。如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/13.png)

此时.vntrader文件夹下的portfolio_strategy_setting.json文件也移除了该策略实例的配置信息。

### 状态跟踪

如果想要通过图形界面跟踪策略的状态，有两种方式：

1. 调用put_event函数

   策略实例中所有的的变量信息，都需要把变量名写在策略的variables列表中，才能在图形界面显示。如果想跟踪变量的状态变化，则需要在策略中调用put_event函数，界面上才会进行数据刷新。

   有时用户会发现自己写的策略无论跑多久，变量信息都不发生变化，这种情况请检查策略中是否漏掉了对put_event函数的调用。

2. 调用write_log函数

   如果不仅想观察到变量信息的状态变化，还想根据策略的状态输出基于自己需求的个性化的日志，可以在策略中调用write_log函数，进行日志输出。

## 运行日志

### 日志内容

多合约组合策略模块UI界面上输出的日志有两个来源，分别是策略引擎和策略实例。

**引擎日志**

策略引擎一般输出的是全局信息。下图中除了策略实例名后加冒号的内容之外，都是策略引擎输出的日志。

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/15.png)

**策略日志**

如果在策略中调用了write_log函数，那么日志内容就会通过策略日志输出。下图红框里的内容是策略实例输出的策略日志。冒号前是策略实例的名称，冒号后是write_log函数输出的内容。

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/16.png)

### 清空操作

如果想要清空多合约组合策略UI界面上的日志输出，可以点击右上角的【清空日志】按钮，则可一键清空该界面上已输出的日志。

点击【清空日志】后，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/14.png)

## 批量操作

在策略经过充分测试，实盘运行较为稳定，不需要经常进行调整的情况下，如果有多个需要运行的组合策略实例，可以使用界面右上角的【全部初始化】、【全部启动】和【全部停止】功能来执行盘前批量初始化、启动策略实例以及盘后批量停止策略实例的操作。

### 全部初始化

在所有策略实例创建成功后，点击右上角的【全部初始化】按钮，则可批量初始化策略实例，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/19.png)

### 全部启动

在所有策略实例初始化成功后，点击右上角的【全部启动】按钮，则可批量启动策略实例，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/20.png)

### 全部停止

在所有策略实例启动成功后，点击右上角的【全部停止】按钮，则可批量停止策略实例，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/portfolio_strategy/21.png)

## 多合约组合策略模板（StrategyTemplate）

多合约组合策略模板提供了信号生成和委托管理功能，用户可以基于该模板(位于site-packages\vnpy_portfoliostrategy\template中)自行开发多合约组合策略。

用户自行开发的策略可以放在用户运行文件夹下的[strategies](#jump)文件夹内。

请注意：

1. 策略文件命名采用下划线模式，如portfolio_boll_channel_strategy.py，而策略类命名采用驼峰式，如PortfolioBollChannelStrategy。

2. 自建策略的类名不要与示例策略的类名重合。如果重合了，图形界面上只会显示一个策略类名。

下面通过PortfolioBollChannelStrategy策略示例，来展示策略开发的具体步骤：

在基于策略模板编写策略逻辑之前，需要在策略文件的顶部载入需要用到的内部组件，如下方代码所示：

```python 3
from typing import List, Dict
from datetime import datetime

from vnpy.trader.utility import ArrayManager, Interval
from vnpy.trader.object import TickData, BarData
from vnpy_portfoliostrategy import StrategyTemplate, StrategyEngine
from vnpy_portfoliostrategy.utility import PortfolioBarGenerator
```

其中，StrategyTemplate是策略模板，StrategyEngine是策略引擎，Interval是数据频率，TickData和BarData都是储存对应信息的数据容器，PortfolioBarGenerator是组合策略K线生成模块，ArrayManager是K线时间序列管理模块。

### 策略参数与变量

在策略类的下方，可以设置策略的作者（author），参数（parameters）以及变量（variables），如下方代码所示：

```python 3

    author = "用Python的交易员"

    boll_window = 18
    boll_dev = 3.4
    cci_window = 10
    atr_window = 30
    sl_multiplier = 5.2
    fixed_size = 1
    price_add = 5
    boll_window = 20
    boll_dev = 2
    fixed_size = 1
    leg1_ratio = 1
    leg2_ratio = 1

    leg1_symbol = ""
    leg2_symbol = ""
    current_spread = 0.0
    boll_mid = 0.0
    boll_down = 0.0
    boll_up = 0.0
```

### 类的初始化
初始化：

- 通过super( )的方法继承StrategyTemplate，在__init__( )函数传入Strategy引擎、策略名称、vt_symbols、参数设置。
- 调用K线生成模块:通过时间切片来把Tick数据合成1分钟K线数据。
- 如果需要，可以调用K线时间序列管理模块：基于K线数据，来生成相应的技术指标。
  
```
    def __init__(
        self,
        strategy_engine: StrategyEngine,
        strategy_name: str,
        vt_symbols: List[str],
        setting: dict
    ):
        """"""
        super().__init__(strategy_engine, strategy_name, vt_symbols, setting)

        self.bgs: Dict[str, BarGenerator] = {}
        self.targets: Dict[str, int] = {}
        self.last_tick_time: datetime = None

        self.spread_count: int = 0
        self.spread_data: np.array = np.zeros(100)

        # Obtain contract info
        self.leg1_symbol, self.leg2_symbol = vt_symbols

        def on_bar(bar: BarData):
            """"""
            pass

        for vt_symbol in self.vt_symbols:
            self.targets[vt_symbol] = 0
            self.bgs[vt_symbol] = BarGenerator(on_bar)
```

### 策略的初始化、启动、停止
通过“多合约组合策略”组件的相关功能按钮实现。

2 . 创建策略所需的存放不同合约K线时间序列管理实例（ArrayManager）和策略变量的字典。

3 . 分别为策略交易的不同合约创建ArrayManager和目标仓位变量并放进字典里。

ArrayManager的默认长度为100，如需调整ArrayManager的长度，可传入size参数进行调整（size不能小于计算指标的周期长度）。

4 . 调用组合策略K线生成模块（PortfolioBarGenerator）：通过时间切片将Tick数据合成1分钟K线数据。如有需求，还可合成更长的时间周期数据，如15分钟K线。

如果只基于on_bar进行交易，这里代码可以写成：

```python 3
        self.pbg = PortfolioBarGenerator(self.on_bars)
```

而不用给pbg实例传入需要基于on_bars周期合成的更长K线周期，以及接收更长K线周期的函数名。

请注意：

 - 合成X分钟线时，X必须设为能被60整除的数（60除外）。对于小时线的合成没有这个限制。

 - PortfolioBarGenerator默认的基于on_bar函数合成长周期K线的数据频率是分钟级别，如果需要基于合成的小时线或者更长周期的K线交易，请在策略文件顶部导入Interval，并传入对应的数据频率给bg实例。

 - **self.on_hour_bars函数名在程序内部已使用**，如需合成1小时K线，请使用self.on_1_hour_bars或者其他命名。

### 策略引擎调用的函数

StrategyTemplate中的update_setting函数和该函数后面四个以get开头的函数以及update_trade和update_order函数，都是策略引擎去负责调用的函数，一般在策略编写的时候是不需要调用的。

### 策略的回调函数

StrategyTemplate中以on开头的函数称为回调函数，在编写策略的过程中能够用来接收数据或者接收状态更新。回调函数的作用是当某一个事件发生的时候，策略里的这类函数会被策略引擎自动调用（无需在策略中主动操作）。回调函数按其功能可分为以下两类：

#### 策略实例状态控制（所有策略都需要）

**on_init**

* 入参：无

* 出参：无

初始化策略时on_init函数会被调用，默认写法是先调用write_log函数输出“策略初始化”日志，再调用load_bars函数加载历史数据。如下方代码所示：

```python 3
    def on_init(self):
        """
        Callback when strategy is inited.
        """
        self.write_log("策略初始化")

        self.load_bars(1)

    def on_start(self):
        """
        Callback when strategy is started.
        """
        self.write_log("策略启动")

    def on_stop(self):
        """
        Callback when strategy is stopped.
        """
        self.write_log("策略停止")
```
### Tick数据回报
策略订阅某品种合约行情，交易所会推送Tick数据到该策略上。

由于是基于1分钟K线来生成交易信号的，故收到Tick数据后，需要用到K线生成模块里面的update_tick函数，通过时间切片的方法，聚合成1分钟K线数据，并且推送到on_bars函数。

#### 接收数据、计算指标、发出交易信号

**on_tick**

* 入参：tick: TickData

* 出参：无

绝大部分交易系统都只提供Tick数据的推送。即使一部分平台可以提供K线数据的推送，但是这些数据到达本地电脑的速度也会慢于Tick数据的推送，因为也需要平台合成之后才能推送过来。所以实盘的时候，VeighNa里所有的策略的K线都是由收到的Tick数据合成的。

当策略收到实盘中最新的Tick数据的行情推送时，on_tick函数会被调用。默认写法是通过PortfolioBarGenerator的update_tick函数把收到的Tick数据推进前面创建的pbg实例中以便合成1分钟的K线，如下方代码所示：

```python 3
    def on_tick(self, tick: TickData):
        """
        Callback of new tick data update.
        """
        if (
            self.last_tick_time
            and self.last_tick_time.minute != tick.datetime.minute
        ):
            bars = {}
            for vt_symbol, bg in self.bgs.items():
                bars[vt_symbol] = bg.generate()
            self.on_bars(bars)

        bg: BarGenerator = self.bgs[tick.vt_symbol]
        bg.update_tick(tick)

        self.last_tick_time = tick.datetime
```

请注意，on_tick函数只在实盘中会被调用，回测不支持。

在回调函数on_bars中，同时收到所有合约的K线行情推送，并实现核心交易逻辑。调用buy/sell/short/cover/cancel_order等函数来发送交易请求。

信号的生成，由3部分组成：

- 清空未成交委托：为了防止之前下的单子在上一个5分钟没有成交，但是下一个5分钟可能已经调整了价格，就用cancel_all()方法立刻撤销之前未成交的所有委托，保证策略在当前这5分钟开始时的整个状态是清晰和唯一的。
- 时间过滤：这里有用(bar.datetime.minute+1) % 5的逻辑来达到每5分钟推送一次的逻辑。
- 指标计算：基于最新的5分钟K线数据来计算相应计算指标，如价差的布林带通道上下轨。
- 信号计算：通过持仓的判断以及结合布林带通道上下轨，在通道突破点以及离场点设置目标仓位。
- 仓位管理：基于目标仓位和现在持仓的差别来计算本次下单的数量并挂出委托（buy/sell)，同时设置离场点(short/cover)。

```
    def on_bars(self, bars: Dict[str, BarData]):
        """"""
        self.cancel_all()

        # Return if one leg data is missing
        if self.leg1_symbol not in bars or self.leg2_symbol not in bars:
            return

        # Calculate current spread
        leg1_bar = bars[self.leg1_symbol]
        leg2_bar = bars[self.leg2_symbol]

        # Filter time only run every 5 minutes
        if (leg1_bar.datetime.minute + 1) % 5:
            return

        self.current_spread = (
            leg1_bar.close_price * self.leg1_ratio - leg2_bar.close_price * self.leg2_ratio
        )

        # Update to spread array
        self.spread_data[:-1] = self.spread_data[1:]
        self.spread_data[-1] = self.current_spread

        self.spread_count += 1
        if self.spread_count <= self.boll_window:
            return

        # Calculate boll value
        buf: np.array = self.spread_data[-self.boll_window:]

        std = buf.std()
        self.boll_mid = buf.mean()
        self.boll_up = self.boll_mid + self.boll_dev * std
        self.boll_down = self.boll_mid - self.boll_dev * std

        # Calculate new target position
        leg1_pos = self.get_pos(self.leg1_symbol)

        if not leg1_pos:
            if self.current_spread >= self.boll_up:
                self.targets[self.leg1_symbol] = -1
                self.targets[self.leg2_symbol] = 1
            elif self.current_spread <= self.boll_down:
                self.targets[self.leg1_symbol] = 1
                self.targets[self.leg2_symbol] = -1
        elif leg1_pos > 0:
            if self.current_spread >= self.boll_mid:
                self.targets[self.leg1_symbol] = 0
                self.targets[self.leg2_symbol] = 0
        else:
            if self.current_spread <= self.boll_mid:
                self.targets[self.leg1_symbol] = 0
                self.targets[self.leg2_symbol] = 0

        # Execute orders
        for vt_symbol in self.vt_symbols:
            target_pos = self.targets[vt_symbol]
            current_pos = self.get_pos(vt_symbol)

            pos_diff = target_pos - current_pos
            volume = abs(pos_diff)
            bar = bars[vt_symbol]

            if pos_diff > 0:
                price = bar.close_price + self.price_add

                if current_pos < 0:
                    self.cover(vt_symbol, price, volume)
                else:
                    self.buy(vt_symbol, price, volume)
            elif pos_diff < 0:
                price = bar.close_price - self.price_add

                if current_pos > 0:
                    self.sell(vt_symbol, price, volume)
                else:
                    self.short(vt_symbol, price, volume)

        self.put_event()

```

 

### 委托回报、成交回报、停止单回报

在于组合策略中需要对多合约同时下单交易，在回测时**无法判断某一段K线内部每个合约委托成交的先后时间顺序**，因此无法提供on_order和on_trade获取委托成交推送，而只能在每次on_bars回调时通过get_pos和get_order来进行相关的状态查询。

同时组合策略模块只支持限价单交易，不提供停止单功能（StopOrder）。

    2. 如果需要在图形界面刷新指标数值，请不要忘记调用put_event()函数。

#### 委托状态更新

因为组合策略中需要对多合约同时下单交易，在回测时无法判断某一段K线内部每个合约委托成交的先后时间顺序，因此无法提供on_order和on_trade函数来获取委托成交推送，而只能在每次on_bars回调时通过get_pos和get_order来进行相关的状态查询。

### 主动函数

**buy**：买入开仓（Direction：LONG，Offset：OPEN）

**sell**：卖出平仓（Direction：SHORT，Offset：CLOSE）

**short**：卖出开仓（Direction：SHORT，Offset：OPEN）

**cover**：买入平仓（Direction：LONG，Offset：CLOSE）

* 入参：vt_symbol: str, price: float, volume: float, lock: bool = False, net: bool = False

* 出参：vt_orderids: List[str] / 无 

buy/sell/short/cover都是策略内部的负责发单的交易请求类函数。策略可以通过这些函数给策略引擎发送交易信号来达到下单的目的。

以下方buy函数的代码为例，可以看到，**具体要交易合约的代码**，价格和数量是必填的参数，锁仓转换和净仓转换则默认为False。也可以看到，函数内部收到传进来的参数之后就调用了StrategyTemplate里的send_order函数来发单（因为是buy指令，则自动把方向填成了LONG，开平填成了OPEN）。

与CTA策略模块不同，组合策略模块不提供本地停止单功能，所以委托函数中移除了stop参数。

如果lock设置为True，那么该笔订单则会进行锁仓委托转换（在有今仓的情况下，如果想平仓，则会先平掉所有的昨仓，然后剩下的部分都进行反向开仓来代替平今仓，以避免平今的手续费惩罚）。

如果net设置为True，那么该笔订单则会进行净仓委托转换（基于整体账户的所有仓位，根据净仓持有方式来对策略下单的开平方向进行转换）。但是净仓交易模式与锁仓交易模式互斥，因此net设置为True时，lock必须设置为False。

请注意，如果向上期所发出平仓委托，因为该交易所必须指定平今、平昨，底层会对其平仓指令自动进行转换。因为上期所部分品种有平今优惠，所以默认是以平今优先的方式发出委托的（如果交易的标的在上期所平昨更优惠的话，可以自行在vnpy.trader.converter的convert_order_request_shfe函数中做适当的修改）。

```python 3
    def buy(self, vt_symbol: str, price: float, volume: float, lock: bool = False, net: bool = False) -> List[str]:
        """
        Send a new order.
        """
        if self.trading:
            vt_orderids = self.strategy_engine.send_order(
                self, vt_symbol, direction, offset, price, volume, lock
            )
            return vt_orderids
        else:
            return []
```


&nbsp;

## 回测研究
backtesting.py定义了回测引擎，下面主要介绍相关功能函数，以及回测引擎应用示例：

### 加载策略

把组合策略逻辑，对应所有合约品种，以及参数设置（可在策略文件外修改）载入到回测引擎中。

实盘的时候，收到传进来的参数后，会调用round_to函数基于合约的pricetick和min_volume对委托的价格和数量进行处理。

负责载入对应品种的历史数据，大概有4个步骤：

- 检查起始日期是否小于结束日期；
- 清除之前载入的历史数据；
- 有条件地从数据库中选取数据，每次载入30天数据。其筛选标准包括：vt_symbol、 回测开始日期、回测结束日期、K线周期；
- 载入数据是以迭代方式进行的，数据最终存入self.history_data。

```
    def load_data(self) -> None:
        """"""
        self.output("开始加载历史数据")

        if not self.end:
            self.end = datetime.now()

        if self.start >= self.end:
            self.output("起始日期必须小于结束日期")
            return

        # Clear previously loaded history data
        self.history_data.clear()
        self.dts.clear()

        # Load 30 days of data each time and allow for progress update
        progress_delta = timedelta(days=30)
        total_delta = self.end - self.start
        interval_delta = INTERVAL_DELTA_MAP[self.interval]

        for vt_symbol in self.vt_symbols:
            start = self.start
            end = self.start + progress_delta
            progress = 0

            data_count = 0
            while start < self.end:
                end = min(end, self.end)  # Make sure end time stays within set range

                data = load_bar_data(
                    vt_symbol,
                    self.interval,
                    start,
                    end
                )

                for bar in data:
                    self.dts.add(bar.datetime)
                    self.history_data[(bar.datetime, vt_symbol)] = bar
                    data_count += 1

                progress += progress_delta / total_delta
                progress = min(progress, 1)
                progress_bar = "#" * int(progress * 10)
                self.output(f"{vt_symbol}加载进度：{progress_bar} [{progress:.0%}]")

                start = end + interval_delta
                end += (progress_delta + interval_delta)

            self.output(f"{vt_symbol}历史数据加载完成，数据量：{data_count}")

        self.output("所有历史数据加载完成")
```
&nbsp;

### 撮合成交

载入组合策略以及相关历史数据后，策略会根据最新的数据来计算相关指标。若符合条件会生成交易信号，发出具体委托（buy/sell/short/cover），并且在下一根K线成交。

回测引擎提供限价单撮合成交机制来尽量模仿真实交易环节：

- 限价单撮合成交：（以买入方向为例）先确定是否发生成交，成交标准为委托价>= 下一根K线的最低价；然后确定成交价格，成交价格为委托价与下一根K线开盘价的最小值。


&nbsp;

下面展示在引擎中限价单撮合成交的流程：
- 确定会撮合成交的价格；
- 遍历限价单字典中的所有限价单，推送委托进入未成交队列的更新状态；
- 判断成交状态，若出现成交，推送成交数据和委托数据；
- 从字典中删除已成交的限价单。

在策略里调用get_all_active_orderids函数，可以获取当前全部活动委托号。

**get_pricetick**

* 入参：vt_symbol

* 出参：pricetick: float / None

在策略里调用get_price函数，可以获取特定合约的最小价格跳动。

**write_log**

* 入参：msg: str

* 出参：无

在策略中调用write_log函数，可以进行指定内容的日志输出。

**load_bars**

* 入参：days: int, interval: Interval = Interval.MINUTE

* 出参：无

在策略中调用load_bars函数，可以在策略初始化时加载K线数据。

如下方代码所示，load_bars函数调用时，默认加载的天数是10，频率是一分钟，对应也就是加载10天的1分钟K线数据。在回测时，10天指的是10个交易日，而在实盘时，10天则是指的是自然日，因此建议加载的天数宁可多一些也不要太少。加载时会先依次尝试通过交易接口、数据服务、数据库获取历史数据，直到获取历史数据或返回空。

```python 3
    def load_bars(self, days: int, interval: Interval = Interval.MINUTE) -> None:
        """
        Cross limit order with last bar/tick data.
        """
        for order in list(self.active_limit_orders.values()):
            bar = self.bars[order.vt_symbol]

            long_cross_price = bar.low_price
            short_cross_price = bar.high_price
            long_best_price = bar.open_price
            short_best_price = bar.open_price

            # Push order update with status "not traded" (pending).
            if order.status == Status.SUBMITTING:
                order.status = Status.NOTTRADED
                self.strategy.update_order(order)

            # Check whether limit orders can be filled.
            long_cross = (
                order.direction == Direction.LONG
                and order.price >= long_cross_price
                and long_cross_price > 0
            )

            short_cross = (
                order.direction == Direction.SHORT
                and order.price <= short_cross_price
                and short_cross_price > 0
            )

            if not long_cross and not short_cross:
                continue

            # Push order update with status "all traded" (filled).
            order.traded = order.volume
            order.status = Status.ALLTRADED
            self.strategy.update_order(order)

            self.active_limit_orders.pop(order.vt_orderid)

            # Push trade update
            self.trade_count += 1

            if long_cross:
                trade_price = min(order.price, long_best_price)
            else:
                trade_price = max(order.price, short_best_price)

            trade = TradeData(
                symbol=order.symbol,
                exchange=order.exchange,
                orderid=order.orderid,
                tradeid=str(self.trade_count),
                direction=order.direction,
                offset=order.offset,
                price=trade_price,
                volume=order.volume,
                datetime=self.datetime,
                gateway_name=self.gateway_name,
            )

            self.strategy.update_trade(trade)
```

&nbsp;

### 计算策略盈亏情况

基于收盘价、当日持仓量、合约规模、滑点、手续费率等计算总盈亏与净盈亏，并且其计算结果以DataFrame格式输出，完成基于逐日盯市盈亏统计。

下面展示盈亏情况的计算过程

- 浮动盈亏 = 持仓量 \*（当日收盘价 - 昨日收盘价）\*  合约规模
- 实际盈亏 = 持仓变化量  \* （当时收盘价 - 开仓成交价）\* 合约规模
- 总盈亏 = 浮动盈亏 + 实际盈亏
- 净盈亏 = 总盈亏 - 总手续费 - 总滑点

```
    def calculate_pnl(
        self,
        pre_close: float,
        start_pos: float,
        size: int,
        rate: float,
        slippage: float
    ) -> None:
        """"""
        # If no pre_close provided on the first day,
        # use value 1 to avoid zero division error
        if pre_close:
            self.pre_close = pre_close
        else:
            self.pre_close = 1

        # Holding pnl is the pnl from holding position at day start
        self.start_pos = start_pos
        self.end_pos = start_pos

        self.holding_pnl = self.start_pos * (self.close_price - self.pre_close) * size

        # Trading pnl is the pnl from new trade during the day
        self.trade_count = len(self.trades)

        for trade in self.trades:
            if trade.direction == Direction.LONG:
                pos_change = trade.volume
            else:
                pos_change = -trade.volume

            self.end_pos += pos_change

            turnover = trade.volume * size * trade.price

            self.trading_pnl += pos_change * (self.close_price - trade.price) * size
            self.slippage += trade.volume * size * slippage
            self.turnover += turnover
            self.commission += turnover * rate

        # Net pnl takes account of commission and slippage cost
        self.total_pnl = self.trading_pnl + self.holding_pnl
        self.net_pnl = self.total_pnl - self.commission - self.slippage
```
&nbsp;



### 计算策略统计指标
calculate_statistics函数是基于逐日盯市盈亏情况（DateFrame格式）来计算衍生指标，如最大回撤、年化收益、盈亏比、夏普比率等。

```
            df["balance"] = df["net_pnl"].cumsum() + self.capital
            df["return"] = np.log(df["balance"] / df["balance"].shift(1)).fillna(0)
            df["highlevel"] = (
                df["balance"].rolling(
                    min_periods=1, window=len(df), center=False).max()
            )
            df["drawdown"] = df["balance"] - df["highlevel"]
            df["ddpercent"] = df["drawdown"] / df["highlevel"] * 100

            # Calculate statistics value
            start_date = df.index[0]
            end_date = df.index[-1]

            total_days = len(df)
            profit_days = len(df[df["net_pnl"] > 0])
            loss_days = len(df[df["net_pnl"] < 0])

            end_balance = df["balance"].iloc[-1]
            max_drawdown = df["drawdown"].min()
            max_ddpercent = df["ddpercent"].min()
            max_drawdown_end = df["drawdown"].idxmin()

            if isinstance(max_drawdown_end, date):
                max_drawdown_start = df["balance"][:max_drawdown_end].idxmax()
                max_drawdown_duration = (max_drawdown_end - max_drawdown_start).days
            else:
                max_drawdown_duration = 0

            total_net_pnl = df["net_pnl"].sum()
            daily_net_pnl = total_net_pnl / total_days

            total_commission = df["commission"].sum()
            daily_commission = total_commission / total_days

            total_slippage = df["slippage"].sum()
            daily_slippage = total_slippage / total_days

            total_turnover = df["turnover"].sum()
            daily_turnover = total_turnover / total_days

            total_trade_count = df["trade_count"].sum()
            daily_trade_count = total_trade_count / total_days

            total_return = (end_balance / self.capital - 1) * 100
            annual_return = total_return / total_days * 240
            daily_return = df["return"].mean() * 100
            return_std = df["return"].std() * 100

            pnl_std = df["net_pnl"].std()

            if return_std:
                # sharpe_ratio = daily_return / return_std * np.sqrt(240)
                sharpe_ratio = daily_net_pnl / pnl_std * np.sqrt(240)
            else:
                sharpe_ratio = 0

            return_drawdown_ratio = -total_net_pnl / max_drawdown
```
&nbsp;

### 统计指标绘图
通过matplotlib绘制4幅图：
- 资金曲线图
- 资金回撤图
- 每日盈亏图
- 每日盈亏分布图

```
    def show_chart(self, df: DataFrame = None) -> None:
        """"""
        # Check DataFrame input exterior
        if df is None:
            df = self.daily_df

        # Check for init DataFrame
        if df is None:
            return

        fig = make_subplots(
            rows=4,
            cols=1,
            subplot_titles=["Balance", "Drawdown", "Daily Pnl", "Pnl Distribution"],
            vertical_spacing=0.06
        )

        balance_line = go.Scatter(
            x=df.index,
            y=df["balance"],
            mode="lines",
            name="Balance"
        )
        drawdown_scatter = go.Scatter(
            x=df.index,
            y=df["drawdown"],
            fillcolor="red",
            fill='tozeroy',
            mode="lines",
            name="Drawdown"
        )
        pnl_bar = go.Bar(y=df["net_pnl"], name="Daily Pnl")
        pnl_histogram = go.Histogram(x=df["net_pnl"], nbinsx=100, name="Days")

        fig.add_trace(balance_line, row=1, col=1)
        fig.add_trace(drawdown_scatter, row=2, col=1)
        fig.add_trace(pnl_bar, row=3, col=1)
        fig.add_trace(pnl_histogram, row=4, col=1)

        fig.update_layout(height=1000, width=1000)
        fig.show()    def show_chart(self, df: DataFrame = None):
        """"""
        if not df:
            df = self.daily_df
        
        if df is None:
            return

        plt.figure(figsize=(10, 16))

        balance_plot = plt.subplot(4, 1, 1)
        balance_plot.set_title("Balance")
        df["balance"].plot(legend=True)

        drawdown_plot = plt.subplot(4, 1, 2)
        drawdown_plot.set_title("Drawdown")
        drawdown_plot.fill_between(range(len(df)), df["drawdown"].values)

        pnl_plot = plt.subplot(4, 1, 3)
        pnl_plot.set_title("Daily Pnl")
        df["net_pnl"].plot(kind="bar", legend=False, grid=False, xticks=[])

        distribution_plot = plt.subplot(4, 1, 4)
        distribution_plot.set_title("Daily Pnl Distribution")
        df["net_pnl"].hist(bins=50)

        plt.show()
```

&nbsp;

效果如下：

![回测结果](https://user-images.githubusercontent.com/11263900/93662305-b8737e00-fa91-11ea-8c83-70ca30e58dc4.png)

![回测图](https://user-images.githubusercontent.com/11263900/93662331-ec4ea380-fa91-11ea-9e93-d61f90d0d282.png)

&nbsp;

### 策略回测示例

- 导入回测引擎和组合策略
- 设置回测相关参数，如：品种、K线周期、回测开始和结束日期、手续费、滑点、合约规模、起始资金
- 载入策略和数据到引擎中，运行回测。
- 计算基于逐日统计盈利情况，计算统计指标，统计指标绘图。


```
from datetime import datetime
from importlib import reload

import vnpy.app.portfolio_strategy
reload(vnpy.app.portfolio_strategy)

from vnpy.app.portfolio_strategy import BacktestingEngine
from vnpy.trader.constant import Interval

import vnpy.app.portfolio_strategy.strategies.pair_trading_strategy as stg
reload(stg)
from vnpy.app.portfolio_strategy.strategies.pair_trading_strategy import PairTradingStrategy

engine = BacktestingEngine()
engine.set_parameters(
    vt_symbols=["y888.DCE", "p888.DCE"],
    interval=Interval.MINUTE,
    start=datetime(2019, 1, 1),
    end=datetime(2020, 4, 30),
    rates={
        "y888.DCE": 0/10000,
        "p888.DCE": 0/10000
    },
    slippages={
        "y888.DCE": 0,
        "p888.DCE": 0
    },
    sizes={
        "y888.DCE": 10,
        "p888.DCE": 10
    },
    priceticks={
        "y888.DCE": 1,
        "p888.DCE": 1
    },
    capital=1_000_000,
)

setting = {
    "boll_window": 20,
    "boll_dev": 1,
}
engine.add_strategy(PairTradingStrategy, setting)

engine.load_data()
engine.run_backtesting()
df = engine.calculate_result()
engine.calculate_statistics()
engine.show_chart()
```

&nbsp;

## 实盘运行
在实盘环境，用户可以基于编写好的组合策略来创建新的实例，一键初始化、启动、停止策略（不要忘记先连接对应接口，获取行情）。


### 创建策略实例
用户可以基于编写好的组合策略来创建新的实例，策略实例的好处在于同一个策略可以同时去运行多个品种合约，并且每个实例的参数可以是不同的。
在创建实例的时候需要填写如图的实例名称、合约品种、参数设置。注意：实例名称不能重名；合约名称是vt_symbol的格式，如y2101.DCE,p2101.DCE。

![创建](https://user-images.githubusercontent.com/11263900/93662304-b6a9ba80-fa91-11ea-895a-ae56fc7ddf60.png)

创建策略流程如下：
- 检查策略实例重名
- 检查是否存在策略类
- 添加策略配置信息(strategy_name, vt_symbols, setting)到strategies字典上
- 添加该策略要订阅行情的所有合约信息到symbol_strategy_map字典中；
- 把策略配置信息保存到json文件内；
- 在图形化界面更新状态信息。

```
    def add_strategy(
        self, class_name: str, strategy_name: str, vt_symbols: str, setting: dict
    ):
        """
        Add a new strategy.
        """
        if strategy_name in self.strategies:
            self.write_log(f"创建策略失败，存在重名{strategy_name}")
            return

        strategy_class = self.classes.get(class_name, None)
        if not strategy_class:
            self.write_log(f"创建策略失败，找不到策略类{class_name}")
            return

        strategy = strategy_class(self, strategy_name, vt_symbols, setting)
        self.strategies[strategy_name] = strategy

        # Add vt_symbol to strategy map.
        for vt_symbol in vt_symbols:
            strategies = self.symbol_strategy_map[vt_symbol]
            strategies.append(strategy)

        self.save_strategy_setting()
        self.put_strategy_event(strategy)
```

![初始化](https://user-images.githubusercontent.com/11263900/93662303-b27d9d00-fa91-11ea-824f-58645bd4e202.png)

&nbsp;

### 初始化策略
- 调用策略类的on_init()回调函数,并且载入历史数据；
- 恢复上次退出之前的策略状态；
- 从.vntrader/portfolio_strategy_data.json内读取策略参数，最新的技术指标，以及持仓数量；
- 调用接口的subcribe()函数订阅指定行情信息；
- 策略初始化状态变成True，并且更新到日志上。
  
```
    def _init_strategy(self, strategy_name: str):
        """
        Init strategies in queue.
        """
        strategy = self.strategies[strategy_name]

        if strategy.inited:
            self.write_log(f"{strategy_name}已经完成初始化，禁止重复操作")
            return

        self.write_log(f"{strategy_name}开始执行初始化")

        # Call on_init function of strategy
        self.call_strategy_func(strategy, strategy.on_init)

        # Restore strategy data(variables)
        data = self.strategy_data.get(strategy_name, None)
        if data:
            for name in strategy.variables:
                value = data.get(name, None)
                if name == "pos":
                    pos = getattr(strategy, name)
                    pos.update(value)
                elif value:
                    setattr(strategy, name, value)

        # Subscribe market data
        for vt_symbol in strategy.vt_symbols:
            contract: ContractData = self.main_engine.get_contract(vt_symbol)
            if contract:
                req = SubscribeRequest(
                    symbol=contract.symbol, exchange=contract.exchange)
                self.main_engine.subscribe(req, contract.gateway_name)
            else:
                self.write_log(f"行情订阅失败，找不到合约{vt_symbol}", strategy)

        # Put event to update init completed status.
        strategy.inited = True
        self.put_strategy_event(strategy)
        self.write_log(f"{strategy_name}初始化完成")
```

&nbsp;

### 启动策略
- 检查策略初始化状态；
- 检查策略启动状态，避免重复启动；
- 调用策略类的on_start()函数启动策略；
- 策略启动状态变成True，并且更新到图形化界面上。

```
    def start_strategy(self, strategy_name: str):
        """
        Start a strategy.
        """
        strategy = self.strategies[strategy_name]
        if not strategy.inited:
            self.write_log(f"策略{strategy.strategy_name}启动失败，请先初始化")
            return

        if strategy.trading:
            self.write_log(f"{strategy_name}已经启动，请勿重复操作")
            return

        self.call_strategy_func(strategy, strategy.on_start)
        strategy.trading = True

        self.put_strategy_event(strategy)
```

![启动](https://user-images.githubusercontent.com/11263900/93662312-bdd0c880-fa91-11ea-97b1-8a3505cfcb97.png)

如果当天发生了交易，主界面如图：

![主界面](https://user-images.githubusercontent.com/11263900/93662316-bf01f580-fa91-11ea-8ac6-807e28cbadd5.png)

&nbsp;

### 停止策略
- 检查策略启动状态；
- 调用策略类的on_stop()函数停止策略；
- 更新策略启动状态为False；
- 对所有为成交的委托（市价单/限价单/本地停止单）进行撤单操作；
- 把策略参数，最新的技术指标，以及持仓数量保存到.vntrader/cta_strategy_data.json内；
- 在图形化界面更新策略状态。

```
    def stop_strategy(self, strategy_name: str):
        """
        Stop a strategy.
        """
        strategy = self.strategies[strategy_name]
        if not strategy.trading:
            return

        # Call on_stop function of the strategy
        self.call_strategy_func(strategy, strategy.on_stop)

        # Change trading status of strategy to False
        strategy.trading = False

        # Cancel all orders of the strategy
        self.cancel_all(strategy)

        # Sync strategy variables to data file
        self.sync_strategy_data(strategy)

        # Update GUI
        self.put_strategy_event(strategy)
```

![停止](https://user-images.githubusercontent.com/11263900/93662315-be695f00-fa91-11ea-8b1a-78a2f945db23.png)

&nbsp;

### 编辑策略
- 重新配置策略参数字典setting；
- 更新参数字典到策略中；
- 保存参数字典setting;
- 在图像化界面更新策略状态。

```
    def edit_strategy(self, strategy_name: str, setting: dict):
        """
        Edit parameters of a strategy.
        """
        strategy = self.strategies[strategy_name]
        strategy.update_setting(setting)

        self.save_strategy_setting()
        self.put_strategy_event(strategy)
```

&nbsp;

### 移除策略
- 检查策略状态，只有停止策略后从可以移除策略；
- 从symbol_strategy_map字典中移除该策略订阅的合约信息；
- 从strategy_orderid_map字典移除活动委托记录；
- 从strategies字典移除该策略的相关配置信息。

```
    def remove_strategy(self, strategy_name: str):
        """
        Remove a strategy.
        """
        strategy = self.strategies[strategy_name]
        if strategy.trading:
            self.write_log(f"策略{strategy.strategy_name}移除失败，请先停止")
            return

        # Remove from symbol strategy map
        for vt_symbol in strategy.vt_symbols:
            strategies = self.symbol_strategy_map[vt_symbol]
            strategies.remove(strategy)

        # Remove from vt_orderid strategy map
        for vt_orderid in strategy.active_orderids:
            if vt_orderid in self.orderid_strategy_map:
                self.orderid_strategy_map.pop(vt_orderid)

        # Remove from strategies
        self.strategies.pop(strategy_name)
        self.save_strategy_setting()

        return True
```


