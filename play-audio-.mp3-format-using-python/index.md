# 使用 python 播放音频（.mp3 格式）


## 第一种方式 playsound

### 1. 安装依赖包

```bash
pip install playsound
```

### 2. 使用

```python
from playsound import playsound

playsound('your music file path')
print('music play finished')

```

## 第二种方式 pygame.mixer.music（推荐使用）

推荐理由，用这个库可以控制播放的音乐，比如暂停，停止，调节声音大小等。

### 1. 安装依赖包

```bash
pip install pygame
```

### 2. 使用

```python
from ast import While
import pygame

"""初始化 pygame"""
pygame.mixer.init()

"""加载音乐文件"""
pygame.mixer.music.load('your music file path')

"""设置音量"""
pygame.mixer.music.set_volume(0.2)

"""播放音乐"""
pygame.mixer.music.play()

"""播放完成再结束"""
while pygame.mixer.music.get_busy():
    pass
print('播放结束')

```
