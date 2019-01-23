---
title: 用python写出动听的歌声
---

## 准备
1. 一首歌的谱子
2. 所有要用到音阶的声音频率
3. python环境

## 行动
1. 首先我们需要一个生成正弦波的函数
```python
def wv(t=0,f=0,v=0.5,wf=ff,sr=8000):
    '''  
    t:写入时长
    f:声音频率
    v：音量
    wf：一个可以写入的音频文件
    sr：采样率
    '''
    tt=0
    dt=1.0/sr

    while tt<=t:
        s=math.sin(tt*math.pi*f)*v*(2**15)#采样，调节音量，映射到[-2^15,2^15)
        s=int(s)
        fd=struct.pack("h",s)#转换成8bit二进制数据
        wf.writeframes(fd)#写入音频文件
        tt+=dt#时间流逝
```
2. 然后我们准备好简谱，然后翻译成对应的声音频率
```python
# 使用到的音阶的频率
note={"1":262,"2":294,"3":330,"4":349,"5":392,"6":440,"7":494,"6-":220,"0":0}
# 简谱
n=[
    "1","2","3","1","1","2","3","1","0",
    "3","4","5","0","3","4","5","0",
    "5","6","5","4","3","1","0","5","6","5","4","3","1","0",
    "2","6-","1","0","2","6-","1"
]
# 音长
tm=[
    2,2,2,2,2,2,2,2,1,
    2,2,2,1.5,2,2,2,2,
    1,1,1,1,2,2,1,1,1,1,1,1,2,1,
    2,2,2,2,2,2,2
]
```

3. 执行
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
import wave
import math
import struct
import matplotlib.pyplot as plt
import numpy as np
ff=wave.open("v1.wav","w")
ff.setframerate(8000)
ff.setnchannels(1)
ff.setsampwidth(2)

for i in range(len(n)):
    wv(tm[i]/4.0,note[n[i]])

ff.close()
```