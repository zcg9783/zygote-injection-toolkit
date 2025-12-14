# Zygote 注入工具包
这是一个 Python 命令行实用程序，利用 Android Zygote 注入漏洞（CVE-2024-31317），可轻松运行和备份私人应用数据。
要运行此工具，您的设备必须**未**更新至 [2024年6月1日安全补丁](https://source.android.com/security/bulletin/2024-06-01)。如果您不确定设备是否存在漏洞，只需运行脚本，它会自动为您检测。

运行前，需要安装 ADB 并启用 USB 调试功能。
### 安装
要安装或更新，只需运行：`pip install --upgrade git+https://github.com/Anonymous941/zygote-injection-toolkit`。

如果您想修改源代码，请先使用 `git clone` 克隆代码库，然后运行 `pip install -e .`。这将以[开发模式](https://setuptools.pypa.io/en/latest/userguide/development_mode.html)进行安装。
### 使用方法
确保已启用 USB 调试且 ADB 正在运行（可通过运行 `adb start-server` 或几乎任何其他 ADB 命令实现）。然后直接运行 `python -m zygote_injection_toolkit`。如果漏洞利用成功运行，您将在端口 1234 上获得一个具有 `system` 权限的反向 shell（位于您的主机和 Android 设备上）。该工具还会自动尝试强制启用 OEM 解锁功能。
### 关于此漏洞利用
**这不是一个 root 提权漏洞利用！** 它无法运行需要 root 权限的应用，也无法安装任何 Magisk 模块。如果您已经拥有 root 权限，则无需运行此漏洞利用。

它能做的是以 `system` 用户的身份执行任意代码。它能够模拟任何应用（包括特权应用）并读取/写入其私有数据（包括无法通过 `adb backup` 备份的数据）。

以下是其主要用途：

- 在解锁会出于安全目的清除所有数据的引导加载程序之前，备份几乎所有数据
- 通过修改系统应用数据来绕过 OEM 限制
- 此工具会自动尝试绕过运营商对引导加载程序解锁的限制，这*可能*允许您解锁引导加载程序，但这不太可能是唯一的保护措施
- 与 root 提权漏洞利用结合使用（此仓库范围之外）

有关此漏洞利用本身的更多信息，请参阅以下两篇技术分析文章：https://rtx.meta.security/exploitation/2024/06/03/Android-Zygote-injection.html, https://infosecwriteups.com/exploiting-android-zygote-injection-cve-2024-31317-d83f69265088
