<div align="center">

# nonebot-plugin-saalc

_✨ 使用 SAA 风格混合发送 SAA 和 alc 的消息 ✨_

<a href="./LICENSE">
    <img src="https://img.shields.io/github/license/AzideCupric/nonebot-plugin-saalc.svg" alt="license">
</a>
<a href="https://pypi.python.org/pypi/nonebot-plugin-saalc">
    <img src="https://img.shields.io/pypi/v/nonebot-plugin-saalc.svg" alt="pypi">
</a>
<img src="https://img.shields.io/badge/python-3.9+-blue.svg" alt="python">

</div>

## 介绍

saalc 提供[SAA](https://github.com/MountainDash/nonebot-plugin-send-anything-anywhere)自有消息段与 [plugin-alconna.uniseg](https://github.com/nonebot/plugin-alconna) 的兼容。允许同时使用 SAA 和 plugin-alconna.uniseg。

`UniMessageFactory` 继承自 SAA 的 `MessageFactory`，与原有的 `MessageFactory` 用法基本一致，只是可以混合使用 `nonebot_plugin_alconna` 和 `nonebot_plugin_saa` 的消息段类型（包括SAA不支持的消息段类型）

```python
from nonebot_plugin_alconna import Text as AlcText, Image as AlcImage, UniMessage, File as AlcFile
from nonebot_plugin_saa import Text as SaaText, Image as SaaImage
from nonebot_plugin_saa.ext.uniseg import UniMessageFactory

@a_matcher.handle()
async def some():
    # 混合使用
    umf = UniMessageFactory(
        [
            AlcText("alc"),
            SaaText("saa"),
            AlcImage(url="https://al.c/image.png"),
            SaaImage("https://sa.a/image.png")
        ]
    )

    await umf.send()

    # 从 UniMessage 转换
    um = UniMessage(
        [
            AlcText("alc"),
            AlcImage(url="https://al.c/image.png"),
        ]
    )
    umf = UniMessageFactory.from_unimsg(um)
    await umf.send()

    # 混用 SAA 不支持的 File 消息段
    umf = UniMessageFactory(
        [
            SaaText("alc"),
            AlcFile(url="https://al.c/file.zip"),
        ]
    )
    await umf.send()
```

> [!IMPORTANT]
>
> - 对于 `UniMessageFactory` 的发送结果(`send/send_to/finish`)， 所产生的消息回执仍然是 SAA 的[消息回执](https://saa.none.bot/usage/message-build#receipt)。

### 兼容性

如果想在现有的 SAA 项目中使用 plugin-alconna.uniseg，可以直接导入 `UniMessageFactory` 来发送消息
只需要这样做：

```python
from nonebot_plugin_saalc import UniMessageFactory as MessageFactory
```

就可以基本无缝地替换原有的 `MessageFactory`。<del>不保证，但你可以提 Issue 甚至 Pull Request 是吧（</del>

saalc 是基于 SAA 的功能扩展，其范围基本以 SAA 的功能为主，saalc 中所提供功能范围的取决情况：

| 功能       | SAA | alc |
| ---------- | --- | --- |
| 消息段范围 | ✅  | ✅  |
| 平台目标   | ✅  |     |
| 消息回执   | ✅  |     |
| 消息 ID    | ✅  |     |

## 安装方法

<details open>
<summary>使用 nb-cli 安装</summary>
在 nonebot2 项目的根目录下打开命令行, 输入以下指令即可安装

    nb plugin install nonebot-plugin-saalc

</details>

<details>
<summary>使用包管理器安装</summary>
在 nonebot2 项目的插件目录下, 打开命令行, 根据你使用的包管理器, 输入相应的安装命令

<details>
<summary>pip</summary>

    pip install nonebot-plugin-saalc

</details>
<details>
<summary>pdm</summary>

    pdm add nonebot-plugin-saalc

</details>
<details>
<summary>poetry</summary>

    poetry add nonebot-plugin-saalc

</details>

打开 nonebot2 项目根目录下的 `pyproject.toml` 文件, 在 `[tool.nonebot]` 部分追加写入

    plugins = ["nonebot_plugin_template"]

</details>

## 相关项目

- [nonebot2](https://nonebot.dev/)
- [nonebot-plugin-alconna](https://alc.none.bot/)
- [nonebot-plugin-send-anything-anywhere](https://saa.none.bot/)
