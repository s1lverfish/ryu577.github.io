---
layout: post
comments: true
title:  "Museum of dancing math"
date:   2019-03-24 16:34:40 -0800
categories: jekyll update
---


To create a bouncy sphere or a wavy sphere, run 

```python
from pyray.shapes.sphere import *
draw_wavy_sphere_wrapper('.\\im', 66, 1)
```

<img src="https://s2.gifyu.com/images/WavySphere.gif" 
alt="Image formed by above method" width="240" height="240" border="10" />


<img src="https://s2.gifyu.com/images/AndreReflcn.gif" 
alt="Image formed by above method" width="240" height="240" border="10" />



```python
from pyray.shapes.polyhedron import *
basedir = '.\\'
tr = Tetartoid()
for i in range(0, 31):
    im = Image.new("RGB", (2048, 2048), (1,1,1))
    draw = ImageDraw.Draw(im,'RGBA')
    r = general_rotation(np.array([0,1,0]),2*np.pi*i/30)
    tr.render_solid_planes(draw, r, shift=np.array([1000, 1000, 0]), scale=750)
    im.save(basedir + "im" + str(i) + ".png")
```

<a href="https://www.youtube.com/watch?v=OV7c6S32IDU" 
target="_blank"><img src="https://s2.gifyu.com/images/tetartoid2.gif" 
alt="Image formed by above method" width="240" height="240" border="10" /></a>


```python
from pyray.shapes.pointswarm import *
points_to_bins()
```

<a href="https://www.youtube.com/watch?v=OV7c6S32IDU" 
target="_blank"><img src="https://s2.gifyu.com/images/pointswarm.gif" 
alt="Image formed by above method" width="240" height="240" border="10" /></a>


