title: Unity Shaders and Effects Cookbook 翻译 第一章 Diffuse Shading
date: 2015-5-23 11:35:00
categories:
- 翻译
tags:
- Shader
---

<h2>Chapter 1:  Diffuse Shading</h2>
This chapter will cover some of the more common diffuse techniques found in todays Game
Development Shading Pipelines. You will learn about:
本章覆盖了一些通用的漫反射技术，这些着色器广泛应用于当今的游戏。你可以学到以下知识
Creating a basic Surface Shader
Adding properties to a Surface Shader
Using properties in a Surface Shader
Creating a custom diffuse lighting model
Creating a Half Lambert lighting model
Creating a ramp texture to control diffuse shading
Creating a faked BRDF using a 2D ramp texture
创建一个基础的表面着色器
添加属性到表面着色器
在表面着色器中使用属性
创建一个自定义的漫反射光照模型
创建一个Half Lambert 光照模型
使用ramp texture来控制漫反射着色器
通过 2D ramp texture来模拟BRDF 
<h2>Introduction
介绍</h2>
The beginning of any good Shader always relies on having a foundational diffuse component or lighting model. So it always makes sense to start the Shader writing process with the diffuse component of the Shader.
若要写出好的着色器，则必须从基础的漫反射光照模型开始。让我们通过漫反射来迈入着色器的大门。
Previously in computer graphics, diffuse shading was done with what was called the fixed function lighting model. It gave graphics programmers just a single lighting model that they could tweak. using a set of parameters and textures. In our current industry. we have access to much more control and flexibility with Cg, and especially in Unity with its Surface Shaders.
对老旧的计算机显卡来说，漫反射 是通过固定功能光照模型来实现的。它只允许图形程序员通过一系列参数和纹理来调整一个光照模型。在当今的计算机产业里，我们通过cg语言可以对显卡拥有更多的控制和灵活性，特别是在Unity的表面着色器语言里。
The diffuse component of a Shader basically describes the way light reflects off a surface in all directions. That might sound very similar to the description of how a reflective mirror works. but it is actually different. A reflective surface actually reflects the image of the surrounding environment. while diffuse lighting takes all the light from light sources. such as the sun. and reflects its light back to the viewer’s eye. We will be covering reflections in a later chapter. but for our purposes right now. this will help us differentiate between the two.
着色器的漫反射功能从根本上解释了灯光是如何在各个角度影响物体表面的，这听起来很像是在描述镜面反射的原理，不过二者确实有所不同。镜面反射实际上反映的是周围的环境，而漫反射则需要处理所有来自光源的光线，比如太阳，并反射回观察者的眼睛。我们会在之后的章节里介绍反射，不过现在这将帮助我们区分二者。
To achieve a basic diffuse lighting model. we will have to create a Shader that will include an emissive color. an ambient color. and the total light accumulated from all light sources. The following recipes show you how to build up a complete diffuse lighting model, and also show some various industry tricks that come in handy for creating more complicated diffuse models using only textures.
为了为了实现yi’g一个基本的漫反射光照模型，我们要先创建一个着色器，其中包含了自发光和环境光，以及所有从光源发射出的光线。下面的方法将展示如何创建一个完整的漫反射光照模型，并介绍一些广泛使用的技巧，用于在使用一张纹理的时候创建复杂的漫反射着色器。
By the end of this chapter you will have learned how to build basic Shaders that perform basic operations Armed with this knowledge, you will be able to create just about any Surface Shader.
在本章的最后，你将学会如何用这些知识来创建一个基础的着色器，还可以创建任何表面着色器。
<h2>Creating a basic Surface Shader
创建一个基础的表面着色器</h2>
As we progress further into the recipes in this book, it is important that you know how to set up your workspace in Unity. so that you can work efficiently, and without any pain. If you are already quite familiar with creating Shaders and setting up Materials in Unity 4. you may skip this recipe. It is here to ensure that newcomers to surface shading in Unity 4 can work with the rest of the recipes.
通过本书的进一步学习，你必须先懂得如何设置Unity的工作环境。这样会使你更有效率开心的工作。如果你已经非常熟悉如何在Unity里面创建一个着色器和设置材质，那你可以跳过这些步骤，我写下这些只是为了确保新人可以正确的学习下面的知识。
Getting ready
To get started with this recipe. you will need to have Unity 4 running, and must have created a new project. There will also be a Unity project included with this cookbook, so you can use that one as well and simply add your own custom Shaders to it, as you step through each recipe. With that completed. you are now ready to step into the wonderful world of real-time shading!
为了开始本章的学习，你必须先有一份可以运行的Unity4并且创建一个新项目。本书附带了一份Unity工程，你可以使用它，这样可以方便的在后面的学习里添加自定义着色器。当你完成了以上操作后，你就拥有了一个美丽的实时着色器世界。
How to do it…
如何开始
Before getting into our first Shader. let’s create a small scene for us to work with. This can be done by going to GameObject | Create Other in the Unity editor. From there you can create a plane, to act as a ground. a couple of spheres. to which we will apply our Shader. and a directional light to give the scene some light. With our scene generated. we can move onto the Shader writing steps:
在开始我们的第一个着色器之前，让我们创建一个工作用的小场景，点击GameObject | Create Other 菜单，然后选择创建Plane ，来充当地面，然后创建一对Spheres，用于演示Shader。然后创建一个directional light 来点亮场景，这样场景就创建完成了，接下来我们讲解创建Shader的步骤：
1. In the Project tab in your Unity editor. right-click on the Assets folder and select create l Folder.
在Project面板里右键创建一个文件夹
【if you are using the Unity project that came with the cookbook. you can skip to step 4.】
如果你已经使用了本书附带的Unity项目，请跳过步骤4
2. Rename the folder that you created to Shaders by right-clicking on it and selecting Rename from the drop-down list, or by selecting the folder and hitting F2 on the keyboard.
将文件夹重命名为Shaders
3. Create another folder and rename it to Materials.
创建一个名为Materials的文件夹
4. Right-click on the Shaders folder and select create l Shader. Then right-click on th Materials folder and select create l Material.
在Shaders文件夹里面创建一个Shader，并且在Materials文件夹里创建一个material
5. Rename both the Shader and the Material to Basicbiffuse.
将二者都命名为Basicbiffuse
6. Launch the Baslcbiffuse Shader into Mononevelop (the default script editor for Unity) by double-clicking on it This will automatically launch the editor for you and display the Shader code.
用Mononevelop 打开Baslcbiffuse Shader。
【You will see that Unity has already populated our Shader with some basic code. This, by default, will get you a basic diffuse Shader that accepts one texture. We will be modifying this base code so that we can learn how to quickly start developing our own custom Shaders.】
【你会发现Unity已经自动在我们的着色器里面填充了一些基础代码，这些代码会帮你创建一个基础的仅用一张纹理的漫反射着色器，我们将在此之上开发，这样就可以更快的学习如何创建一个着色器】
7. Now let’s give our Shader a custom folder from which it’s selected. The very first line of code in the Shader is the custom description we have to give the Shader so that Unity can make it available in the Shader drop-down list when assigning to Materials. We have renamed our path to Shader “Cookbookshaders/BasicDiffuse “, but you can name it to whatever you want and can rename it at any time. So don’t worry about any dependencies at this point. Save the shader in MonoDevelop and return to the Unity editor. Unity will automatically compile the Shader when it recognizes that the file has been updated. This is what your Shader should look like at this point:
现在让我们把着色器放到之前的文件夹里。着色器代码的第一行是我们给这个着色器的自定义描述，用于Unity决定这个着色器出现在材质菜单的哪个位置，将之重命名为”Cookbookshaders/BasicDiffuse ” ，不过你可以在任何时候将之修改为任何你想要的名字。不用担心会有东西依赖于它，在MonoDevelop 里保存文件并返回到Unity里面，它会自动识别是否有更新过并进行编译。现在我们的第一个着色器就像下面的代码一样。

