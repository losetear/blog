title: Unity Shaders and Effects Cookbook《目录》
date: 2015-5-23 00:00:00
categories:
- 翻译
tags:
- Shader
---

<h1>Preface  1
引言 1</h1>

<h1>Chapter 1: Diffuse Shading  7
第一章：漫反射着色器 7</h1>
Introduction  7
介绍 7
Creating a basic Surface Shader  8
创建一个基础的表面着色器 8
Adding properties to a Surface Shader  12
添加属性到表面着色器
Using properties in a Surface Shader  14
在表面着色器中使用属性
Creating a custom diffuse lighting model  17
创建一个自定义的漫反射光照模型
Creating a Half Lambert lighting model  20
创建一个Half Lambert[a] 光照模型
Creating a ramp texture to control diffuse shading  22
使用ramp texture来控制漫反射着色器
Creating a faked BRDF using a 2D ramp texture  24
通过 2D ramp texture来模拟BRDF[b] 
<h1>Chapter 2: Using Textures for Effects  29
第二章：使用纹理 29</h1>
Introduction  29
介绍 29
Scrolling textures by modifying UV values  30
修改UV来滚动纹理 30
Animating sprite sheets  33
让sprite sheets 运动起来 33
Packing and blending textures  39
打包和混合纹理 39
Normal mapping  44
法线贴图 44
Creating procedural textures in the Unity editor  48
在Unity编辑器里创建procedural textures 48
Photoshop levels effect  54
实现Photoshop 的效果 54

<h1>Chapter 3: Making Your Game Shine with Specular  59
第三章：让你的游戏像镜子那样发光  59</h1>
Introduction  59
介绍 59
Utilizing Unity3D’s built-in Specular type  60
使用Unity3D自带的镜面反射Shader 60
Creating a Phong Specular type  62
创建Phong镜面反射类型 62
Creating a BlinnPhong Specular type  66
创建BlinnPhong镜面反射类型 66
Masking Specular with textures  69
用纹理来遮罩镜面反射 69
Metallic versus soft Specular  74
金属类型的镜面反射 74
Creating an Anisotropic Specular type  79
创建一个各向异性镜面反射类型 79

<h1>Chapter 4:Reflecting Your World 87
第四章：反射你的世界 87</h1>
Creating cubemaps in Unity3D 88
在Unity3D里创建一个立方贴图 88
Simple Cubemap reflection in Unity3D 93
简单的立方贴图反射 93
Masking reflections in Unity3D 96
Unity3D里的遮罩反射96
Normal maps and reflections in Unity3D 100
Unity3D 里的法线贴图和反射 100
Fresnel reflections in Unity3D 104
Unity3D 里的菲涅耳反射[c]
Creating a simple dynamic Cubemap system 108
创建一个简单的动态立方贴图 108

<h1>Chapter 5:Lighting Models 113
第五章：光照模型 113</h1>
introduction 113
介绍 113
The Lit sphere lighting model 114
被照亮的球体光照模型 114
The diffuse convolution lighting model 119
漫发射叠加光照模型 119
Creating a vehicle paint lighting model 125
创建交通工具的绘制光照模型 125
Skin shader 130
皮肤着色器 130
Cloth shading 137
布料着色器 137

<h1>Chapter 6: Transparency 143
第六章：透明 143</h1>
introduction 143
介绍 143
Creating transparency with alpha 143
用alpha控制透明度 143
Transparent cutoff shader 146
透明裁剪着色器 146
Depth sorting with render queues 148
用渲染对列控制深度排序 148
GUI and transparency 151
用在Gui上的透明 151

<h1>Chapter 7:Vertex Magic 159
第七章：顶点魔法 159</h1>
introduction 159
介绍 159
Accessing a vertex color in a Surface Shader 160
在表面着色器里访问顶点颜色 160
Animating vertices in a Surface Shader 164
在表面着色器里运动顶点 164
Using vertex color for terrains 168
在地形上使用顶点色 168

<h1>Chapter 8: Mobile Shader Adjustment 173
第八章：手机着色器调整 173</h1>
introduction 173
介绍 173
What is a cheap Shader? 174
什么是快速的着色器 174
Profiling your Shaders 179
分析你的着色器 179
Modiflying your shaders for mobile 185
为了手机修改你的着色器 185

<h1>Chapter 9: Making Your Shader World Modular with Cglncludes 191
第九章：用Cglncludes 让你的着色器世界开发变得更加模块化 191</h1>
introduction 191
介绍 191
Cginclude files that are built into Unity 192
Unity自带的Cginclude文件 192
Creating a cginclude file to store lighting models 195
创建一个cginclude文件来保存光照模型 195
Building shaders with #define directives 199
用#define 指令来建立着色器 199

<h1>Chapter 10:Screen Effects with Unity Render Textures 203
第十章：用Unity渲染纹理实现屏幕效果 203</h1>
Introduction 203
介绍 203
Setting up the screen effects script system 204
设置屏幕效果的脚本系统 204
Brightness, saturation, and contrast with screen effects 213
用屏幕效果调整亮度、饱和度以及对比度 213
Basic Photoshop-like blend modes with screen effects 218
像Photoshop那样混合模型和屏幕效果 218
The Overlay blend mode with screen effects 224
在模型上覆盖屏幕效果 224

<h1>Chapter 11: Gameplay and Screen Effects 229
第十一章：游戏性和屏幕效果 229</h1>
Introduction 229
介绍 229
Creating an old movie screen effect 230
创建一个旧电影屏幕效果 230
Creating a night vision screen effect 239
创建一个夜视仪效果 239

<h1>Index 249
索引249</h1>
[a]Ivan Zhang:
这个算法会加强暗部的亮度
[b]Ivan Zhang:
BRDF 双向反射分布函数 参见wiki http://en.wikipedia.org/wiki/Bidirectional_reflectance_distribution_function
[c]Ivan Zhang:
参见 Wiki http://zh.wikipedia.org/wiki/%E8%8F%B2%E6%B6%85%E8%80%B3%E6%96%B9%E7%A8%8B
[d]Ivan Zhang:
表面着色器