# 柯西 1.3 的 MultiSP 兼容插件

`XiMultiSPCompat` 由 Antigravity 为 MomoCyann 的柯西项目制作。实测环境是 Windows 版《皇牌空战 7》单人任务、AC7 专用 UE4SS、Northter 的 AC7 MultiSP 2.27.0 和柯西 1.3。任务中只用原版单特武时，可只安装柯西机体包与原有依赖。[AC7 MultiSP 源码与说明](https://github.com/KareNorthter/AC7MultiSP) · [AC7 专用 UE4SS](https://github.com/Playmore-ModdingConcepts/PM-UE4SS)

## 安装

1. 退出游戏，安装 AC7 专用 UE4SS、原版 AC7 MultiSP 和柯西 1.3。
2. 把拿到的 `XiMultiSPCompat` 整个文件夹复制到游戏根目录的 `ue4ss/Mods/`。其中应有空的 `enabled.txt`、`Scripts/main.lua` 和 `bin/XiMultiSPCompat.dll`。如果曾用早期试验版替换 MultiSP 核心，先恢复 MultiSP 2.27.0 自带的 `DualSP.dll`，再加载独立插件。
3. 在单人任务里同时选择 ADMM（SP1）和 EML（SP3），从完全展开的 SP1 直接切到 EML，再直接切回来。观察前一姿态是否收回、步枪是否随 EML 举起，并核对发射与准星。退回主武器后再试一次普通往返切换。

```text
ACE COMBAT 7/
├─ Game/Content/Paks/~mods/
│  └─ ~~~~~~~~A_X-02S_To_XiGundam_v1.3_P.pak
└─ ue4ss/Mods/
   ├─ DualSP/
   │  └─ bin/DualSP.dll
   └─ XiMultiSPCompat/
      ├─ enabled.txt
      ├─ Scripts/main.lua
      └─ bin/XiMultiSPCompat.dll
```

回退时退出游戏，把整个 `XiMultiSPCompat` 文件夹移出 `ue4ss/Mods/`。

## 原因与实现

MultiSP 可以把武器从 SP1 直接切到 SP3，柯西原来的变形动画依赖的 `swc1` 却可能仍留在展开值。经过主武器会触发原生的退出路径，所以绕一次主武器能复位。修复复用了已有的两段动画，在目标武器槽改变时从当前开度推进 `swc1` 和 `swc3`。

Lua 在单人飞行场景识别柯西的 X-02S Pawn、机体网格、AnimBP 与三个武器配置，再绑定 DLL。DLL 只调整柯西取姿态用的两个开度，武器选择、装填和发射仍由游戏与 MultiSP 负责。检测到上游已平滑处理对应过渡时，插件会让出控制。DLL 会核对游戏运行入口，游戏或 UE4SS 更新后应重新验证兼容性。

## 验证与复现

- 查看 `ue4ss/Mods/XiMultiSPCompat/XiMultiSPCompat.log`。`BOUND` 表示识别到了适用机体，`ENGAGED` 表示开始处理过渡，`TRANSFER from=0 to=2` 表示观察到 SP1 → SP3。`YIELDED` 表示上游已处理当前过渡。日志用于确认插件进入了哪条路径，最终姿态以实战画面为准。
- 本地已安装的 64 位 `XiMultiSPCompat.dll` SHA-256 为 `7548fb1a7466ef14adc57b28146b39bc3a8982df2d8378304239fe28dae6f6f3`。项目发起者已确认这套 MultiSP 兼容方案可用。
- 要复做插件，需要取得 Antigravity 交付的 C++、Lua 和构建脚本。已验证的构建使用 llvm-mingw 的 Windows x64 UCRT 工具链，Python 与 `pefile` 用于核查 DLL。GitHub 仓库收录本说明与 Agent Skill。
