---
layout: post_latex
title: 现代抗锯齿技术——PPAA中的新星SMAA
tags: ['computer graphics']
published: true
---

## PPAA

所谓PPAA（Post Processing Anti Aliasing），是基于后处理的抗锯齿技术。在PPAA之前，主流AA技术的是MSAA（MultiSamples AA）、SSAA（SuperSamples AA），SSAA是AA中最暴力也是最完美的解决方案，而MSAA是与硬件紧密结合的built-in AA。对于forward rendering来说，MSAA几乎是唯一的选择。

然而，MSAA这种古老的、built-in的技术，已经不太能满足现代渲染器的需求了。它有两大问题，一是MSAA会有多余的AA计算，二是MSAA不适用于deferred rendering。

鉴于MSAA的不足，PPAA就蓬勃发展起来了。PPAA强大之处在于可以自定义、且硬件无关，所以基于PPAA的算法非常多。而其中的翘楚，SMAA，性能以及AA质量都很不错，且适用于defer框架。所以本文着重介绍SMAA。

<!--more-->

## SMAA

### 边缘检测

锯齿问题体现在图像上几何物体的边缘处，也就是说，如果能准确地post process出图像上哪些地方是边，哪些地方不是。检测过少，锯齿边就会残留；检测过多，图像就会糊。为来更好地提升AA质量，SMAA边缘检测算法的选取非常关键。

相比基于normal map、depth map，基于颜色的边缘检测尤佳。一是因为，颜色信息容易获得，而深度图／法线图相对难获得，例如对于图像处理领取，用户提供的只有照片而已；二是因为它还有一个优点：对于做了shading后才产生的锯齿，也一样能处理（例如有梯度的toon shading）。

SMAA首推的是基于Luma（亮度）的边缘检测算法。
 
1. vertex shader，根据纹理坐标输出3组offset：

```c
    vec4 SMAA_RT_METRICS = vec4(1.0 / imgSize.x, 1.0 / imgSize.y, imgSize.x, imgSize.y);
    Offsets[0] = fma(SMAA_RT_METRICS.xyxy, vec4(-1.0, 0.0, 0.0, -1.0), texCoord.xyxy); // Left / Top Edge
    Offsets[1] = fma(SMAA_RT_METRICS.xyxy, vec4( 1.0, 0.0, 0.0,  1.0), texCoord.xyxy); // Right / Bottom Edge
    Offsets[2] = fma(SMAA_RT_METRICS.xyxy, vec4(-2.0, 0.0, 0.0, -2.0), texCoord.xyxy); // Leftx2 / Topx2 Edge
```


![smaa1.png](../images/2017.10/smaa1.png)

2. fragment shader，先求出fragment的luma值：

```c
    // Calculate lumas:
    float3 weights = float3(0.2126, 0.7152, 0.0722);
    float L = dot(texture(colorTex, texcoord).rgb, weights);
```

（RGB->luma的公式来自wiki https://en.wikipedia.org/wiki/Relative_luminance  ）


    float Lleft = dot(texture(colorTex, offsets[0].xy).rgb, weights);
    float Ltop  = dot(texture(colorTex, offsets[0].zw).rgb, weights);