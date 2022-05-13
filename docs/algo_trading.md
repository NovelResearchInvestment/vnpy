# 算法交易
算法交易可以用于把巨型单子拆分成一个个小单，能够有效降低交易成本，冲击成本等（冰山算法、狙击手算法)；也可以在设定的阈值内进行高抛低吸操作(网格算法、套利算法）。

&nbsp;

AlgoTrading是用于**算法委托执行交易**的模块，用户可以通过其UI界面操作来便捷完成启动算法、保存配置、停止算法等任务。

算法交易模块主要由4部分构成，如下图：

- engine：定义了算法引擎，其中包括：引擎初始化、保存/移除/加载算法配置、启动算法、停止算法、订阅行情、挂撤单等。
- template：定义了交易算法模板，具体的算法示例，如冰山算法，都需要继承于该模板。
- algos：具体的交易算法示例。用户基于算法模板和官方提供是算法示例，可以自己搭建新的算法。
- ui：基于PyQt5的GUI图形应用。

### VeighNa Station加载

启动登录VeighNa Station后，点击【交易】按钮，在配置对话框中的【应用模块】栏勾选【AlgoTrading】。

## 基本操作

在VN Trader的菜单栏中点击“功能”—>“算法交易”即可打开如图算法交易模块窗口，如下图。

算法交易模块有2部分构成：
- 委托交易，用于启动算法交易；
- 数据监控，用于监控算法交易执行情况，并且能够手动停止算法。

![](https://vnpy-community.oss-cn-shanghai.aliyuncs.com/forum_experience/yazhang/algo_trader/algo_trader_all_section.png)

&nbsp;

### 委托交易

下面以时间加权平均算法为例，具体介绍如下图委托交易功能选项。
- 算法：目前提供了5种交易算法：时间加权平均算法、冰山算法、狙击手算法、条件委托、最优限价；
- 本地代码：vt_symbol格式，如AAPL.SMART, 用于算法交易组建订阅行情和委托交易；
- 方向：做多或者做空；
- 价格：委托下单的价格；
- 数量：委托的总数量，需要拆分成小单进行交易；
- 执行时间：运行改算法交易的总时间，以秒为单位；
- 每轮间隔：每隔一段时间（秒）进行委托下单操作；
- 启动算法：设置好算法配置后，用于立刻执行算法交易。

所以，该算法执行的任务如下：通过时间加权平均算法，买入10000股AAPL（美股），执行价格为180美金，执行时间为600秒，间隔为6秒；即每隔6秒钟，当买一价少于等于180时，以180的价格买入100股AAPL，买入操作分割成100次。

![](https://vnpy-community.oss-cn-shanghai.aliyuncs.com/forum_experience/yazhang/algo_trader/trading_section.png)

交易配置可以保存在json文件，这样每次打开算法交易模块就不用重复输入配置。其操作是在“算法名称”选项输入该算法设置命名，然后点击下方"保存设置”按钮。保存的json文件在C:\Users\Administrator\\.vntrader文件夹的algo_trading_setting.json中，如图。

![](https://vnpy-community.oss-cn-shanghai.aliyuncs.com/forum_experience/yazhang/algo_trader/setting.png)

委托交易界面最下方的“全部停止”按钮用于一键停止所有执行中的算法交易。

&nbsp;

### 数据监控

数据监控由4个部分构成。

- 活动组件：显示正在运行的算法交易，包括：算法名称、参数、状态。最右边的“停止”按钮用于手动停止执行中的算法。

![](https://vnpy-community.oss-cn-shanghai.aliyuncs.com/forum_experience/yazhang/algo_trader/action.png)

&nbsp;

- 历史委托组件：显示已完成的算法交易，同样包括：算法名称、参数、状态。

![](https://vnpy-community.oss-cn-shanghai.aliyuncs.com/forum_experience/yazhang/algo_trader/final.png)

&nbsp;

- 日志组件：显示启动、停止、完成算法的相关日志信息。在打开算法交易模块后，会进行初始化，故日志上会首先显示“算法交易引擎启动”和“算法配置载入成功”。

![](https://vnpy-community.oss-cn-shanghai.aliyuncs.com/forum_experience/yazhang/algo_trader/log_section.png)

&nbsp;

- 配置组件：用于载入algo_trading_setting.json的配置信息，并且以图形化界面显示出来。
用户可以点击“使用”按钮立刻读取配置信息，并显示在委托交易界面上，点击“启动算法”即可开始进行交易；
用户也可以点击“移除”按钮来移除该算法配置，同步更新到json文件内。

![](https://vnpy-community.oss-cn-shanghai.aliyuncs.com/forum_experience/yazhang/algo_trader/setting_section.png)

&nbsp;

## 算法示例


### 直接委托算法

直接发出新的委托（限价单、停止单、市价单）

```
    def on_tick(self, tick: TickData):
        """"""
        if not self.vt_orderid:
            if self.direction == Direction.LONG:
                self.vt_orderid = self.buy(
                    self.vt_symbol,
                    self.price,
                    self.volume,
                    self.order_type,
                    self.offset
                )
                
            else:
                self.vt_orderid = self.sell(
                    self.vt_symbol,
                    self.price,
                    self.volume,
                    self.order_type,
                    self.offset
                )
        self.put_variables_event()
```

&nbsp;

### 时间加权平均算法

在启动模块之前，请先连接交易接口（连接方法详见基本使用篇的连接接口部分）。看到VeighNa Trader主界面【日志】栏输出“合约信息查询成功”之后再启动模块，如下图所示：

```
    def on_timer(self):
        """"""
        self.timer_count += 1
        self.total_count += 1
        self.put_variables_event()

        if self.total_count >= self.time:
            self.write_log("执行时间已结束，停止算法")
            self.stop()
            return

        if self.timer_count < self.interval:
            return
        self.timer_count = 0

        tick = self.get_tick(self.vt_symbol)
        if not tick:
            return

        self.cancel_all()

        left_volume = self.volume - self.traded
        order_volume = min(self.order_volume, left_volume)

        if self.direction == Direction.LONG:
            if tick.ask_price_1 <= self.price:
                self.buy(self.vt_symbol, self.price,
                         order_volume, offset=self.offset)
        else:
            if tick.bid_price_1 >= self.price:
                self.sell(self.vt_symbol, self.price,
                          order_volume, offset=self.offset)
```

&nbsp;

配置参数要求如下：

- 算法：在下拉框中选择要执行的交易算法；
- 本地代码：格式为vt_symbol（合约代码 + 交易所名称）；
- 方向：多、空；
- 价格：委托下单的价格；
- 数量：委托的总数量，需要拆分成小批订单进行交易；
- 执行时间（秒）：运行该算法交易的总时间，以秒为单位；
- 每轮间隔（秒）：每隔多少时间进行委托下单操作，以秒为单位；
- 开平：开、平、平今、平昨。

### 保存配置

交易算法的配置信息可以用json文件保存在本地，这样每次打开算法交易模块无需重复输入，具体操作如下：

- 在【配置名称】选项中输入该算法配置信息的命名，然后点击下方【保存配置】按钮，即可保存配置信息到本地；
- 保存配置后，在界面右侧的【配置】组件可以看到用户保存的配置名称和配置参数。

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/4.png)

保存的配置文件在.vntrader文件夹下的algo_trading_setting.json中，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/5.png)


## 启动算法

目前VeighNa一共提供了六种常用的示例算法。本文档以时间加权平均算法（TWAP）为例，介绍算法启动过程。

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/3.png)

参数配置完成后（已保存的算法信息，通过在【配置】栏对应的算法下点击【使用】，可切换界面左侧配置的信息内容），点击【启动算法】按钮，即可立刻执行算法交易。

若启动成功，则可在右上角【执行中】界面观测到该算法的执行状态。

图中算法执行的任务具体为：使用时间加权平均算法，买入10000手豆油2109合约（y2109），执行价格为9000元，执行时间为600秒，每轮间隔为6秒；即每隔6秒钟，当合约卖一价小于等于9000时，以9000的价格买入100手豆油2109合约，将买入操作分割成100次。

## CSV启动

当有较多算法需要启动时，可以通过CSV文件来一次性批量启动。点击图形界面左侧的【CSV启动】按钮，在弹出的对话框中找到要导入的CSV文件后打开即可快速启动算法。

请注意，CSV文件的格式应如下图所示，和左侧编辑区的各字段一致：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/12.png)

结合Excel的表格快速编辑功能，批量添加算法较为方便。启动成功后，CSV文件中所有算法的执行情况均会显示在【执行中】界面下，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/14.png)

请注意，CSV启动之后，只能在【执行中】、【日志】和【已结束】三个界面观测到内容输出及变化，不会将CSV文件中的算法信息添加到配置中。


## 停止算法

当用户需要停止正在执行的交易算法时，可以在【执行中】界面点击【停止】按钮，停止某个正在执行的算法交易，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/6.png)

用户也可以在委托交易界面点击最下方的【全部停止】按钮，一键停止所有执行中的算法交易，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/7.png)


## 数据监控

数据监控界面由四个部分构成：

执行中组件：显示正在执行的算法交易，包括：算法、参数和状态。成功启动算法之后，切换到右上角【执行中】界面，会显示该算法的执行状态，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/6.png)

