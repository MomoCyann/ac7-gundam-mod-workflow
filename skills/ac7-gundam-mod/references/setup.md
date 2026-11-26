# 工具和准备

柯西项目在 Windows 上用 Blender 5.3.0 编辑，用 Unreal Engine 4.18 Cook 和打包。在别的环境复做时，先确认工具能读当前素材和目标游戏的版本。

| 环节 | 准备的软件 | 第一个验证 |
| --- | --- | --- |
| AC6 素材解包 | WitchyBND，柯西原零件通常在 `*.partsbnd.dcx`，贴图来自 TPF/DDS。只做 AC7 资源替换时可跳过这一步。[WitchyBND](https://github.com/ividyon/WitchyBND/releases) | 能打开一个已获准使用的零件，找到 FLVER 和对应贴图。 |
| 模型编辑 | Blender。柯西项目使用 Windows 便携版 5.3.0、`io_import_flver` 0.2.0 和 `io_scene_psk_psa` 9.1.3。[Blender](https://www.blender.org/download/) · [FLVER Importer](https://github.com/shintheweapon/Blender-flver-importer) · [PSK/PSA 插件](https://github.com/DarklightGames/io_scene_psk_psa) | 在同一环境导入一个 FLVER、一个 PSK/PSKX，读取 AC7 的 PSA。记录比例、朝向和骨轴。 |
| Cook 与打包 | UE **4.18**，使用同一安装内的 `UE4Editor-Cmd.exe` 和 `UnrealPak.exe`。在 Epic Games Launcher 的 Unreal Engine 库里选旧版本。[Epic 安装说明](https://dev.epicgames.com/documentation/unreal-engine/install-unreal-engine) | 用小 FBX 生成一套 Cook 资产，在正确挂载路径打包并通过 `UnrealPak -Test`。 |
| 资产比对 | Gildor 的 UE Viewer（UModel），项目使用 `-game=ue4.18` 回读打包资产。UAssetGUI/UAssetAPI 可辅助检查蓝图、导入表和属性。[UModel](https://www.gildor.org/en/projects/umodel) · [UAssetGUI](https://github.com/atenfyr/UAssetGUI) | 从已运行的参考包导出目标骨架和动画，确认网格、动作与相关 `.uasset`、`.uexp`、`.ubulk` 可以对应。 |
| 脚本和验证 | Python、NumPy。柯西 1.1 的姿态差值插值使用 SciPy，检查图集像素可用 Pillow。涉及 `bpy` 的脚本用 Blender 自带 Python 运行。[Python](https://www.python.org/downloads/) · [NumPy](https://numpy.org/install/) · [SciPy](https://scipy.org/install/) · [Pillow](https://pillow.readthedocs.io/en/stable/installation/basic-installation.html) | 先检查脚本解释器和依赖，再把工具、输入、临时输出和成品目录分别配置。 |
| 实机 | PC 版 AC7、可测试的目标机体与单人任务。柯西使用 X-02S，实测环境额外加载了 kosnag 的骨骼上限扩展。[骨骼上限扩展](https://www.nexusmods.com/acecombat7skiesunknown/mods/2392) | 原游戏与已有参考各跑一次机库和任务，记录加载的 PAK。 |
| 同一任务携带多种特武（可选） | 使用 AC7 专用 UE4SS 和 AC7 MultiSP。柯西实测组合使用 MultiSP 2.27.0 与 Antigravity 制作的 `XiMultiSPCompat`。[兼容插件安装与验证](multisp-compat.md) | 确认单人任务加载了插件，用 ADMM 和 EML 验证直接互切，记录游戏与 MultiSP 版本。 |

## 给 Agent 的输入

- 游戏与工具路径，目标机体与武器槽，已验证的初始 PAK。只配置实际要用的工具。
- 可使用的源模型与贴图、原作者链接和许可。参考包也记录来源，以及哪些文件允许改编和分发。
- 当前 `.blend` 及需采集的场景、要保留的姿态和编辑稿、用于对齐的截图。给每次手工调整留可追踪的文件版本。
- 假如目标是复做某阶段，额外准备该阶段的 FBX 或 Cook 基线、骨名和权重映射、挂点坐标、以及上一次通过的 PAK。只提供发布包时，可先做逆向核对和问题定位。

第一次验收可以很小。先换一份可见机体，完成从 Blender → FBX → Cook → PAK → 机库 → 单人任务的闭环。这个基线可用时，再逐步加入速度变形、武器和自发光。
