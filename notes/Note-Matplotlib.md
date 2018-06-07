---
layout: page
title: Matplotlib
category: note
tags: Python
---

* content
{:toc}

# Basic

Import

```python
from matplotlib import pyplot as plt
```

Build figure

```python
fig = plt.figure(1)
fig = plt.figure(1, figsize=(10,10))	# set figure size
```

Tighten the layout

```python
fig.tight_layout()
```

Build subplots

```python
ax = plt.subplot(111)
ax = plt.subplot(211) # build two subplots and select the left one
ax = plt.subplot(111, projection='polar') # build polar subplot
```

Draw graphs

```python
ax.plot()
ax.bar()
ax.hist()
ax.scatter()
ax.plot_date()
```

Show figure

```python
fig.show()
```

Clear figure

```python
fig.clf()
```

Save figure

```python
plt.savefig('path/name.png')
```

Legend & Label & Tick & Grid

```python
# title
ax.set_title('plot', fontsize=20)

# label
ax.set_xlabel('Threshold (m/s)')
ax.set_ylabel('Strom periods (hours)')

# ticks
ax.set_xticks(np.arange(0, 1.1, 0.1))
ax.set_yticks(np.arange(0, 1.1, 0.1))
ax.set_xticklabels(labels, size=9, rotation=15)

# axis limits
plt.xlim(0, 1)
# or
ax.set_xlim(0, 1)

# grid
ax.grid(True)
ax.grid(False)
ax.yaxis.grid(True)

# legend
ax.legend(loc='lower left', frameon=False)
```

Two y-axis

```python
ax1 = plt.subplot(111)
ax1.plot(X1, Y, c='blue', label="X1")
ax2 = ax1.twinx()
ax2.plot(X2, Y, c='orange', label="X2")
```

Circle

```python
circle = plt.Circle((x, y), radius, linestyle='solid', fill=False)
ax.add_artist(circle)
```

Annotate

```python
ax.annotate('annotation',
	xy=(xx, yy),
    xycoords='data',
    xytext=(-30, -50),
    textcoords='offset points',
    fontsize=16,
    arrowprops=dict(arrowstyle="->",
        connectionstyle="arc3,rad=.2"
        )
    )
```

# 3D

```python
from mpl_toolkits.mplot3d import Axes3D
ax = fig.add_subplot(111, projection='3d')
ax.scatter(XX, YY, ZZ, c=c, marker=m)
ax.plot_wireframe(XX, YY, ZZ)
ax.plot_surface(XX, YY, ZZ)
```

# Animation

Import
```python
from matplotlib.animation import FuncAnimation
```

Example 1
```python
fig, ax = plt.subplots()
line, = ax.plot(np.random.rand(10))
ax.set_ylim(0, 1)

def update(data):
    line.set_ydata(data)
    return line,

def data_gen():
    while True:
        yield np.random.rand(10)

anim = FuncAnimation(fig, update, data_gen, interval=100, frames=300, blit=True)

anim = FuncAnimation(fig,
    update, # 反复调用改变图形的函数
    data_gen,   # 数据生成器
    init_func=init, # 图像初始化
    frames=360, # 动画长度，一次循环包含的帧数，update函数中要传入frame
    interval=100,   # 更新频率，以ms计
    blit=True   # True=更新所有点，False=仅更新产生变化的点，但mac须选False
    )
plt.show()
```

Example 2
```python
fig = plt.figure(1)
ax = fig.add_subplot(111, projection='3d')

x, y, z = np.random.rand(3)
ax.scatter(x, y, z)

def update(*arg):
    x, y, z = np.random.rand(3)
    ax.scatter(x, y, z)

anim = FuncAnimation(fig, update)
fig.show()
```


Rotating view
```python
def animate(frame):
    ax.view_init(elev=10., azim=frame)
```

Example 3

```python
n = 5
XX, YY, ZZ = np.random.rand(3, n)
fig = plt.figure(1)
ax = fig.add_subplot(111, projection='3d')
ax.grid(False)
fig.tight_layout()
    
def init():
    ax.set_xlim(1.1 * min(XX), 1.1 * max(XX))
    ax.set_ylim(1.1 * min(YY), 1.1 * max(YY))
    ax.set_zlim(1.1 * min(ZZ), 1.1 * max(ZZ))
    ax.patch.set_facecolor('#FFFFFF')
    ax.set_axis_off()
    ax.scatter(XX, YY, ZZ, c='#002755')
    for i in range(n):
        for j in range(i+1,n):
            ax.plot((XX[i], XX[j]), 
                (YY[i], YY[j]), 
                (ZZ[i], ZZ[j]), 
                c='#A49EA7',
                linewidth=0.8)
    return ax,

def update(frame):
    ax.view_init(elev=10., azim=frame)
    return ax,

anim = FuncAnimation(fig, update, init_func=init, interval=10, frames=360)
```



Save

```python
anim.save('anim.mp4', writer='ffmpeg', fps=30, dpi=40)
anim.save('anim.gif', writer='imagemagick', fps=30, dpi=40)
```

# Basemap

import
```python
# import
from mpl_toolkits.basemap import Basemap
```

build projection
```python
m = Basemap(llcrnrlon=-93.536379,
	llcrnrlat=16.179933,
    urcrnrlon=-82.002156,
    urcrnrlat=26.132693,
    resolution='i',
    projection='tmerc',
    lon_0=-87.0,
    lat_0=0.0
    )
```

project lon/lat to xx/yy
```python
xx, yy = m(lon, lat)
df['xx'] = df.apply(lambda x: m(x['lon'], x['lat'])[0], axis=1)
df['yy'] = df.apply(lambda x: m(x['lon'], x['lat'])[1], axis=1)
```

draw map background
```python
m.drawcoastlines(color='0.50', linewidth=0.6, zorder=0)
m.fillcontinents(color='#f2efe8', alpha=0.8, zorder=0)
m.drawmapboundary(fill_color='#b7d0cd', zorder=0)
```

