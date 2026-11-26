---
name: ac7-gundam-mod
description: Build, diagnose, or document an Ace Combat 7 X-02S aircraft replacement with local user-supplied assets. Use for Blender rig and light edits, UE4.18 cooking, speed and joystick articulation, aim alignment, SP1 transitions, compatible weapons, or consolidating a tested PAK stack. The Xi Gundam project is a worked case study.
---

# AC7 机体替换

按当前任务选读参考。第一次搭环境时读[工具和准备](references/setup.md)，制作和排错时读[工作流程](references/workflow.md)，需要核对柯西 1.0 至 1.1 的参数、来源或开发分工时读[项目记录](references/xi-case-study.md)。所有工具路径和输入由当前工作区提供。

1. 确认目标槽位、正在加载的 PAK、当前 Blender 主文件和可回滚的实机基线。记录原样复用、修改复用和参考实现各自的来源。先用一份最小网格替换验证机库和单人任务，再增加动画通道。
2. 从已运行的 Pawn、`PartsAnimationConfig`、`PlaneBodyConfig` 和主动画读出真实驱动。保存原骨名、骨序、父链、绑定矩阵、材质槽和挂点。把速度、襟翼、四向操纵、火控和 SP1 选择分路。分析普通和 SP1 时，同时检查渲染骨与喷口/发射位的世界变换。
3. 从用户确认的 `.blend` 导出 FBX，在 UE 4.18 Cook。将肩灯按实际斜面洞口拟合为平面，检查权重、UV、材质和真正的发光遮罩。修改 Constant Key Lerp 动画时，按游戏读取的逐骨 T/R/S 顺序同步重写轨道流和偏移表。
4. SP1 目标姿态可独立于飞行速度，进入从当前普通姿态混合过去，退出接回实时普通姿态。模型重影先查场景中同一部件是否被两份网格同时绘制。机库闪退先还原可加载的 Pawn、武器类、移动组件、发射网格和 AnimBP 组合，再逐项加回改动。
5. 修正低速横滚残留时，沿渲染骨和实际枪口挂点分别向上查父链，比较摇杆压住跨速度段与回中之后的变换。只改收枪姿态时，按速度采集用户改的骨骼，逐帧确认瞄准和开火依赖的父链及轨道保持一致。
6. 合包时从已验证的安装组合读取挂载点、内部路径及覆盖关系，保留每个路径本来生效的文件。核对 `.uasset`、`.uexp` 和 `.ubulk` 来源，以及仍单独加载的第三方包。用 UnrealPak `-Test`，解包比较 SHA-256，再切换安装。记录离线核对、实机观察和可回退的文件名与优先级。

交付时列出已验证的内容、所用来源和环境依赖。公开分发按实际复用的模型、贴图、骨骼、动画和蓝图取得适用许可。
