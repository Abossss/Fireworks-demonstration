# 🎆 Python煙花效果實現教程
<div align="center">

[简体中文](README_CN.md) / 繁体中文 / [English](README.md) / [Deutsch](README_DE.md) / [日本語](README_JP.md)

</div>
這是一個使用Pygame實現的煙花效果程序，包含了逼真的煙花動畫、祝福文字、星空背景和月亮效果。本教程將帶你一步步實現這個有趣的項目。

## 項目概述

本項目實現了以下功能：
- 🎇 隨機生成的煙花效果
- ✨ 帶有漸變和消散效果的粒子系統
- 💝 隨機顯示的中文祝福語
- 🌟 帶有閃爍效果的星空背景
- 🌕 逼真的月亮效果
- 🖱️ 滑鼠點擊互動功能
- 🎁 月亮彩蛋功能

## 技術原理

### 🔮 粒子系統原理
粒子系統是實現煙花效果的核心技術，主要包括以下幾個方面：
1. **粒子生命週期管理**：每個粒子都有一個生命週期計數器，當生命週期結束時，粒子會被銷毀。
2. **物理運動模擬**：使用速度向量和角度控制粒子的運動軌跡。
3. **顏色漸變**：通過調整粒子的alpha通道值實現淡入淡出效果。
4. **大小變化**：粒子大小隨時間逐漸減小，增加真實感。

### 💥 煙花爆炸效果
煙花爆炸效果的實現基於以下原理：
1. **初始速度分佈**：使用極坐標系統，隨機生成360度範圍內的初始速度向量。
2. **爆炸形狀控制**：支持圓形、星形和球形三種煙花形狀。
3. **聲音同步**：爆炸時播放隨機選擇的音效，增強視聽效果。
4. **文字粒子效果**：使用粒子系統模擬文字顯示，實現漸變消散效果。

### 🌙 月亮渲染實現
月亮效果的實現包含以下特點：
1. **圖片載入**：使用PNG格式的月亮圖片，保持透明度。
2. **互動彩蛋**：滑鼠懸停在月亮區域觸發特殊文字煙花，顯示「彩蛋！」文字。

## 🛠️ 環境配置

### 1. Python環境要求
- Python 3.8+
- pip包管理器

### 2. 安裝依賴
```bash
pip install pygame==2.5.0
```

### 3. 資源文件準備
1. **音效文件**：
   - explosion.mp3：主爆炸音效
   - explosion2.mp3：次爆炸音效
   - explosion3.mp3：尾聲音效

2. **圖片文件**：
   - moon.png：月亮圖片
   - logo.ico：程序圖標

## 📝 核心類設計

### 1. 遊戲配置（GameConfig）
```python
@dataclass
class GameConfig:
    SCREEN_WIDTH: int = info.current_w
    SCREEN_HEIGHT: int = info.current_h
    FPS: int = 60
    FIREWORK_SPAWN_RATE: float = 0.05
    TEXT_SPAWN_RATE: float = 0.2
```

### 2. 粒子系統（Particle）
```python
class Particle:
    def __init__(self, x: float, y: float, color: Tuple[int, int, int], 
                 angle: Optional[float] = None, is_text: bool = False):
        self.speed = random.uniform(0.3, 1.5) if is_text else random.uniform(2, 8)
        self.angle = angle if angle is not None else random.uniform(0, 2 * math.pi)
        self.lifetime = random.randint(100, 140) if is_text else random.randint(40, 80)
```

### 3. 煙花類（Firework）
```python
class Firework:
    def __init__(self):
        self.pattern = random.choice(['circle', 'star', 'sphere'])
        self.show_text = True if ResourceManager._moon_clicked else random.random() < GameConfig.TEXT_SPAWN_RATE
        self.target_explosion_height = random.uniform(min_height, max_height)
```

## 🎮 互動功能說明

1. **滑鼠點擊發射**
   - 點擊螢幕任意位置發射煙花
   - 煙花從底部升起，在點擊的X坐標位置爆炸

2. **月亮彩蛋**
   - 懸停月亮區域觸發特殊煙花
   - 特殊煙花會顯示「彩蛋！」文字
   - 從月亮位置發射，營造獨特效果

## ⚡ 性能優化說明

### 1. 粒子系統優化
- 文字粒子採用較低的採樣率（0.3）減少粒子數量
- 使用不同的生命週期管理文字和普通粒子
- 粒子大小和透明度平滑過渡

### 2. 渲染優化
- 使用SRCALPHA表面處理透明效果
- 分層渲染保證視覺效果

### 3. 內存優化
- 及時清理消失的煙花和粒子
- 使用dataclass減少內存佔用
- 資源文件統一管理

## ❓ 常見問題解答

### 1. 性能問題
Q: 運行時卡頓？
A: 嘗試以下解決方案：
- 降低TEXT_SPAWN_RATE值
- 減少FIREWORK_SPAWN_RATE值
- 降低粒子採樣率

### 2. 顯示問題
Q: 煙花效果顯示不完整？
A: 檢查以下幾點：
- 確保螢幕解析度設置正確
- 檢查窗口大小是否合適
- 驗證資源文件是否完整

## 📄 許可證

本項目採用GNU General Public License v3.0（GNU GPL v3）許可證。這意味著：

- 你可以自由地使用、修改和分發本代碼
- 如果你分發修改後的版本，必須同樣採用GNU GPL v3許可證
- 你必須公開源代碼
- 你必須標明原始作者和修改內容
- 不提供任何擔保