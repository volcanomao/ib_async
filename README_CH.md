[![Build](https://github.com/ib-api-reloaded/ib_async/actions/workflows/test.yml/badge.svg?branch=next)](https://github.com/ib-api-reloaded/ib_async/actions) [![PyVersion](https://img.shields.io/badge/python-3.10+-blue.svg)](#) <!-- [![Status](https://img.shields.io/badge/status-beta-green.svg)](#) --> [![PyPiVersion](https://img.shields.io/pypi/v/ib_async.svg)](https://pypi.python.org/pypi/ib_async) [![License](https://img.shields.io/badge/license-BSD-blue.svg)](#) <!-- [![Downloads](https://static.pepy.tech/badge/ib-insync)](https://pepy.tech/project/ib-insync) --> [![Docs](https://img.shields.io/badge/Documentation-green.svg)](https://ib-api-reloaded.github.io/ib_async/)

# ib_async

## 更新

项目正在新的管理下。请查看[原始讨论](https://github.com/mattsta/ib_insync/discussions)了解近期历史。在[新的主要仓库](https://github.com/ib-api-reloaded/ib_async)下创建新的讨论、PR或问题以获取持续更新。

欢迎新的贡献。如果您的更新和对IBKR/TWS及Python的理解都非常高质量，我们愿意增加更多的维护者拥有提交权限。

这是一个小型项目，用户群体经验和知识差异较大，因此如果您提出的问题更多的是关于IBKR的问题而不是客户端的问题，除非您的问题是直接的客户端问题，而不是许多IBKR API边缘案例中的问题，我们可能无法提供帮助。如果您不确定问题是否与IBKR、我们的客户端或您自己的代码使用相关，可以随时打开[讨论话题](https://github.com/ib-api-reloaded/ib_async/discussions)。

## 介绍

`ib_async`库的目标是使与来自Interactive Brokers的[Trader Workstation API](https://ibkrcampus.com/ibkr-api-page/twsapi-doc/)的交互尽可能简单。

主要特性包括：

* 易于使用的线性编程风格；
* 一个[IB组件](https://ib-api-reloaded.github.io/ib_async/api.html#module-ib_async.ib)能够自动与TWS或IB Gateway应用程序保持同步；
* 基于[asyncio](https://docs.python.org/3/library/asyncio.html)和[eventkit](https://github.com/erdewit/eventkit)的完全异步框架，适合高级用户；
* 在Jupyter笔记本中与实时数据的交互操作。

一定要查看[笔记本](https://ib-api-reloaded.github.io/ib_async/notebooks.html)、[使用方法](https://ib-api-reloaded.github.io/ib_async/recipes.html)和[API文档](https://ib-api-reloaded.github.io/ib_async/api.html)。

## 安装

```
pip install ib_async
```

要求：

- Python 3.10 或更高版本
  - 我们计划支持[2年前的Python版本](https://devguide.python.org/versions/)，这允许我们随着时间的推移继续添加更新功能和性能改进。
- 一个运行中的IB Gateway应用程序（或启用了API模式的TWS）
    - [稳定版本网关](https://www.interactivebrokers.com/en/trading/ibgateway-stable.php) — 每几个月更新一次
    - [最新版本网关](https://www.interactivebrokers.com/en/trading/ibgateway-latest.php) — 每周更新
- 确保[API端口已启用](https://ibkrcampus.com/ibkr-api-page/twsapi-doc/#tws-download)并勾选“连接时下载未完成订单”。
- 您可能还想增加Java内存使用量至4096 MB（在`Configure->Settings->Memory Allocation`下）以防止加载大量数据时网关崩溃。

不需要IB提供的ibapi包。`ib_async`内部实现了完整的IBKR API协议。

## 手动构建

首先，安装poetry：

```
pip install poetry -U
```
### 仅安装库

```
poetry install
```
### 安装所有内容（启用文档 + 开发测试）

```
poetry install --with=docs,dev
```

## Generate Docs

```
poetry install --with=docs
poetry run sphinx-build -b html docs html
```

## Check Types

```
poetry run mypy ib_async
```

## Build Package

```
poetry build
```

## Upload Package (if maintaining)

```
poetry install
poetry config pypi-token.pypi your-api-token
poetry publish --build
```

## 示例

这是一个完整的脚本来下载历史数据：

```python
from ib_async import *
# util.startLoop()  # 在笔记本中时取消注释此行

ib = IB()
ib.connect('127.0.0.1', 7497, clientId=1)

ib.reqMarketDataType(4)  # 使用免费的、延迟的、冻结的数据
contract = Forex('EURUSD')
bars = ib.reqHistoricalData(
    contract, endDateTime='', durationStr='30 D',
    barSizeSetting='1 hour', whatToShow='MIDPOINT', useRTH=True)

# 转换为pandas数据框（需要安装pandas）：
df = util.df(bars)
print(df)
```

Output:

```
                   date      open      high       low     close  volume
0   2019-11-19 23:15:00  1.107875  1.108050  1.107725  1.107825      -1
1   2019-11-20 00:00:00  1.107825  1.107925  1.107675  1.107825      -1
2   2019-11-20 01:00:00  1.107825  1.107975  1.107675  1.107875      -1
3   2019-11-20 02:00:00  1.107875  1.107975  1.107025  1.107225      -1
4   2019-11-20 03:00:00  1.107225  1.107725  1.107025  1.107525      -1
..                  ...       ...       ...       ...       ...     ...
705 2020-01-02 14:00:00  1.119325  1.119675  1.119075  1.119225      -1
```

## Documentation

The complete [API documentation](https://ib-api-reloaded.github.io/ib_async/api.html).

[Changelog](https://ib-api-reloaded.github.io/ib_async/changelog.html).

## 社区资源

如果您有其他与 `ib_async` 或 `ib_insync` 相关的公开工作，请打开一个问题，我们可以在这里保持一个活跃的列表。

以下项目并未得到任何实体的认可，仅供参考或娱乐目的。

- Adi 关于使用 IBKR API 的直播录像：[Interactive Brokers API in Python](https://www.youtube.com/playlist?list=PLCZZtBmmgxn8CFKysCkcl-B1tqRgCCNIX)
- Matt 的 IBKR Python CLI：[icli](http://github.com/mattsta/icli)
- 通过 IBKR API 进行企业数据解析：[ib_fundamental](https://github.com/quantbelt/ib_fundamental)

## 免责声明

该软件在简化的 BSD 许可证条件下提供。

本项目与 Interactive Brokers Group, Inc. 没有任何关联。

[官方 Interactive Brokers API 文档](https://ibkrcampus.com/ibkr-api-page/twsapi-doc/)

## 历史

该库最初由 [Ewald de Wit](https://github.com/erdewit) 创建，作为 [`tws_async` 在2017年初](https://github.com/erdewit/tws_async)，然后在2017年中期成为更知名的 [`ib_insync` 库](https://github.com/erdewit/ib_insync)。他维护并改进了该库，让全世界的人可以免费使用，直到他在2024年初意外去世。之后，我们决定将项目重命名为 `ib_async`，并在一个新的 GitHub 组织下进行，因为我们失去了对原始仓库及其打包和文档基础设施的修改权限。

该库目前由 [Matt Stancliff](https://github.com/mattsta) 维护，我们欢迎更多的提交者和组织贡献者，如果有人表现出帮助的兴趣。
