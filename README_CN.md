# 🎆 Python烟花效果实现教程
<div align="center">

简体中文 / [繁体中文](README_TC.md) / [English](README.md) / [Deutsch](README_DE.md) / [日本語](README_JP.md)

</div>
这是一个使用Pygame实现的烟花效果程序，包含了逼真的烟花动画、祝福文字、星空背景和月亮效果。本教程将带你一步步实现这个有趣的项目。

## 项目概述

本项目实现了以下功能：
- 🎇 随机生成的烟花效果
- ✨ 带有渐变和消散效果的粒子系统
- 💝 随机显示的中文祝福语
- 🌟 带有闪烁效果的星空背景
- 🌕 逼真的月亮效果
- 🖱️ 鼠标点击互动功能
- 🎁 月亮悬停彩蛋功能（鼠标悬停月亮区域触发特殊烟花）

## 技术原理

### 🔮 粒子系统原理
粒子系统是实现烟花效果的核心技术，主要包括以下几个方面：
1. **粒子生命周期管理**：每个粒子都有一个生命周期计数器，当生命周期结束时，粒子会被销毁。
2. **物理运动模拟**：使用速度向量和角度控制粒子的运动轨迹。
3. **颜色渐变**：通过调整粒子的alpha通道值实现淡入淡出效果。
4. **大小变化**：粒子大小随时间逐渐减小，增加真实感。

### 💥 烟花爆炸效果
烟花爆炸效果的实现基于以下原理：
1. **初始速度分布**：使用极坐标系统，随机生成360度范围内的初始速度向量。
2. **爆炸形状控制**：支持圆形、星形和球形三种烟花形状。
3. **声音同步**：爆炸时播放随机选择的音效，增强视听效果。
4. **文字粒子效果**：使用粒子系统模拟文字显示，实现渐变消散效果。

### 🌙 月亮渲染实现
月亮效果的实现包含以下特点：
1. **图片加载**：使用PNG格式的月亮图片，保持透明度。
2. **交互彩蛋**：鼠标悬停月亮区域触发特殊文字烟花。

## 🛠️ 环境配置

### 1. Python环境要求
- Python 3.8+
- pip包管理器

### 2. 安装依赖
```bash
pip install pygame==2.5.0
```

### 3. 资源文件准备
1. **音效文件**：
   - explosion.mp3：主爆炸音效
   - explosion2.mp3：次爆炸音效
   - explosion3.mp3：尾声音效

2. **图片文件**：
   - moon.png：月亮图片
   - logo.ico：程序图标

## 📝 核心类设计

### 1. 游戏配置（GameConfig）
```python
@dataclass
class GameConfig:
    SCREEN_WIDTH: int = info.current_w
    SCREEN_HEIGHT: int = info.current_h
    FPS: int = 60
    FIREWORK_SPAWN_RATE: float = 0.05
    TEXT_SPAWN_RATE: float = 0.2
```

### 2. 粒子系统（Particle）
```python
class Particle:
    def __init__(self, x: float, y: float, color: Tuple[int, int, int], 
                 angle: Optional[float] = None, is_text: bool = False):
        self.speed = random.uniform(0.3, 1.5) if is_text else random.uniform(2, 8)
        self.angle = angle if angle is not None else random.uniform(0, 2 * math.pi)
        self.lifetime = random.randint(100, 140) if is_text else random.randint(40, 80)
```

### 3. 烟花类（Firework）
```python
class Firework:
    def __init__(self):
        self.pattern = random.choice(['circle', 'star', 'sphere'])
        self.show_text = True if ResourceManager._moon_clicked else random.random() < GameConfig.TEXT_SPAWN_RATE
        self.target_explosion_height = random.uniform(min_height, max_height)
```

## 🎮 交互功能说明

1. **鼠标点击发射**
   - 点击屏幕任意位置发射烟花
   - 烟花从底部升起，在点击的X坐标位置爆炸

2. **月亮彩蛋**
   - 鼠标悬停在月亮区域触发特殊烟花
   - 特殊烟花会显示"彩蛋！"文字



## ⚡ 性能优化说明

### 1. 粒子系统优化
- 文字粒子采用较低的采样率（0.3）减少粒子数量
- 使用不同的生命周期管理文字和普通粒子
- 粒子大小和透明度平滑过渡

### 2. 渲染优化
- 使用SRCALPHA表面处理透明效果
- 分层渲染保证视觉效果

### 3. 内存优化
- 及时清理消失的烟花和粒子
- 使用dataclass减少内存占用
- 资源文件统一管理

## ❓ 常见问题解答

### 1. 性能问题
Q: 运行时卡顿？
A: 尝试以下解决方案：
- 降低TEXT_SPAWN_RATE值
- 减少FIREWORK_SPAWN_RATE值
- 降低粒子采样率

### 2. 显示问题
Q: 烟花效果显示不完整？
A: 检查以下几点：
- 确保屏幕分辨率设置正确
- 检查窗口大小是否合适
- 验证资源文件是否完整

## 📄 许可证

本项目采用GNU General Public License v3.0（GNU GPL v3）许可证。这意味着：

- 你可以自由地使用、修改和分发本代码
- 如果你分发修改后的版本，必须同样采用GNU GPL v3许可证
- 你必须公开源代码
- 你必须标明原始作者和修改内容
- 不提供任何担保