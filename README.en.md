<p align="center"><a href="README.md">中文</a> | English</p>

<p align="center">
  <a href="https://store.steampowered.com/app/2868840"><img src="https://img.shields.io/badge/Game-Slay%20the%20Spire%202-B3202E" alt="Game"></a>
  <a href="https://godotengine.org"><img src="https://img.shields.io/badge/Godot-4.5%20%28.NET%29-478CBF?logo=godotengine&amp;logoColor=white" alt="Godot"></a>
  <a href="https://esotericsoftware.com"><img src="https://img.shields.io/badge/Spine-3.8.75%20%2F%204.2.x-FF7A00" alt="Spine"></a>
</p>

A collection of skin mods for *Slay the Spire 2* from the Steam Workshop collection "何忆卫" (Heyiwei), plus additional standalone mods, including the original Godot projects and Spine sources.

The collection draws assets from multiple works; see the list below for details.

This repository contains the **production projects** (Godot projects + Spine source files), not ready-to-use releases. To just play, head to the [Steam Workshop](https://steamcommunity.com/profiles/76561199036683806/myworkshopfiles/?appid=2868840), or download directly from the [Releases](https://github.com/HoshimiSenya/sts2-mod-hyw/releases) page.

- Engine: Godot **4.5** (.NET / Mono), Forward Plus renderer

---

## Repository Layout

```
Project/
├── godot/                  11 standalone Godot projects, one per mod
│   └── <character>-skin-<name>/     kebab-case naming
│       ├── project.godot
│       ├── export_presets.cfg
│       ├── animations/      Spine runtime assets (.skel / .atlas / .png)
│       ├── images/          UI and character art
│       ├── scenes/          Scenes (.tscn)
│       └── src/             C# scripts
└── spine/                  10 Spine source projects, PascalCase naming
    └── <Character>Skin_<Name>/
        ├── *.spine          Editable Spine project files
        ├── pic/             Original sliced images
        └── build_pic/       Sliced images for export
```

`godot/` and `spine/` use **two different naming conventions** and do not map one-to-one (see the table below).

---

## Mod List

| # | Godot project directory | Mod ID / Spine directory | In-game name | Asset source | Version |
|---|---|---|---|---|---|
| 1 | `defect-skin-goldenglow-summerflowers` | `DefectSkin_Goldenglow_SummerFlowers` | 【故障机器人皮肤】澄闪（夏卉） | Arknights | 0.5.0 |
| 2 | `fool-skin-naiyu` | `FoolSkin_Naiyu` / *(no Spine source)* | 【愚者皮肤】奶愚 | Milk Frog | 0.1.0 |
| 3 | `fool-skin-tocka` | `FoolSkin_Tocka` | 【愚者皮肤】冻河 | Arknights | 0.2.0 |
| 4 | `ironclad-skin-erii` | `IroncladSkin_Erii` | 【铁甲战士皮肤】绘梨衣 | The Gate of Cassell | 0.1.0 |
| 5 | `ironclad-skin-talulah` | `IroncladSkin_Talulah` | 【铁甲战士皮肤】塔露拉 | Arknights | 0.8.0 |
| 6 | `ironclad-skin-talulah-fighter` | `IroncladSkin_Talulah_Fighter` | 【铁甲战士皮肤】塔露拉（斗士） | Arknights | 0.5.0 |
| 7 | `ironclad-skin-talulah-operator` | `IroncladSkin_Talulah_Operator` | 【铁甲战士皮肤】塔露拉（干员） | Arknights | 0.3.0 |
| 8 | `necro-skin-scene-unseenterrain` | `NecroSkin_Scene_UnseenTerrain` | 【亡灵契约师皮肤】稀音（新地形） | Arknights | 0.4.0 |
| 9 | `regent-skin-rosmontis-anairydream` | `RegentSkin_Rosmontis_AnAiryDream` | 【储君皮肤】迷迭香（轻盈一梦） | Arknights | 0.6.0 |
| 10 | `silent-skin-cantabile` | `SilentSkin_Cantabile` | 【静默猎手皮肤】晓歌 | Arknights | 0.5.0 |
| 11 | `silent-skin-typhon` | `SilentSkin_Typhon` | 【静默猎手皮肤】提丰 | Arknights | 0.2.0 |

The mod ID is also the directory name under the game's `mods/` folder, and the filename prefix for `.pck` / `.json`.

> `fool-skin-naiyu` has no corresponding Spine source project; its art assets live directly inside the Godot project.

---

## Before Building

### 1. Install the spine-godot GDExtension separately

This repository **does not** include `spine_godot_extension.gdextension`. The game ships `libspine_godot.windows.template_release.x86_64.dll`, but the Godot editor still needs a working spine-godot plugin to open these projects; otherwise `.skel` / `.atlas` imports fail with `Failed to find gdextension libraries for plugin res://bin/spine_godot_extension.gdextension`.

### 2. The Spine version must be converted

**The Spine assets in the two directories are not the same version**, which is the easiest trap when modifying art assets:

| Location | Spine version | Notes |
|---|---|---|
| `spine/**.spine` | **3.8.75** | Editable source projects; open with Spine 3.8.75 |
| `godot/**.skel` `**.atlas` | **4.2.x** (mostly 4.2.00) | Runtime data; must match the spine-godot extension bundled with the game |

The game's Spine runtime is **4.2.x**, so the full pipeline is:

```
Edit .spine project in Spine 3.8.75  ->  export .skel / .atlas
    ->  convert to 4.2.x  ->  drop into the Godot project
```

Mismatching in either direction fails: opening a 3.8.75 `.spine` with a newer Spine prompts a one-way project upgrade, while dropping 3.8.75 export output straight into Godot breaks the importer.

After editing assets, update the new `.spine` (3.8.75) and the converted `.skel` / `.atlas` (4.2.x) **as a pair** - never just one side.

### 3. Some projects contain placeholder scripts

In the following two projects, every `src/**.cs` file is a **1-byte empty file**:

- `necro-skin-scene-unseenterrain` (8 files)
- `regent-skin-rosmontis-anairydream` (8 files)

Their `.pck` still exports fine (the scripts are on the export exclusion list), but opening scenes in the editor will report script errors.

---

## Build and Deploy

1. Install **Godot 4.5 .NET** (the standard build cannot handle C# projects).
2. Open a project with Godot: `godot/<project>/project.godot`, and wait for asset import to finish.
3. Make sure the `Windows Desktop` preset in `export_presets.cfg` is usable (the `config/features` in `project.godot` are already `4.5` / `Forward Plus`).
4. Export the `.pck`, named `<ModID>.pck`.
5. Place two files under the game's `mods/<ModID>/`:

```
mods/<ModID>/<ModID>.pck
mods/<ModID>/<ModID>.json
```

The `.json` manifest format (example: `SilentSkin_Typhon`):

```json
{
  "id": "SilentSkin_Typhon",
  "name": "【静默猎手皮肤】提丰",
  "description": "将原版静默猎手模型及部分美术资源替换为《明日方舟》中的提丰。",
  "version": "0.2.0",
  "min_game_version": "0.111.0",
  "has_pck": true,
  "has_dll": false,
  "dependencies": [],
  "affects_gameplay": false
}
```

---

## Known Issues

- Script UIDs can collide across projects in Godot 4.4+ (the same-named `.cs` is referenced by multiple projects). All 11 projects currently have their `.tscn` UIDs aligned with the local `.cs.uid` files; if a uid mismatch warning appears locally, letting the editor rewrite the UID once is enough.

---

## Copyright and Disclaimer

- Art assets in each mod remain the property of their respective rights holders:
  - Assets from *Arknights* belong to Hypergryph.
  - Assets from *The Gate of Cassell* belong to their respective rights holder.
  - The Milk Frog assets belong to their original author.
- The game assembly and the decompiled C# source code belong to MegaCrit.
- This repository is for **derivative fan work and learning/exchange purposes**. It contains no game content and is not used for any commercial purpose.
- If a rights holder objects, the relevant content will be removed immediately.

---

## License

This repository does not provide an open-source license. Rights to the code and art assets are as described above; apart from the author's original work, no redistribution or commercial use is permitted.
