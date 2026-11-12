# voxel-portfolio-hero

单文件等距体素（isometric voxel）作品集 Hero 页：Canvas 上数百枚小方块循环坍塌、散落，再重组成下一个形态。零构建、零依赖（唯一外链为 Google Fonts），双主题（亮/暗），支持 `prefers-reduced-motion` 与移动端安全区。

## 仓库结构

- `index.html` — **重制版（推荐）**：「夜航」主题。靛夜蓝 + 青瓷绿 + 琥珀配色，Noto Serif SC + Fraunces + JetBrains Mono 字体，画布居左，含跑马灯横幅、三卡作品区、三步流程；形态循环为拱桥 → 双峰雪山 → 浮空宝石；鼠标透镜为**向内引力扭曲**（悬停时方块被吸向指针，暖琥珀光晕）。
- `voxel-portfolio-hero/` — 原版 skill 包（SKILL.md + references/shapes.md + assets/template.html），米纸+朱红「Phantom Epic」主题，形态为塔楼 → PE 字标 → 云朵。

## 修改说明（重制版 vs 原版）

| 维度 | 原版 | 重制版 |
|---|---|---|
| 配色 | 米纸 `#F5F2E9` / 朱红 `#B83A26` | 靛夜蓝 `#1A2340` / 青瓷 `#0E7C7B` / 琥珀 `#D98E2B` |
| 字体 | Cormorant Garamond / Pinyon Script / IBM Plex Mono | Noto Serif SC / Fraunces / Manrope / JetBrains Mono |
| 布局 | 画布居右 | 画布居左（移动端置顶） |
| 板块 | 4 卡 2 列 + 4 步流程 | 跑马灯 + 3 卡 3 列 + 3 步圆角卡 |
| 形态 | 塔楼 / PE 字标 / 云朵 | 拱桥 / 双峰雪山 / 浮空宝石 |
| 透镜 | 向外放大扭曲（冷色） | 向内引力扭曲（暖琥珀） |
| 节奏 | H=3.4s / M=2.7s | H=3.0s / M=2.4s |

动画引擎（等距投影、坍塌重组机制）为通用技术实现，重制版重写了全部视觉表达层。

## 用法

直接浏览器打开 `index.html` 即可。改内容看注释与文案节点；换形态参考 `voxel-portfolio-hero/references/shapes.md` 的配方格式（体素为 `[x,y,z,色键]`，色键定义在脚本顶部 `PAL`）。

## License

MIT
