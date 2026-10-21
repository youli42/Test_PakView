# Test_PakView

`PakViz` 插件的宿主测试工程（Unreal Engine 5.8），用于开发和验证编辑器内的资产/包体体积分析工具。

插件以 **git submodule** 的形式放在 `Plugins/PakViz`：

- 插件仓库：<https://github.com/youli42/PakViz>
- 版本管理：插件源码在独立仓库维护，工程只记录子模块的 commit；升级插件时在 `Plugins/PakViz` 里 `git pull` 后提交父仓库的 gitlink 即可。

## 克隆

```bat
git clone --recurse-submodules https://github.com/youli42/Test_PakView.git
```

如果已经克隆但没带子模块：

```bat
git submodule update --init --recursive
```

## 编译 / 运行

需要 Unreal Engine 5.8。

```bat
"C:\Program Files\Epic Games\UE_5.8\Engine\Build\BatchFiles\Build.bat" ^
  Test_PakViewEditor Win64 Development ^
  -Project="E:\E_SATA_WP\UE\Test_PakView\Test_PakView.uproject" -WaitMutex -FromMsBuild
```

打开 `Test_PakView.uproject` 后，菜单 **Tools → PakViz → 打开 PakViz** 打开 PakStat 主窗口（文件夹树 + Class 面板 + 树图）；**Tools → PakViz → 资产排行** 打开独立的资产排行窗口。引擎/插件内容统一收在文件夹树的 **Plugins** 父节点下，可用工具栏「内容」区域的引擎/插件/脚本开关控制显隐。

无头验证：

```bat
"C:\Program Files\Epic Games\UE_5.8\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" ^
  "E:\E_SATA_WP\UE\Test_PakView\Test_PakView.uproject" ^
  -unattended -nop4 -nosplash -nullrhi -stdout ^
  -ExecCmds="PakViz.OpenTab,PakViz.OpenAssetRanking,PakViz.Scan,PakViz.SelfTest,PakViz.Exit" ^
  -abslog="E:\E_SATA_WP\UE\Test_PakView\Saved\Logs\PakViz_Verify.log"
```

通过标准：日志出现 `PakViz self test finished`，且无 `Assertion failed` / `Fatal error`。

## 说明

- 本工程 `Content` 为空，扫描结果主要来自 `/Engine` 和插件内容，属预期（PakStat 页的 `/Game` 业务分组为空）。
- 插件是 Editor-only，不参与 cook/打包。
- 插件使用、数据口径、路线图见 [Plugins/PakViz/README.md](Plugins/PakViz/README.md)。
