Soyuz TMA and R7 — KSP 1.12.5 修复版

中文部分

一、原 Mod 简介

"Soyuz TMA and R7" 是一个高质量真实还原的 KSP 部件包，旨在重现俄罗斯联盟号运载火箭与联盟号/进步号飞船：

- R7 火箭家族：高清一体化助推级模型、可工作的游动发动机(Vernier)与格栅翼、利用贮箱余量氧化剂工作的分离反推喷管、RD-0110 与 RD-0124 发动机(覆盖联盟 U 与联盟 2 构型)、弗雷盖特上面级与 ESA 载荷整流罩(联盟 ST 构型)。
- 联盟号与进步号飞船：带完整 IVA 的返回舱、独立的姿态/平移两套 RCS 系统、带开合动画的发动机罩(SKD Cap)、与原版对接口兼容且带伸缩探杆的对接机构、易用的太阳翼、真实的分舱分离时序。

原 Mod 最初由 DECQ 于 2014 年发布；2020 年由 Dragon01 与 zer0Kerbal 整理重制为 v1.0(1.11.x)，SpaceDock 上传者为 Nigel_Cardozo。RD-0124 发动机模型由 BobCat 制作(授权使用)；包内分发的 SSTU 插件按 GPL3.0 许可，ASET/RasterPropMonitor 按各自许可，非必需但可提供高质量 IVA 体验。

二、官方链接