{% codeblock lang:c# %}
Shader "CookbookShaders/BasicDiffuse"{
Properties {
_MainTex ("Base (RGB)", 2D) = "white" {}
}
SubShader {
Tags { "RenderType"="Opaque" }
LOD 200
 
CGPROGRAM
#pragma surface surf Lambert
sampler2D _MainTex;
struct Input {
float2 uv_MainTex;
};
void surf (Input IN, inout SurfaceOutput o) {
half4 c = tex2D (_MainTex, IN.uv_MainTex);
o.Albedo = c.rgb;
o.Alpha = c.a;
}
ENDCG
}
FallBack "Diffuse"
}
{% endcodeblock %}
8. Select the Material called Basicoiffuse that we created in step 4 and look at the Inspector tab. From the Shader drop-down list, select Cookbookshaders l Basicniffuse (your Shader path might be different if you chose to use a different path name). This will assign your Shader to your material and now make it ready for you to assign to an object.
在Inspector 选中Basicoiffuse 材质，在右边的Shader列表里选中我们刚创建的Cookbookshaders l Basicniffuse （如果你更改过shader的Path路径，这里将会不一样），然后就可以将这个shader赋值到材质上去，这个材质已经准备好可以在对象上使用了。
[To assign a material to an object, you can simply click-and-drag your Material from the Project tab to the object in your scene. You can also drag a Material onto the Inspector tab of an 1 object, within the Unity editor, to assign a Material. ]
【你可以通过简单的拖拽材质到项目面板里的一个对象上来赋予材质，也可以拖拽到Inspector面板里的对象上】
{% asset_img image00.png %}
Not much to look at, at this point, but our Shader development environment is set up and we can now start to modify the Shader to suit our needs.
到此为止我们的开发环境已经设置完毕，现在可以按照我们的需求修改着色器了。