已结束组件：显示已完成的算法交易，同样包括：算法、参数和状态。算法结束或者停止之后，切换到右上角【已结束】界面，会显示该算法的执行状态，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/9.png)

日志组件：显示启动、停止、完成算法的相关日志信息。在打开算法交易模块后，会进行初始化，故【日志】组件会首先输出“算法交易引擎启动”和“算法配置载入成功”，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/11.png)

配置组件：用于载入algo_trading_setting.json的配置信息，并以图形化界面显示在【配置】栏下，如下图所示：

![](https://vnpy-doc.oss-cn-shanghai.aliyuncs.com/algo_trading/8.png)

- 点击配置组件的【使用】按钮立刻读取该配置信息，并显示在委托交易界面上，随后点击【启动算法】即可开始进行交易；
- 点击配置组件的【移除】按钮可以移除该算法配置，并同步更新到json文件内。


## 示例算法

示例算法路径位于algo_trading.algos文件夹下（请注意，个别算法是没有写开平方向的，若有需要，可基于自身需求进行个性化修改）。目前，算法交易模块提供了以下六种内置算法：

### DMA - 直接委托算法

直接委托算法（DMA）直接发出新的委托（限价单、停止单、市价单）。

### TWAP - 时间加权平均算法

时间加权平均算法（TWAP）具体执行步骤如下：

- 将委托数量平均分布在某个时间区域内，每隔一段时间用指定的价格挂出买单（或者卖单）。

- 买入情况：卖一价低于目标价格时，发出委托，委托数量在剩余委托量与委托分割量中取最小值。

- 卖出情况：买一价高于目标价格时，发出委托，委托数量在剩余委托量与委托分割量中取最小值。

### Iceberg - 冰山算法

冰山算法（Iceberg）具体执行步骤如下：

- 在某个价位挂单，但是只挂一部分，直到全部成交。
- 买入情况：先检查撤单：最新Tick卖一价低于目标价格，执行撤单；若无活动委托，发出委托：委托数量在剩余委托量与挂出委托量中取最小值。
- 卖出情况：先检查撤单：最新Tick买一价高于目标价格，执行撤单；若无活动委托，发出委托：委托数量在剩余委托量与挂出委托量中取最小值。

- 买入情况：先检查撤单，最新Tick卖一价低于目标价格，执行撤单；若无活动委托，发出委托，委托数量在剩余委托量与挂出委托量中取最小值。

- 卖出情况：先检查撤单，最新Tick买一价高于目标价格，执行撤单；若无活动委托，发出委托，委托数量在剩余委托量与挂出委托量中取最小值。

        self.timer_count = 0

        contract = self.get_contract(self.vt_symbol)
        if not contract:
            return

        # If order already finished, just send new order
        if not self.vt_orderid:
            order_volume = self.volume - self.traded
            order_volume = min(order_volume, self.display_volume)

            if self.direction == Direction.LONG:
                self.vt_orderid = self.buy(
                    self.vt_symbol,
                    self.price,
                    order_volume,
                    offset=self.offset
                )
            else:
                self.vt_orderid = self.sell(
                    self.vt_symbol,
                    self.price,
                    order_volume,
                    offset=self.offset
                )
        # Otherwise check for cancel
        else:
            if self.direction == Direction.LONG:
                if self.last_tick.ask_price_1 <= self.price:
                    self.cancel_order(self.vt_orderid)
                    self.vt_orderid = ""
                    self.write_log(u"最新Tick卖一价，低于买入委托价格，之前委托可能丢失，强制撤单")
            else:
                if self.last_tick.bid_price_1 >= self.price:
                    self.cancel_order(self.vt_orderid)
                    self.vt_orderid = ""
                    self.write_log(u"最新Tick买一价，高于卖出委托价格，之前委托可能丢失，强制撤单")

        self.put_variables_event()
```

&nbsp;

### 狙击手算法

- 监控最新tick推送的行情，发现好的价格立刻报价成交。
- 买入情况：最新Tick卖一价低于目标价格时，发出委托，委托数量在剩余委托量与卖一量中取最小值。
- 卖出情况：最新Tick买一价高于目标价格时，发出委托，委托数量在剩余委托量与买一量中取最小值。

```
    def on_tick(self, tick: TickData):
        """"""
        if self.vt_orderid:
            self.cancel_all()
            return

        if self.direction == Direction.LONG:
            if tick.ask_price_1 <= self.price:
                order_volume = self.volume - self.traded
                order_volume = min(order_volume, tick.ask_volume_1)

                self.vt_orderid = self.buy(
                    self.vt_symbol,
                    self.price,
                    order_volume,
                    offset=self.offset
                )
        else:
            if tick.bid_price_1 >= self.price:
                order_volume = self.volume - self.traded
                order_volume = min(order_volume, tick.bid_volume_1)

                self.vt_orderid = self.sell(
                    self.vt_symbol,
                    self.price,
                    order_volume,
                    offset=self.offset
                )

        self.put_variables_event()
```

&nbsp;

### 条件委托算法

- 监控最新tick推送的行情，发现行情突破立刻报价成交。
- 买入情况：Tick最新价高于目标价格时，发出委托，委托价为目标价格加上超价。
- 卖出情况：Tick最新价低于目标价格时，发出委托，委托价为目标价格减去超价。

```
    def on_tick(self, tick: TickData):
        """"""
        if self.vt_orderid:
            return

        if self.direction == Direction.LONG:
            if tick.last_price >= self.stop_price:
                price = self.stop_price + self.price_add

                if tick.limit_up:
                    price = min(price, tick.limit_up)

- 买入情况：先检查撤单：最新Tick买一价不等于目标价格时，执行撤单；若无活动委托，发出委托，委托价格为最新Tick买一价，委托数量为剩余委托量。

- 卖出情况：先检查撤单：最新Tick买一价不等于目标价格时，执行撤单；若无活动委托，发出委托，委托价格为最新Tick卖一价，委托数量为剩余委托量。