- SpaceDock：[https://spacedock.info/mod/2563/Soyuz%20TMA%20and%20R7](https://spacedock.info/mod/2563/Soyuz%20TMA%20and%20R7)
- 官方论坛帖：[https://forum.kerbalspaceprogram.com/topic/198004-111x-soyuz-tma-and-r7-v10-09112020/](https://forum.kerbalspaceprogram.com/topic/198004-111x-soyuz-tma-and-r7-v10-09112020/)
- 许可：MIT（RD-0124 模型为 BobCat 保留所有权利、经授权使用）

官方发布标注支持至 KSP 1.10.1，本包为社区制作的 1.12.5 兼容修复版。

三、修复内容

1. 联盟 TMA 服务舱(SOYUZ_ENGINE / PAO)

- 移除了损坏的 SSTUAnimateControlled 与 SSTUDeployableEngine 模块。它们缺少必需的 ANIMATION 子节点，导致 OnLoad 崩溃 + 飞行中每帧刷屏的 NullReferenceException(日志中超过 23000 条)，并把 SKD 发动机锁定在"未展开"状态。
- 恢复 ModuleAnimateGeneric 开关 —— "Open SKD Cap / Close SKD Cap"(打开发动机罩)重新可用。
- SKD 发动机关机后可正常重新点火(RO 配置下 30 次点火、挤压式供应)。

2. 联盟整流罩(SOYUZ_FG1 / FAIRING_FG_1)

- 移除了引用不存在 realISP 事件的 SSTUFieldManipulator 及 SSTU_FIELDDATA 节点，消除整流罩部件加载时的 NRE。

3. 新增 1.12.5 兼容补丁(1.12.5_CompatFix.cfg)

- 修正 RealismOverhaul 官方适配配置中的字段拼写错误：useGimbalResponseSpeec → useGimbalResponseSpeed、gimbalTransform → gimbalTransformName，恢复联盟/进步 PAO 万向节 16°/s 的响应速度。

4. 进步号(Progress)部件核验与修复

- 逐一核对 PAO_PROGRESS / PROGRESS / PROGRESS_FG1：模型动画名(Default Take、ANTENNA、LIGHT_SOYUZ、animation)、推力/相机/RCS 变换名、RO 下的 RF 引擎转换(S5.80)全部正确。
- PAO_PROGRESS 的 SKD 罩动画模块补齐了显式字段(isOneShot / allowManualControl / allowAnimationWhileShielded)。

5. 配置文件格式修复(全 Mod 扫描)

- 23 个 .cfg 文件缺失文件末尾的 PART 闭合括号(2014 原版遗留问题，KSP 解析器宽容所以从未报错)——已全部补上，全部文件括号平衡。
- EAS_top.cfg(逃逸塔抛离发动机)末尾截除了 PART 之外无法被游戏读取的畸形残留节点。

6. 工艺文件清理

- 联盟号 / 联盟号对接阿波罗 / 自动存档飞船三个 VAB 工艺文件中移除了已不存在的 SSTU 死模块(原文件备份为 .bak)。

7. 引擎现代化

- PAO 的过时 ModuleEngines 升级为 ModuleEnginesFX(含 EFFECTS 音效/粒子与 RCSFX)，完全适配 KSP 1.8+ 引擎框架。

### 四、安装方法

1. 将压缩包内 GameData 下的 SOYUZ_SPACECRAFT 与 DECQ_R7 两个文件夹复制到你的 KSP GameData 目录(覆盖同名文件)。
2. 启动 KSP —— ModuleManager 会自动检测文件变化并重新应用补丁(首次加载稍慢属正常)。
3. 已有的飞船与存档无需重建；旧的 SSTU 模块记录会被安全忽略。
4. 推荐环境：RealismOverhaul + RealFuels + RealPlume + ROEngines(RO 官方自带本 Mod 的适配配置)；纯原版环境也可正常使用。
5. VAB 示例工艺(VAB/SOYUZ.craft、VAB/PROGRESS.craft)可复制到 saves/<存档名>/Ships/VAB/ 使用。

五、本包内容

- Soyuz_TMA_and_R7_1.12.5_Fixed.zip 

六、验证方法

1. VAB 中右键服务舱(PAO)，出现 Open SKD Cap 按钮。
2. 飞行中：开罩 → 点火 → 关机 → 再次点火，应可正常重启。
3. Alt+F12 控制台不再刷 NullReferenceException。

本修复基于对 KSP.log(约 5 万行)与 ModuleManager 缓存的分析，所有修改均为配置层面(.cfg)，未改动任何模型或插件二进制文件。



 English Section

Soyuz TMA and R7 — Fixed for KSP 1.12.5

1. About the Original Mod

"Soyuz TMA and R7" is a high-quality realistic part pack that recreates the Russian Soyuz rocket and the Soyuz/Progress spacecraft:

- R7 rocket family: HD integrated booster models, working verniers and grid fins, reaction nozzles for stage separation using leftover oxidizer, RD-0110 and RD-0124 engines (Soyuz U and Soyuz 2 configurations), Fregat upper stage and ESA fairings (Soyuz ST configuration).
- Soyuz & Progress: descent module with a fully working IVA, separate RCS systems for attitude control and translation, an animated engine cover (SKD Cap), a docking port compatible with the stock Clamp-o-tron plus an extendable soft-dock probe, easy-to-use solar panels, and a realistic separation sequence.

The mod was originally created by DECQ in 2014; it was repacked and updated to v1.0 (KSP 1.11.x) in 2020 by Dragon01 and zer0Kerbal, and uploaded to SpaceDock by Nigel_Cardozo. The RD-0124 engine model is by BobCat (used with permission); the redistributed SSTU plugin is under GPL3.0, and ASET / RasterPropMonitor are included under their own licenses (not required, but provide a high-quality IVA).

2. Official Links

- SpaceDock: [https://spacedock.info/mod/2563/Soyuz%20TMA%20and%20R7](https://spacedock.info/mod/2563/Soyuz%20TMA%20and%20R7)
- Forum thread: [https://forum.kerbalspaceprogram.com/topic/198004-111x-soyuz-tma-and-r7-v10-09112020/](https://forum.kerbalspaceprogram.com/topic/198004-111x-soyuz-tma-and-r7-v10-09112020/)
- License: MIT (RD-0124 model © BobCat, used with permission)

The official release is marked for KSP 1.10.1; this package is a community-made 1.12.5 compatibility fix.

3. Fix List

1. Soyuz TMA Service Module (SOYUZ_ENGINE / PAO)

- Removed the broken SSTUAnimateControlled / SSTUDeployableEngine modules which lacked the required ANIMATION sub-nodes, causing an OnLoad crash plus a per-frame NullReferenceException flood (23,000+ entries in KSP.log) and locking the SKD engine in its un-deployed state.
- Restored the ModuleAnimateGeneric toggle — Open/Close SKD Cap works again.
- The SKD engine can now be shut down and restarted normally (30 ignitions, pressure-fed under RO).

2. Soyuz Fairing (SOYUZ_FG1 / FAIRING_FG_1)

- Removed the SSTUFieldManipulator module and its SSTU_FIELDDATA nodes that referenced a non-existent "realISP" event (the NRE source on the fairing part).

3. New 1.12.5 Compatibility Patch (1.12.5_CompatFix.cfg)

- Fixes field-name typos in RealismOverhaul's official configs (useGimbalResponseSpeec → useGimbalResponseSpeed, gimbalTransform → gimbalTransformName), restoring the 16°/s gimbal response on both Soyuz and Progress PAO.

4. Progress Parts Verification & Fix

- Verified all Progress parts (PAO_PROGRESS / PROGRESS / PROGRESS_FG1): animation and transform names, and the RF engine conversion (S5.80) under RO are all correct.
- Added explicit animation fields to the Progress SKD cover module (isOneShot / allowManualControl / allowAnimationWhileShielded).

5. Config File Format Fix (Full Mod Scan)

- Appended the missing closing brace to 23 .cfg files (a legacy issue from the original 2014 files that KSP's lenient parser silently tolerated) — all files are now balanced.
- Truncated malformed trailing nodes left outside the PART node in EAS_top.cfg.

6. Craft File Cleanup

- Removed dead SSTU module blocks from three VAB craft files; originals backed up as .bak.

7. Engine Modernization

- Upgraded the PAO's deprecated ModuleEngines to ModuleEnginesFX (with EFFECTS audio/particles and RCSFX), fully compatible with the KSP 1.8+ engine framework.

4. Installation

1. Copy SOYUZ_SPACECRAFT and DECQ_R7 from the zip's GameData folder into your KSP GameData (overwrite).
2. Launch KSP — ModuleManager detects the changed files and re-applies patches automatically (a slightly longer first load is normal).
3. Existing craft files and saves do not need rebuilding; old SSTU module records are ignored safely.
4. Recommended environment: RealismOverhaul + RealFuels + RealPlume + ROEngines (RO ships official configs for this mod); it also works standalone without RO.
5. The sample crafts (VAB/SOYUZ.craft, VAB/PROGRESS.craft) can be copied to saves/<save name>/Ships/VAB/.

5. Package Contents

- Soyuz_TMA_and_R7_1.12.5_Fixed.zip

6. Verification

1. In the VAB, right-click the PAO → the Open SKD Cap button is present.
2. In flight: open cap → ignite → shutdown → ignite again, restart works.
3. The Alt+F12 console no longer spams NullReferenceException.

This fix is based on analysis of KSP.log (~50k lines) and the ModuleManager cache. All changes are config-level (.cfg) only; no models or plugin binaries were modified.