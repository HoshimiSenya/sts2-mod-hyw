# STS2 Mod Collection — Arknights Skins

《杀戮尖塔 2》(Slay the Spire 2) 的皮肤模组合集，把原版角色模型与部分美术资源替换为《明日方舟》角色。

本仓库是**制作工程**（Godot 工程 + Spine 源文件），不是可直接使用的 release。想直接玩的请走创意工坊。

- 作者：星见千夜 @Bilibili
- 引擎：Godot **4.5** (.NET / Mono)，渲染后端 Forward Plus

---

## 目录结构

```
Project/
├── godot/                  11 个独立的 Godot 工程，每个对应一个模组
│   └── <character>-skin-<name>/     kebab-case 命名
│       ├── project.godot
│       ├── export_presets.cfg
│       ├── animations/      Spine 运行时资源（.skel / .atlas / .png）
│       ├── images/          UI 与角色立绘
│       ├── scenes/          场景（.tscn）
│       └── src/             C# 脚本
└── spine/                  10 个 Spine 源工程，PascalCase 命名
    └── <Character>Skin_<Name>/
        ├── *.spine          可编辑的 Spine 工程文件
        ├── pic/             原始切图
        └── build_pic/       导出用切图
```

`godot/` 与 `spine/` 是**两套命名风格**，且并非一一对应（见下表）。

---

## 模组清单

| # | Godot 工程目录 | 模组 ID / Spine 目录 | 游戏内名称 | 角色 | 版本 |
|---|---|---|---|---|---|
| 1 | `defect-skin-goldenglow-summerflowers` | `DefectSkin_Goldenglow_SummerFlowers` | 【故障机器人皮肤】澄闪（夏卉） | 故障机器人 | 0.5.0 |
| 2 | `fool-skin-naiyu` | `FoolSkin_Naiyu` / *（无 Spine 源）* | 【愚者皮肤】奶愚 | 愚者 | 0.1.0 |
| 3 | `fool-skin-tocka` | `FoolSkin_Tocka` | 【愚者皮肤】冻河 | 愚者 | 0.2.0 |
| 4 | `ironclad-skin-erii` | `IroncladSkin_Erii` | 【铁甲战士皮肤】绘梨衣 | 铁甲战士 | 0.1.0 |
| 5 | `ironclad-skin-talulah` | `IroncladSkin_Talulah` | 【铁甲战士皮肤】塔露拉 | 铁甲战士 | 0.8.0 |
| 6 | `ironclad-skin-talulah-fighter` | `IroncladSkin_Talulah_Fighter` | 【铁甲战士皮肤】塔露拉（斗士） | 铁甲战士 | 0.5.0 |
| 7 | `ironclad-skin-talulah-operator` | `IroncladSkin_Talulah_Operator` | 【铁甲战士皮肤】塔露拉（干员） | 铁甲战士 | 0.3.0 |
| 8 | `necro-skin-scene-unseenterrain` | `NecroSkin_Scene_UnseenTerrain` | 【亡灵契约师皮肤】稀音（新地形） | 亡灵契约师 | 0.4.0 |
| 9 | `regent-skin-rosmontis-anairydream` | `RegentSkin_Rosmontis_AnAiryDream` | 【储君皮肤】迷迭香（轻盈一梦） | 储君 | 0.6.0 |
| 10 | `silent-skin-cantabile` | `SilentSkin_Cantabile` | 【静默猎手皮肤】晓歌 | 静默猎手 | 0.5.0 |
| 11 | `silent-skin-typhon` | `SilentSkin_Typhon` | 【静默猎手皮肤】提丰 | 静默猎手 | 0.2.0 |

模组 ID 同时是游戏 `mods/` 下的目录名，以及 `.pck` / `.json` 的文件名前缀。

> `fool-skin-naiyu` 没有对应的 Spine 源工程，其美术资源直接在 Godot 工程内。

---

## 构建前必须知道的事

### 1. 需要自备游戏程序集（本仓库不含）

各工程的 `.tscn` 通过 `ext_resource type="Script"` 引用 `res://src/Core/Nodes/**.cs`，这些脚本是 **GDRE 从游戏 `sts2.dll` 反编译**得到的。

