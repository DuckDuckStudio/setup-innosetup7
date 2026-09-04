# 安装 Inno Setup 7

在 Windows GitHub Actions 工作流中安装 Inno Setup 7。

在 Windows Runner ([windows-2025](https://github.com/actions/runner-images/blob/main/images/windows/Windows2025-VS2026-Readme.md#tools) / [windows-2022](https://github.com/actions/runner-images/blob/main/images/windows/Windows2022-Readme.md#tools)) 中，默认安装有 Inno Setup 6，但这不是我们想要的。  
这个 Action 会使用 [Chocolatey](https://chocolatey.org/) 卸载已有的 Inno Setup 6，然后通过 [WinGet](https://learn.microsoft.com/zh-cn/windows/package-manager/winget/) 安装 Inno Setup 7，并将 Inno Setup 7 的目录加入工作流的 `PATH`，后续步骤可以直接调用 `iscc` 等命令。

## Q&A

### 为什么不统一使用一个包管理器处理？

[在 Windows 上默认安装的软件，是通过 Chocolatey 安装的](https://github.com/actions/runner-images#package-managers-usage)，[Chocolatey 会将 Inno Setup 安装到 `C:\ProgramData\Chocolatey\bin`](https://docs.chocolatey.org/en-us/default-chocolatey-install-reasoning/)。

如果用 WinGet 来卸载 Chocolatey 安装的 Inno Setup 6，WinGet 可不会管你这的那的，调用卸载程序卸载完了就结束了。  
然后 `iscc`（其他命令同理）依旧指向 `C:\ProgramData\Chocolatey\bin\iscc.exe`，后续调用会报找不到文件。

如果你想用 Chocolatey 来安装 Inno Setup 7，你会发现 [Chocolatey 上只有 Inno Setup 6](https://community.chocolatey.org/packages?q=innosetup)。

### 为什么不能在 Ubuntu / MacOS 上使用？

我想这些系统上并没有 Chocolatey 和 WinGet，对吧？

## 使用方法

就像你平时使用其他操作一样：

```yaml
- name: 安装 Inno Setup 7
  uses: DuckDuckStudio/setup-innosetup7@1.0.0
```

## 许可

本项目采用 [MIT](LICENSE.txt) 许可证。
