# VFX Shader Dataset

面向 **2D / 2.5D 平面视效（UI 动效）** 的着色器生成评测数据集，用于训练与评估多 Agent 闭环生成系统（[VFX-Agent](https://github.com/yangfei1223/VFX-Agent)）从视频参考自动生成 Shadertoy 格式 GLSL 着色器的能力。

每个样本由一对文件组成：
- `<name>.webm` — 设计参考视频（视觉目标）
- `<name>.json` — 人工标注的视效元数据（效果类别、形状、颜色、动画、复杂度等）

---

## 数据集统计

| 指标 | 数值 |
|------|------|
| 样本总数 | **50** |
| 视频总大小 | 162.6 MB |
| 平均单样本 | 3.3 MB（最大 7.9 MB，最小 0.1 MB） |
| 范围内样本（is_in_scope） | 50（100%） |
| 2D 样本（is_2d） | 36（72%） |
| 含动画样本（has_animation） | 30（60%） |

### 复杂度分布

| 复杂度 | 数量 | 占比 |
|--------|------|------|
| simple | 19 | 38% |
| medium | 25 | 50% |
| complex | 6 | 12% |

### 动画类型分布

| 动画类型 | 数量 |
|----------|------|
| flow（流动） | 21 |
| none（静态） | 19 |
| pulse（脉冲） | 5 |
| rotate（旋转） | 4 |
| morph（形变） | 1 |

### 填充类型分布

| 填充类型 | 数量 |
|----------|------|
| solid（实心） | 31 |
| mixed（混合） | 14 |
| hollow（空心） | 5 |

---

## 视效分类（9 类，共 50 个样本）

本数据集聚焦于移动端 / Web 平台常见的 UI 视效，按 **effect_category** 分为 9 类。所有类别均为 2D / 2.5D，**排除 3D raymarching、场景渲染与体渲染**。

| 类别 | 数量 | 说明 |
|------|------|------|
| **glow** | 14 | 光晕 / 辉光效果（`exp(-d * intensity)`） |
| **liquid** | 11 | 液态效果（水、玻璃、墨迹、形变流体） |
| **particle** | 10 | 粒子系统（飞溅、星尘、烟花、电浆） |
| **shape** | 4 | 几何形状（SDF 实心 / 空心填充） |
| **space** | 4 | 空间 / 天体效果（极光、黑洞、星系） |
| **gradient** | 3 | 多色渐变背景（线性 / 径向） |
| **warp** | 2 | 域形变（Domain Warp） |
| **ripple** | 1 | 涟漪扩散（`sdCircle` + 正弦波） |
| **special** | 1 | 复合 UI 视效（如液态玻璃 UI） |

### 各类别样本清单

<details>
<summary><b>glow（14）</b> — 光晕 / 辉光</summary>

`buffer-bloom` · `glow-shader-test2` · `glow-tutorial` · `liquid-glass-icon` · `magic-particles` · `multicolored-2d-metaball` · `particles4` · `plasma-waves` · `pulse-circle` · `shiny-circle` · `shiny-rectancle` · `strobe-light-trail` · `total-noob` · `waves-remix`
</details>

<details>
<summary><b>liquid（11）</b> — 液态效果</summary>

`akaeo-ether-liquid-glass` · `color-ripples` · `drive-home` · `flat-water-effects2` · `iterations-inversion2` · `liquid-galss-test` · `rainbow` · `vortex-street` · `warping-procedural2` · `water-color-blending` · `water-dropplet`
</details>

<details>
<summary><b>particle（10）</b> — 粒子系统</summary>

`dna-helis` · `electron` · `happy-diwali-2019` · `reactive-radial-ripples` · `space-rings` · `sparks-drifting` · `sparks-from-fire` · `star-tunnel` · `watch-it-burn` · `windows-95`
</details>

<details>
<summary><b>shape（4）</b> — 几何形状</summary>

`2d-physics-balls` · `glorious-line-algorithm` · `heart-2d` · `twitter-blue-check`
</details>

<details>
<summary><b>space（4）</b> — 空间 / 天体</summary>

`auroras` · `black-hole-with-accretion-risk` · `galaxy3` · `warp-speed2`
</details>

<details>
<summary><b>gradient（3）</b> — 多色渐变</summary>

`4-col-grad` · `color-gradient` · `supah-frosted-glass`
</details>

<details>
<summary><b>warp（2）</b> — 域形变</summary>

`cool-s-distance` · `moon-distance-2d`
</details>

<details>
<summary><b>ripple（1）</b> — 涟漪扩散</summary>

`hypnotic-ripples`
</details>

<details>
<summary><b>special（1）</b> — 复合 UI 视效</summary>

`liquid-glass-ui`
</details>

---

## 主要颜色分布

样本的主色调（dominant_colors）统计（top 15）：

| 颜色 | 出现次数 |
|------|----------|
| Black | 15 |
| Orange | 12 |
| White | 9 |
| Blue | 8 |
| Cyan | 7 |
| Red | 5 |
| Yellow | 4 |
| Green | 3 |
| Deep Blue | 3 |
| Purple | 3 |

---

## 目录结构

```
vfx-shader-dataset/
├── README.md                          # 本文件
└── data/                              # 数据目录（50 个样本）
    ├── 4-col-grad.json                # 元数据
    ├── 4-col-grad.webm                # 设计参考视频
    ├── heart-2d.json
    ├── heart-2d.webm
    ├── ...
    ├── shiny-circle.json
    └── shiny-circle.webm
```

每个样本由 `data/` 下同名的一对 `.json` + `.webm` 文件组成，共 100 个数据文件。

---

## 元数据字段说明（`<name>.json`）

| 字段 | 类型 | 说明 |
|------|------|------|
| `sample_name` | string | 样本名（与文件名一致） |
| `video_file` | string | 视频文件名 |
| `effect_category` | string | 视效类别（9 类之一） |
| `effect_name` | string | 视效名称（英文） |
| `visual_description` | string | 自然语言视觉描述（中文） |
| `dominant_colors` | string[] | 主色调列表 |
| `has_animation` | bool | 是否含动画 |
| `complexity` | string | 复杂度：`simple` / `medium` / `complex` |
| `is_2d` | bool | 是否为 2D / 2.5D |
| `is_in_scope` | bool | 是否在数据集范围内 |
| `scope_reason` | string | 若不在范围内，给出原因 |
| `key_elements` | string[] | 关键视觉元素 |
| `shape_type` | string | 形状类型（`circle` / `heart` / `organic` / `abstract` / `mixed` …） |
| `fill_type` | string | 填充类型：`solid` / `hollow` / `mixed` |
| `animation_type` | string | 动画类型：`none` / `flow` / `pulse` / `rotate` / `morph` |

### 示例

```json
{
  "effect_category": "shape",
  "effect_name": "Gradient Red Heart Icon",
  "visual_description": "图片中心展示了一个红色的心形图案，表面带有从左至右的柔和光影渐变，呈现出立体感。背景是温暖的米色到浅棕色的径向渐变，四周略带暗角效果。",
  "dominant_colors": ["Red", "Beige"],
  "has_animation": false,
  "complexity": "simple",
  "is_2d": true,
  "is_in_scope": true,
  "scope_reason": "",
  "key_elements": ["Heart Shape", "Gradient Fill"],
  "shape_type": "heart",
  "fill_type": "solid",
  "animation_type": "none",
  "sample_name": "heart-2d",
  "video_file": "heart-2d.webm"
}
```

---

## 使用方法

### 1. 直接使用元数据

```python
import json
from pathlib import Path

samples = [json.load(open(f)) for f in Path("data").glob("*.json")]
glow_samples = [s for s in samples if s["effect_category"] == "glow"]
```

### 2. 配合 VFX-Agent 进行评测

参考 [VFX-Agent](https://github.com/yangfei1223/VFX-Agent) 的端到端测试流程：

```bash
# 单样本测试
cd backend && python tests/e2e/test_e2e_single.py heart-2d

# 批量测试（所有样本）
cd backend && python tests/e2e/test_e2e_batch.py --all
```

评分标准：
- **≥ 0.85**：✅ PASS（通过）
- **0.80 – 0.85**：⚠️ ACCEPTABLE（可接受）
- **< 0.80**：❌ FAIL（不可接受）

---

## 评分规则

VFX-Agent 的 Inspect Agent 采用 **8 维度加权评分** 对生成的 shader 进行量化评估。每个维度独立打分（0.0–1.0），加权平均得到 `overall_score`。

### 8 个评分维度

| 维度 | 权重 | 评分要点 |
|------|------|----------|
| **composition** | 0.10 | 整体构图、布局、主体位置 |
| **geometry** | 0.10 | SDF 形状、边缘质量、比例尺寸 |
| **color** | 0.15 | 主色调 RGB、渐变过渡、饱和度 |
| **animation** | 0.10 | 动画节奏、时长、循环方式 |
| **background** | 0.20 | 背景颜色、纹理、严格性检查（权重最高） |
| **lighting** | 0.10 | 光晕、Fresnel、高光、阴影 |
| **texture** | 0.10 | 噪声纹理、颗粒感、细节层次 |
| **vfx_details** | 0.15 | 粒子效果、特殊细节、创新元素 |

> 注：`background` 权重最高（0.20），因为背景错误（如纯白背景偏差）会导致整体效果失败。

### 评分计算公式

**单维度得分**（基于检查项）：

```
dimension_score = (correct_items / total_check_items) * 0.7
                + (no_problem_items / total_check_items) * 0.3
```

**总分**（加权平均）：

```
overall_score = Σ (dimension_score × dimension_weight) / Σ (weights)
```

### 通过阈值

| overall_score | 状态 | passed |
|---------------|------|--------|
| 0.90 – 1.0 | Excellent | ✅ true |
| 0.85 – 0.90 | Acceptable | ✅ true |
| 0.70 – 0.85 | Needs tweaking | ❌ false |
| 0.50 – 0.70 | Major changes needed | ❌ false |
| 0.00 – 0.50 | No match | ❌ false |

**评测通过的判定标准**：`overall_score ≥ 0.85`。

### 维度评分示例

```json
"dimension_scores": {
  "composition": {"score": 0.9, "notes": "心形居中，比例合适"},
  "geometry":    {"score": 0.6, "notes": "边缘过于锐利，缺少柔和过渡"},
  "color":       {"score": 0.8, "notes": "红色系正确，渐变对比不足"},
  ...
}
```

当某个关键维度（如 `geometry`）评分过低时，即使其他维度较高，总分也会被显著拉低，反映该维度对整体效果的影响。

---

## 评测基线（VFX-Agent v0.5.0，max_iter=3）

| 指标 | 数值 |
|------|------|
| 样本数 | 19 |
| 平均分 | 0.71 |
| 通过率（≥ 0.85） | 26.3% |
| 最高分 | 4-col-grad (0.95) |
| 最低分 | sparks-drifting (0.00，超时) |

完整基线结果详见 [VFX-Agent AGENTS.md](https://github.com/yangfei1223/VFX-Agent/blob/master/AGENTS.md)。

---

## 版权与许可

本数据集的参考视频均来自 [Shadertoy](https://www.shadertoy.com/) 社区的公开作品，仅供学术研究与算法评测使用。

- 视频版权归 [Shadertoy](https://www.shadertoy.com/) 上对应作品的原作者所有
- 如需用于商业用途，请先取得原作者授权
- 元数据（JSON）采用 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 许可

若您是某段视频的作者并认为内容不应收录，请提 Issue 联系删除。

---

## 引用

如本数据集对您的研究有帮助，请引用：

```bibtex
@misc{vfx-shader-dataset,
  author       = {Fei Yang},
  title        = {VFX Shader Dataset: 2D UI 视效着色器生成评测数据集},
  year         = {2026},
  url          = {https://github.com/yangfei1223/vfx-shader-dataset},
}
```

---

*最后更新：2026-07-01*