`src/` 下的 C# 源码属于 `MegaCrit.Sts2.*` 命名空间，是游戏本体程序集的产物，**本仓库随附这些 .cs 仅为让 Godot 编辑器能正确挂载场景脚本**。它们全部列在各工程的 `export_presets.cfg` → `export_filter="exclude"` 排除名单里，**不会被打进 `.pck`**。

编译或深入修改请自行准备：

```
<游戏安装目录>/data_sts2_windows_x86_64/sts2.dll       # 程序集
<游戏安装目录>/data_sts2_windows_x86_64/sts2.xml       # XML 文档注释（查 API 用）
```

用 GDRE Tools（Godot RE Tools）自行反编译即可重新生成 `src/` 下的脚本。

### 2. 需要自装 spine-godot GDExtension

仓库**不含** `spine_godot_extension.gdextension`。游戏本体自带 `libspine_godot.windows.template_release.x86_64.dll`，但 Godot 编辑器打开本工程时仍需可用的 spine-godot 插件，否则 `.skel` / `.atlas` 导入会失败（报 `Failed to find gdextension libraries for plugin res://bin/spine_godot_extension.gdextension`）。

### 3. `.godot/` 不在仓库里，首次打开需重新导入

本仓库通过 `.gitignore` 排除了所有 `.godot/` 目录（原本占 149 MiB）。首次用 Godot 打开某个工程时会重新导入资源，耗时属正常现象。

### 4. 部分工程脚本为占位文件

以下两个工程的 `src/**.cs` 全部为 **1 字节空文件**（GDRE 导出时未生成内容）：

- `necro-skin-scene-unseenterrain`（8 个）
- `regent-skin-rosmontis-anairydream`（8 个）

这些工程的 `.pck` 能正常导出（脚本已在排除名单中），但**在编辑器里打开场景时脚本会报错**。要修复就按上文第 1 条自行反编译后覆盖。

---

## 构建与部署

1. 安装 **Godot 4.5 .NET 版**（普通版无法处理 C# 工程）。
2. 用 Godot 打开某个 `godot/<project>/project.godot`，等待资源导入完成。
3. 确认 `export_presets.cfg` 中 `Windows Desktop` 预设可用（`project.godot` 的 `config/features` 已是 `4.5` / `Forward Plus`）。
4. 导出 `.pck`，命名规则为 `<模组ID>.pck`。
5. 在游戏 `mods/<模组ID>/` 下放置两个文件：

```
mods/<模组ID>/<模组ID>.pck
mods/<模组ID>/<模组ID>.json
```

`.json` 清单格式（以 `SilentSkin_Typhon` 为例）：

```json
{
  "id": "SilentSkin_Typhon",
  "name": "【静默猎手皮肤】提丰",
  "author": "星见千夜 @Bilibili",
  "description": "将原版静默猎手模型及部分美术资源替换为《明日方舟》中的提丰。",
  "version": "0.2.0",
  "min_game_version": "0.111.0",
  "has_pck": true,
  "has_dll": false,
  "dependencies": [],
  "affects_gameplay": false
}
```

创意工坊发布使用 `.sts2-workshop-uploader/` 工作流（该目录为本地发布配置，未纳入本仓库）。

---

## 已知问题

- `silent-skin-typhon` 的 `project.godot` 中 `config/name` 曾误写为 `SilentSkin_Cantabile`（从 Cantabile 工程复制所致），现已修正为 `SilentSkin_Typhon`。
- Godot 4.4+ 的脚本 UID 在不同工程间可能重复（同名 `.cs` 被多个工程引用）。当前 11 个工程的 `.tscn` 内 UID 与本地 `.cs.uid` **已全部对齐**；若你本地打开时出现 uid mismatch 警告，让编辑器重写一次 UID 即可。

---

## 版权与免责声明

- 角色美术资产版权归 **Hypergryph（鹰角网络） / 《明日方舟》** 所有。
- 游戏本体程序集、反编译所得 C# 源码版权归 **MegaCrit（《杀戮尖塔 2》）** 所有。
- 本仓库为**二次创作与学习交流用途**，不含游戏本体内容，不用于任何商业目的。
- 如版权方提出异议，将立即删除相关内容。

---

## License

本仓库**不提供开源许可**。代码与美术资源权利归属如上，除作者原创部分外不得再分发或商用。
