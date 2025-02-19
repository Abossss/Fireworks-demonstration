# 🎆 Python Firework Effect Implementation Tutorial
<div align="center">

[简体中文](README_CN.md) / [繁体中文](README_TC.md) / English / [Deutsch](README_DE.md) / [日本語](README_JP.md)

</div>
This is a firework effect program implemented using Pygame, featuring realistic firework animations, blessing texts, starry sky background, and moon effects. This tutorial will guide you step by step through this interesting project.

## Project Overview

This project implements the following features:
- 🎇 Randomly generated firework effects
- ✨ Particle system with gradient and dissipation effects
- 💝 Randomly displayed Chinese blessing texts
- 🌟 Starry sky background with twinkling effects
- 🌕 Realistic moon effect
- 🖱️ Mouse click interaction
- 🎁 Moon hover easter egg (special fireworks triggered when hovering over the moon area)

## Technical Principles

### 🔮 Particle System Principles
The particle system is the core technology for implementing firework effects, mainly including the following aspects:
1. **Particle Lifecycle Management**: Each particle has a lifecycle counter, and when the lifecycle ends, the particle is destroyed.
2. **Physics Motion Simulation**: Using velocity vectors and angles to control particle motion trajectories.
3. **Color Gradient**: Implementing fade-in and fade-out effects by adjusting the particle's alpha channel value.
4. **Size Change**: Particle size gradually decreases over time, increasing realism.

### 💥 Firework Explosion Effect
The firework explosion effect is implemented based on the following principles:
1. **Initial Velocity Distribution**: Using a polar coordinate system to randomly generate initial velocity vectors within a 360-degree range.
2. **Explosion Shape Control**: Supports three firework shapes: circular, star, and spherical.
3. **Sound Synchronization**: Plays randomly selected sound effects during explosion to enhance audiovisual experience.
4. **Text Particle Effect**: Using particle system to simulate text display with gradient dissipation effects.

### 🌙 Moon Rendering Implementation
The moon effect implementation includes the following features:
1. **Image Loading**: Using PNG format moon image with transparency.
2. **Interactive Easter Egg**: Special text fireworks triggered when hovering over the moon area.

## 🛠️ Environment Setup

### 1. Python Environment Requirements
- Python 3.8+
- pip package manager

### 2. Install Dependencies
```bash
pip install pygame==2.5.0
```

### 3. Resource File Preparation
1. **Sound Effects**:
   - explosion.mp3: Main explosion sound
   - explosion2.mp3: Secondary explosion sound
   - explosion3.mp3: Finale sound

2. **Image Files**:
   - moon.png: Moon image
   - logo.ico: Program icon

## 📝 Core Class Design

### 1. Game Configuration (GameConfig)
```python
@dataclass
class GameConfig:
    SCREEN_WIDTH: int = info.current_w
    SCREEN_HEIGHT: int = info.current_h
    FPS: int = 60
    FIREWORK_SPAWN_RATE: float = 0.05
    TEXT_SPAWN_RATE: float = 0.2
```

### 2. Particle System (Particle)
```python
class Particle:
    def __init__(self, x: float, y: float, color: Tuple[int, int, int], 
                 angle: Optional[float] = None, is_text: bool = False):
        self.speed = random.uniform(0.3, 1.5) if is_text else random.uniform(2, 8)
        self.angle = angle if angle is not None else random.uniform(0, 2 * math.pi)
        self.lifetime = random.randint(100, 140) if is_text else random.randint(40, 80)
```

### 3. Firework Class (Firework)
```python
class Firework:
    def __init__(self):
        self.pattern = random.choice(['circle', 'star', 'sphere'])
        self.show_text = True if ResourceManager._moon_clicked else random.random() < GameConfig.TEXT_SPAWN_RATE
        self.target_explosion_height = random.uniform(min_height, max_height)
```

## 🎮 Interaction Features

1. **Mouse Click Launch**
   - Click anywhere on the screen to launch fireworks
   - Fireworks rise from the bottom and explode at the clicked X coordinate

2. **Moon Easter Egg**
   - Hover over the moon area to trigger special text fireworks
   - Special effects with unique particle patterns

## 🌈 Special Effects

1. **Particle Effects**
   - Gradient color changes
   - Size reduction over time
   - Alpha channel fade-out

2. **Background Effects**
   - Twinkling stars
   - Realistic moon glow
   - Dynamic night sky

## 🎯 Future Improvements

1. **Performance Optimization**
   - Particle pool implementation
   - Graphics rendering optimization
   - Memory usage optimization

2. **Feature Enhancements**
   - More firework patterns
   - Custom text input
   - Multiple language support

## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.