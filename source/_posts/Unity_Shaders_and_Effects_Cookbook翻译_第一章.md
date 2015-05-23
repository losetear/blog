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

<h1>How it works…
工作原理</h1>
Unity has made the task of getting your Shader environment up and running, very easy for you. It is simply a matter of a few clicks and you are good to go. There are a lot of elements working in the background, with regard to the Surface Shader itself. Unity has taken the Cg Shader language and made it more efficient to write, by doing a lot of the heavy Cg code lifting for you. The Surface Shader language is a more component-based way of writing Shaders. Tasks such as processing your own texture coordinates and transformation matrices have already been done for you, so you don’t have to start from scratch any more. In the past, we would have to start a new Shader and rewrite a lot of code over and over again. As you gain more experience with Surface Shaders, you will naturally want to explore more of the underlying functions of the Cg language and how Unity is processing all of the low-level graphics processing unit (GPU) tasks for you.
Unity已经帮你准备好了着色器环境，只是几次简单的点击就可以实现。为了运行表面着色器，有很多元素在后台支撑着它。Unity使用了Cg做为着色器语言，为了使之更方便书写，在后台写了大量的Cg代码。表面着色器是一个基于组件的着色器语，诸如纹理坐标和转置矩阵之类的功能已经准备好了，因此你不必从头开始了。在以前，你为了实现一个着色器，必须重复书写很多代码。在你已经非常熟悉表面着色器之后，你自然会想要探索更多未知的幕后，比如cg语言和Unity是如何在底层的GPU上执行低级图形运算的。
So, by simply changing the Shader’s path name to a name of our choice, we have got our basic diffuse Shader working in the Unity environment, working with lights and shadows and all that, by just changing one line of code!
因此，通过简单的更改着色器路径名，我们已经拥有了一份可以在Unity下运行的基础漫反射着色器，并且灯光和阴影正常工作。而这一切，只是改变一行代码而已！
<h2>See also
参考</h2>
For more information on where to find a large portion of the built-in Cg functions for Unity. go to your Unity install directory and navigate to Unity4\Edittor\Data\CGIncludes. Within that folder there are three files that are of note at this point. the UnityCG.cginc. Lighting.cginc. and UnityShaderVariables.cginc. Our current Shader is making use of all these files at the moment.
下面讲解如何找到Unity自带的Cg代码，在你Unity安装目录下的Unity4\Edittor\Data\CGIncludes.里，有三个文件是需要注意的，UnityCG.cginc Lighting.cginc. 和 UnityShaderVariables.cginc。我们目前的着色器正在使用这三个文件里的功能。
We will go more in-depth with Cglnclude files in Chapter 9. Making Your Shader World Modular with Cglncludes.
我们会在第九章深入讲解Cglnclude文件里的接口。
<h2>Adding properties to a Surface Shader
在表面着色器里加入属性</h2>
Properties of a Shader are very important to the Shader pipeline, as they are the method you use to let the artist or user of the Shader assign textures. and tweak your Shader values. Properties allow you to expose GUI elements in a Material’s Inspector tab without you having to use a separate editor. which provides visual ways to tweak a Shader.
着色器的属性在渲染管道过程里是很重要的，因为它们是让艺术家或使用者分配纹理或调整着色器值的接口。属性可以做为GUI属性暴露在材质的检视面板上，而不需要单独的设置，它提供了可视化的方法供你调整着色器。
With your Shader opened in MonoDevelop. look at the block of lines 3 through 6. This is called the Properties block. Currently. it will have one property in it called _MainTex. If you look at your Material that has this Shader applied to it. you will notice that there is one texture GUI element in the Inspector tab. These lines of code, in our Shader. is creating this GUI element for us.
用MonoDevelop打开你的着色器，看第三行到第六行，这些就是着色器的属性，目前我们只有一个名为_MainTex的属性，如果你查看对应的材质，你会注意到检视面板上有个对应的纹理GUI元素，这个是依赖于代码里的属性自动生成的。
Again. Unity has made this process very efficient in terms of coding and the amount of time it takes to iterate through changing your properties
再次声明，Unity通过代码很有效率的实现了这个功能，它通过遍历属性来完成这个过程。
<h2>How to do it…
怎么去实现</h2>
Let’s see how this works in our current Shader called BasicDiffuse. by creating our own properties and learning more about the syntax involved:
让我们通过创建自己的着色器属性，来了解所涉及到的语法在BasicDiffuse着色器里是如何工作的。
1. In our Properties block of our Shader. remove the current property by deleting the following code from our current Shader.
1.在我们的着色器属性里，删除下面的属性代码
{% codeblock lang:c# %}
Main’I‘ex (“Base (RG8) ” , 20) = “white”
{% endcodeblock %}
2. Now enter the following code. save the Shader. and re-enter the Unity editor.
2.然后输入下面的代码，并保存着色器，然后再次打开Unity编辑器
{% codeblock lang:c# %}
_EmissiveColor (“Emissive Color”, Color) = (1,1,1,1)
{% endcodeblock %}
3. when you return to Unity. the Shader will compile and you will see that our Materials Inspector tab now has a color swatch. named Emlsslve color. instead of a texture swatch. Let‘s add one more and see what happens. Enter the following code:
3.当你返回到Unity时，着色器已经完成了编译，并且你可以看到在材质检视面板上出现了一个名为Emlsslve color的颜色样本，它替换掉了之前的纹理样本。接下来再次输入下面的代码：
{% codeblock lang:c# %}
_AmbientColor (“Ambient Color”, Range(0,10)) = 2
{% endcodeblock %}
4. We have added another Color Swatch to the MateriaI’s Inspector tab. Now let’s add one more to get a feel for other kinds of properties that we can create. Add the following code to the properties block:
我们已经在材质面板上添加另一个颜色属性，现在让我们来添加其他类型的属性，请输入下面的代码：
{% codeblock lang:c# %}
_MySliderValue (“This is a Slider”, Range(0,10)) = 2.5
{% endcodeblock %}
5. We have now created another GUI element that allows us to visually interact with our Shader. This time we created a slider with the name This is a Slider. as shown in the following screenshot:
现在你可以看到面板上出现了另一种GUI元素，这次我们创建的是一个滑动条，就像你在屏幕上看到的那样：
{% asset_img image11-300x101.png %}
Properties allow you to create a visual way to tweak Shaders without having to change values in the Shader code itself.
属性可以让你创建一个可视化面板来调整你的着色器，而不需要去更改代码。
<h2>How it works…
它是如何工作的</h2>
Every Unity Shader has a built-in structure it is looking for in its code. The properties block is one of those functions that is expected by Unity. The reason behind this is to give you. the Shader programmer. a means of quickly creating GUI elements that tie directly into your Shader code. These properties that you declare in the properties block can then be used in your Shader code to change values. colors. and textures
每一个Unity着色器都拥有如下的内置结构，这些属性块则是Unity所希望带给你的功能之一。之所以这样做，是希望给你，给图形程序员一种快捷的方式来创建Gui元素并和你的着色器代码绑定。这些你申明的属性块可以在接下来的代码里用于调整颜色或者纹理。
{% asset_img image09.png %}
Let’s take a look at what is going on underneath the hood here. when you first start writing a new property. you will need to give it a Variable Name. The variable name is going to be the name that your Shader code is going to use to get the value from the GUI element. This saves us a lot of time because we don‘t have to set up that system ourselves.
让我们来看看Unity引擎在背后都做了那些工作，当你开始添加一个新的属性后，你必须赋予它一个名字，而这个名字就是你的着色器代码从Gui面板获取属性的方法，这节省了我们大量的时间，因为我们不需要亲自去完成这个系统。
The next elements of a property are the Inspector GUI Name and the type of the property. which is contained within parentheses. The Inspector GUI Name is the name that is going to appear in the Materials Inspector tab when the user is interacting with and tweaking the Shader. The Type is the type of data that this property is going to control. There are many types that we can define for properties inside of Unity Shaders. The following table describes the types of variables we can have in our Shaders:
接下来介绍出现在Gui面板的属性名字和类型，Gui的名字就是出现在材质面板里的属性名字，这是用于调整着色器的，属性的数据类型就是我们控制的。Unity提供了很多类型让我们在着色器里使用，可以参看下面的表格：
<h3>Surface Shader property types</h3>

参数 | 注解
----|------
Range (min. max)|This creates a float property as a slider from the minimum value to the maximum value 创建一个float属性，可以创建一个标注最大最小值的滑动条
Color|This creates a color swatch in the lnepeceor tab that opens up a color picker – (fIoat.float.float.float)定义颜色属性，可以使用颜色采样器UI|
2D|This creates a texture swatch that allows a user to drag a texture into the Shader创建纹理属性，可以直接拖拽一个纹理进去
Rect|This creates a non-power-of-2 texture swatch and functions the same as the 2D GUI element创建一个非2次方的纹理属性，就像2D属性一样
Cube|This creates a cube map swatch in Inspector and allows a user to drag-and-drop a cube map into the Shader创建一个立方贴图属性，你可以直接拖拽立方贴图上去
Float|This creates a float value in Inspector but without a slider创建一个非滑动条的float属性
Vector|This creates a four-float property that allows you to create directions or colors创建拥有四个float的属性，你可以用于标记方向或颜色

Finally, there is the default value. This simply sets the value of this property to the value you place in the code. So. in the example image. the default value for the property named _AmbientColor, which is of the type Color. is set to a value of 1 , 1 , 1 , 1. Since this is a color property expecting a color. which is RGBA or a float4. or r, g, b, a = x, y, z,w this color property. when it is first created. is set to white.
最后，这些属性都拥有默认值，代码里的属性会被快速赋予默认值。因此，在示例里，由于_AmbientColor的类型是Color，所以默认值被设为 1 , 1 , 1 , 1。这四个值代表RGBA 或者float4，或者r, g, b, a = x, y, z,w，当它第一次创建后，就被设置为白色。
<h2>See also
参考</h2>
The properties are documented in the Unity manual at http://docs.unity3d.com/Documentation/Components/SL-Properties.html
这些属性都记录在Unity手册里，网址是http://docs.unity3d.com/Documentation/Components/SL-Properties.html
Using properties in a Surface Shader
在表面着色器里使用属性
Now that we have created some properties. let’s actually hook them up to the Shader so we can use them as tweaks to our Shader and make the material process much more interactive.
现在我们已经创建了一些属性，让我们将之和着色器关联起来，以让我们可以使用属性来调整着色器并让材质获得更多的交互。
We can use the properties‘ values from the MateriaI’s Inspector tab because we have attached a variable name to the property itself. but in the Shader code you have to set a couple things up before you can start calling the value by its variable name.
我们可以在材质面板调整属性，因为已经为属性赋予了一个变量名，不过你要想在着色器代码里通过变量名来获得对应的属性，则必须创建与之相互呼应的另一个变量。
How to do it…
怎么实现
The following steps show you how to use the properties in a Surface Shader:
接下来为你展示如何在表面着色器中使用属性
1. To begin. let’s remove the following lines of code. as we deleted the property called MainTex in the Creating a basic Surface Shader recipe of this chapter:
首先，让我们删除以下代码，这是本章中添加的MainTex 属性。
{% codeblock lang:c# %}
sampler2D _MainTex;
half4 c = tex2D (_MainTex, IN.uv_MainTex);
{% endcodeblock %}
2. Next, add the following lines of code to the Shader. below the  CGPROGRAM line:
然后在CGPROGRAM 标记下面添加以下代码
{% codeblock lang:c# %}
float4 _EmissiveColor;
float4 _AmbientColor;
float _MySliderValue;
{% endcodeblock %}
3. With step 2 complete, we can now use the values from the properties in our Shader. Let’s do this by adding the value from the _EmissiveColor property to the Ambientcolor property. and giving the result of that to the o.Albedo line of code. So. let’s add the following code to the Shader inside the surf function:
当你完成步骤2之后，你就可以在着色器里使用属性值了。让我们将_EmissiveColor 和Ambientcolor 的值想加，然后把计算结果赋值给 o.Albedo 。加入以下代码到surf函数里。
{% codeblock lang:c# %}
void surf (Input IN, inout SurfaceOutput o)
{
float4 c;
c = pow((_EmissiveColor + _AmbientColor), _MySliderValue);
o.Albedo = c.rgb;
o.Alpha = c.a;
}
{% endcodeblock %}
4. Finally, your Shader should look like the following Shader code. If you save your Shader in Monooevelop and re-enter Unity, your Shader will compile. If there were no errors, you will now have the ability to change the ambient and emissive colors of the Material, as well as increase the saturation of the final color by using the slider value.Pretty neat. huh!
完成后的代码应该和下面一样，如果你保存了着色器代码并返回到Unity之后，应该会看到着色器在自动编译。如果没有错误，你现在可以在材质里修改ambient 和emissive 属性的颜色值了，让我们通过滑动条来增加这些属性的饱和度。
{% codeblock lang:c# %}
Shader "CookbookShaders/BasicDiffuse"
{
//We define Properties in the properties block
Properties
{
_EmissiveColor ("Emissive Color", Color) = (1,1,1,1)
_AmbientColor ("Ambient Color", Color) = (1,1,1,1)
_MySliderValue ("This is a Slider", Range(0,10)) = 2.5
}
SubShader
{
Tags { "RenderType"="Opaque" }
LOD 200
CGPROGRAM
#pragma surface surf Lambert
//We need to declare the properties variable type inside of the CGPROGRAM so we can access its value from the properties block.
float4 _EmissiveColor;
float4 _AmbientColor;
float _MySliderValue;
struct Input
{
float2 uv_MainTex;
};
void surf (Input IN, inout SurfaceOutput o)
{
//We can then use the properties values in our shader
float4 c;
c = pow((_EmissiveColor + _AmbientColor), _MySliderValue);
o.Albedo = c.rgb;
o.Alpha = c.a;
}
ENDCG
}
FallBack "Diffuse"
}
{% endcodeblock %}
【The pcw(arg1 , arg2) is a built-ln function that will perform the equivalent math functlon of power. So, argument 1 is the value we want to raise to a power, and argument 2 ls the powet we want to rabe It to.
pcw(arg1 , arg2)是自带的函数，相当于数学函数里的power。代表x的y次方
To flnd out more Information about the pow () function, look to the Cg tutorial. It Is a great free resource that you can use fol Ieamlng more about shading and to get a glossary of all the functions available to you In the Cg shading language:
你可以翻阅Cg教程。这是一份很好的教程，你可以查阅各种着色器能够使用的函数名。
http://http.developer.nvidia.com/CgTutorial/cg_tutorial_appendix_e.html】
The following screenshot demonstrates the result obtained by using our properties to control our Materials colors and saturation. from within the Material’s Inspector tab:
下面这个截图展示了在材质面板调整颜色和饱和度之后的样子
{% asset_img image06.png %}
when you declare a new property in the property block. you are providing a way for the Shader to retrieve the tweaked value from the Materials Inspector tab. This value is stored in the variable name portion of the property. In this case. _AmbienColor. _EmissiveColor. and _MySliderValue are the variables in which we are storing the tweaked values. In order for you to be able to use the value in the subshader{ } block. you need to create three new variables with the same names as the property‘s variable name. This automatically sets up a link between these two so they know they have to work with the same data. Also. it declares the type of data we want to store in our subshader variables. which will come in handy when we look at optimizing Shaders in a later chapter.
当你在属性区申明了一个新属性之后，着色器就提供了一种从材质面板获得调整后的属性值的方法，这些值保存在对应的变量名里。在我们的着色器里_AmbienColor. _EmissiveColor.
和 _MySliderValue这些变量保存着你调整后的值。为了能在subshader里使用这些值，你需要创建三个和属性一样名字的变量，unity会自动将二者关联在一起，它们拥有同样的数据。同样，这些变量的数据类型将会在后面的优化章节里用到。
Once you have created the subshader variables. you can then use the values in the surf function. In this case we want to add the _EmissiveColor and _AmbientColor variables together and take it to a power of whatever the _MySliderValue variable is equal to in the Material’s Inspector tab.
当你在subshader里创建好变量后，你就可以在surf函数里使用它们了。在这个例子里，我们希望把_EmissiveColor 和 _AmbientColor 以及_MySliderValue 变量通过power函数叠加在一起。
We have now created the foundation for any Shader you will create that requires a diffuse component.
现在我们创建了一个任何着色器都可以使用的漫反射函数。
<h2>Creating a custom diffuse lighting model
创建自定义漫反射光照模型</h2>
Using Unity’s built-in lighting functions is all well and good. but you will quickly outgrow these and want to create a lot more custom lighting models Speaking from experience. we have never worked on a project that has used just the built-in Unity lighting functions and called it good. We would create custom lighting models for just about everything This would allow us to do things such as produce rim lighting effects. more Cubemap-based types of lightings. or even control over how your Shaders react to gameplay. as seen in Shaders that control force fields
Unity自带的光照函数已经很棒了，不过你需要通过创建大量自定义光照模型来获得进步。我们永远不能说一个只使用自带光照模型的项目是个好项目。我们希望创建自定义光照模型来实现各种效果，比如它允许我们实现边缘高亮，以及更多的基于立方贴图的光照。甚至控制你的着色器如何对游戏进行反馈，显然它可以控制一切。
This recipe will focus on creating our own custom diffuse lighting model that we can use to modify and create a number of different effects.
下面让我们来看看如何创建自定义光照模型，并用它创建出各种各样的效果。
<h2>How to do it…
如何实现</h2>
Using the basic diffuse Shader we created in the last recipe. let’s modify it again by performing the following steps:
使用我们在前面创建的着色器，让我们通过下面的步骤来修改它：
1. Let’s modify the #pragma statement to the following code:
将#pragma标记修改如下
{% codeblock lang:c# %}
#pragma surface surf BasicDiffuse
{% endcodeblock %}
2. Add the following code to the subshader:
加入下面代码
{% codeblock lang:c# %}
inline float4 LightingBasicDiffuse (SurfaceOutput s, fixed3 lightDir, fixed atten)
{
float difLight = max(0, dot (s.Normal, lightDir));
float4 col;
col.rgb = s.Albedo * _LightColor0.rgb * (difLight * atten * 2);
col.a = s.Alpha;
return col;
}
{% endcodeblock %}
3. Save the Shader in MonoDevelop and return to Unity. The Shader will compile. and if everything went well. you will see that no real visible change has happened to our Material. What we have done is removed the connection to the built-in Unity diffuse lighting and created our own lighting model that we can customize.
保存着色器代码并返回Unity窗口，着色器会自动编译。如果没问题的话你就可以看到材质已经发送了变化，我们前面所有的工作都是为了实现删除Unity自带的漫反射光照并且实现了一个自定义光照模型。
<h2>How it works…
如何工作</h2>
There are definitely a lot of elements working here. so let’s try to break it down piece by piece and learn why this works in the way that it does:
可以清楚的看到上面我们加入了很多代码，现在让我们一行行来解释它是如何工作的：
The #pragma surface directive tells the Shader which lighting model to use for its calculation. It worked when we first created the Shader because Lambert is a lighting model defined in the Lighting.cginc file. So it was able to use this on creation. We have now told the Shader to look for a lighting model by the name BaseDiffuse.
pragma指令告诉着色器将使用哪个光照模型来计算。在我们创建的第一个着色器代码里，默认使用Lighting.cginc文件里包含的Lambert 光照模型，所以可以计算光照。现在我们告诉着色器将使用名叫BaseDiffuse的光照模型。
Creating a new lighting model is done by declaring a new lighting model function.Once you have done that, you simply replace the function’s name with a name of your choice. For example. LightingName becomes Lighting< Your Chosen Name>  
通过创建一个新的光照模型函数就能实现光照了，当你完成这个步骤后，可以将函数名替换成你想要的名字。函数名的格式是：Lighting<任何名字>
There are three types of lighting model functions that you can use:
你能够使用三种格式的光照模型函数
half4 LightingName (surfaceoutput s, half3 lightDir,half atten){}
This function is used for forward rendering when the view direction is not needed.
上面这个函数用于不需要视角方向的前向渲染
ha1f4 LightingName (surfaceoutput s, ha1f3 lightDir, half3 viewDir, half atten){}
This function is used in forward rendering when a view direction is needed.
这个函数用于需要视角方向的前向渲染
ha1f4 LightingName _PrePass (surfaceoutput s, half4 lignt}
This function is used when you are using deferred rendering for your project.
此函数用于延迟渲染
The dot product function is another built-in mathematical function in the Cg language. We can use it to compare the directions of two vectors in space. The dot product
checks whether two vectors are either parallel to each other or perpendicular. By gving the dot product function, for two vectors you will get a float value in the range
of -1 no 1: where -1 is parallel and has the vector facing away from you, 1 is parallel and has the vector facing toward you, and 0 is completely perpendicular to you.
dot函数是cg语言的另一个内置函数，我们可以用它来比较两个向量在空间里的方向。dot函数会检查两个向量是互相平行还是垂直。任意两个向量都可以通过dot函数获得一个范围，区间是 -1到1，-1就是平行表示向量远离你，1代表朝向你的向量，0表示和你垂直。
The vector dot product (or inner product) of the normalized vectors N and L is a measure of the angle between the two vectors. The smaller the angle between the vectors. the greater the dot-product value will be. and the more incident light the surface will receive.
归一化的向量间的dot计算是估算夹角的一种方法，夹角越小，则dot值越大，并且表面可以接收到更多的入射光
<h2>Reference:
参考</h2>
http://http.developer.nvidia.com/CgTutorial/cg_tutorial_chapter05.html
To complete the diffuse calculation, we need to multiply it with the data being provided to us by Unity and by the surfaceoutput struct. For this we need to multiply the a.Albedo value (which comes from our surf function) with the incoming _LightColor0.rgb value (which Unity provides), and then multiply the result of that with (difLight * atten) . Then, finally, return that value as the color. See the following code:
为了完成漫反射计算，我们需要将Unity提供给我们的数据乘上SurfaceOutput结构。为此我们需要乘上a.Albedo（来自上SurfaceOutput） 和_LightColor0.rgb（来自Unity） 然后再将结果与(difLight * atten) 相乘，最后，返回这个值做为颜色。请看下例
{% codeblock lang:c# %}
inline float4 LightingBasicDiffuse (SurfaceOutput s, fixed3 lightDir, fixed atten)
{
float difLight = max(0, dot (s.Normal, lightDir));
float4 col;
col.rgb = s.Albedo * _LightColor0.rgb * (difLight * atten * 2);
col.a = s.Alpha;
return col;
}
{% endcodeblock %}
The following screenshot demonstrates the result of our basic diffuse Shader:
下面这个截图展示了我们的基础man’fan’s下面这个截图展示了我们的漫反射着色器结果
{% asset_img image02.png %}
<h2>There’s more…
更多</h2>
By using the built-in Cg function called max, we can clamp the values that get returned from the dot product function. The max function takes two arguments. max (arg1, arg2 ) . We are using it in our Shader to make sure the values we are using for our diffuse calculation are between 0 and the maximum of the dot product .This way we will never get a value below 0. especially not -1, which would create extremely black areas in your Shader that wouldn’t play well with your Shader math later in the Shader process.
通过cg自带的max函数，我们可以限制dot计算结果。max函数采用两个参数max (arg1, arg2 ) ，我们在着色器里通过max来确保漫反射的计算结果永远落在0和dot的最大值之间。这样你就永远不会获得小于0的值，甚至-1，否则可能会在你的着色器区间生成黑色区域，并且在你之后的着色器运算过程中容易出问题。
There is also the saturate function within the Cg function library. This helps us to clamp
float values between 0 and 1 as well. The only difference between max () and saturate
is that you simply feed your float value into saturate. The max function takes two arguments
and returns the maximum value between the two.
除此之外cg里还有个类似saturate函数，可以帮助我们将float值限制到0和1之间。saturate和max的区别就是saturate可以直接将float值转换成饱和度。max函数接受两个参数并返回二者之间最大的值。
See also
You can find more infonnation on the Surface Shader lighting model function
arguments at
你可以在下面的网址找到更多的表面着色器光照的信息http://docs.unity3d.com/Documentation/Components/SL-SurfaceShaderLighting.html

-------------------------------------------------------------------------------------------------------------------------------------
<h2>Creating a Half Lambert lighting model
创建Half Lambert光照模型</h2>
Half Lambert was a technique created by Valve as a way of getting the lighting to show the surface of an object in low-light areas. It basically brightens up the diffuse lighting of the Material and wraps the diffuse light around an object’s surface.
Half Lambert 是一种用于在低光照区域照亮物体的技术，它基本上加亮了材质的漫反射光照以及环绕物体表面的漫反射灯光。
“Harf Lambert” lighting is a technique first developed in the original Half-Life (https://developer.valvesoftware.com/wiki/Half-Life ). it is designed to prevent the rear of an object losing its shape and looking too flat. Half Lambert is a completely nonphysical technique and gives a purely perceived visual enhancement. it is an example of a forgiving lighting model.
Harf Lambert技术最早应用于原始的半条命游戏。设计用于防止一个物体的背后失去形状看起来过于扁平，Half Lambert完全是一个非物理的技术，并给出了一个纯粹的感知视觉增强。它是高包含度光照模型的一个例子。
<h2>Reference:
参考</h2>
https://developer.valvesoftware.com/wiki/Half_Lambert
<h2>How to do it…
如何实现</h2>
Using the basic Shader that we created in the last recipe. let’s update the diffuse calculation
by following the next step:
首先使用我们在前面创建的基础着色器，让我们通过下面步骤来升级漫反射计算
Modify the diffuse calculation by multiplying it by 0.5. So. you would add the following
code to your lighting function:
将漫反射计算结果乘上0.5，然后，将下面的代码加入到你的光照函数里
{% asset_img image01-300x82.png %}
The following screenshot demonstrates the result of the implementation of the Half Lambert
technique into our Shader’s Iighting model:
下面的截图展示了将Half Lambert技术加入到我们的着色器光照模型后的结果

{% asset_img image08.png %}
<h2>How it works…
工作原理</h2>
The Half Lambert technique works by taking the range of values of the diffuse lighting dividing it in half. and then adding 0.5 back to it. This basically means that if you have a value of 1 and you cut it in half. you will have 0.5. Then, if you add 0.5 back to it. you will have 1 again. If you did this same operation to a value of 0. you would end up with 0.5. So, we have taken a range of values from 0 to 1 and re-mapped it to be within a range of 0.5 to 1.0. The following shows the diffuse value mapped to a function graph. showing the result of the Half Lambert calculation:
Half Lambert实现的原理是把漫反射光照除2然后加上0.5。意思就是如果你的值是1，对半开后就是0.5，然后你再加0.5回去，将会再得到1。如果你对0进行操作，那么你会得到0.5，因此我们将0到1之间的所有值重新映射到0.5和1的区间。下面展示了漫反射的值经过Half Lambert计算后的函数曲线图。

{% asset_img image00.png %}
<h2>Creating a ramp texture to control diffuse shading
创建一个梯度纹理来控制漫反射着色</h2>
Another great tool in your Shader writing toolbox is the use of a ramp texture to drive the color of the diffuse lighting. This allows you to accentuate the surface’s colors to fake the effects of more bounce light or a more advanced lighting setup. You see this technique used a lot more for cartoony games. where you need a more artist-driven look to your Shaders and not so much of a physically~accurate lighting model.
在你的着色器工具箱里还有个伟大的工具，那就是用一个梯度纹理来控制漫反射的颜色。这允许你突出表面的颜色，来模拟更多的光照或者更先进的灯光设置。你应该在很多卡通游戏里看到过这种技术的使用，当你不想要真实物理模拟的光照而是更艺术化的显示时，你会需要它的。
This technique became more popular with Team Fortress 2. where Valve came up with a unique approach to lighting their characters. They produced a very popular white paper on the subject. and you should definitely give it a read.The Valve White Paper on Team Fortress 2 Lighting and shading available at
这个技术随着Team Fortress 2游戏而开始流行，因为Value创造出了一种全新的方法来照亮他们的角色。他们还写了一本非常流行的白皮书，你绝对应该去看看。通过下面地址访问白皮书 http://www.valvesoftware.com/publications/2007/NPAR07_IllustrativeRenderingInTeamFortress2.pdf

<h2>Getting ready
准备</h2>
To get this started you will need to create a ramp texture in some image editing application. We used Photoshop for this particular demonstration. but any image editing application should be able to make a gradient
首先你需要用图像编辑软件创建一张梯度纹理，在这里我们提供了一张用Photoshop制作的特殊图片，不过不管哪个图像编辑软件应该都可以制作梯度纹理。

{% asset_img image05.png %}
<h2>How to do it…
如何做</h2>
Let’s begin our Shader by entering the following code:
让我们通过下面代码来开始学习
Simply modify the lighting function so that it includes this new code:
像下面这样修改之前的光照函数

{% asset_img image04-300x87.png %}
The following is the result you will see after running the code:
运行代码之后你就可以看到下图展示的结果了

{% asset_img image12.png %}
<h2>How it works…
工作原理</h2>
This line of code is returning a set of colors. or a float3. which is also the same as r. g. b. These colors are being produced by a Cg function called tex2D. The tex2D () function takes two arguments The first is the texture property we use for this operation. The second includes the UVs from the model that we want to map to the texture.
这行代码返回了一个颜色集，或者叫float3，相等于r.g.b三个属性，这些颜色是通过tex2D的cg函数生成的。tex2D函数有两个参数，第一个是我们使用的纹理，第二个值包含了一个我们希望映射到这张纹理的uv坐标。
In this case we do not want to use any UVs from a vertex. but instead we want to use the diffuse float range to map the UVs of the ramp texture. This ultimately wraps the ramp texture around the surface of the object. based on the direction to the light being calculated.
在这种情况下我们不希望使用顶点的uv值，而是用漫反射值来替换它来当作uv坐标映射到梯度纹理。最终将根据灯光计算后的方向来映射整个梯度纹理到物体的表面。
We take the re-mapped diffuse values from the Half Lambert operation and pass them into float2 () to create the lookup values for the texture. When a value of 0 is set as the hLambert variable. the tex2D function looks up the pixel value at the UV value of (0.0). In this case it’s the subtle peach color from the ramps gradient. when a value of 1 is set for the hLal’nbert variable. the tex2D function looks up the pixel at the UV value of (1.1). or the white color.
我们使用Half Lambert计算并重新映射的漫反射，并将之转换成float2类型从而创建纹理搜索值。当hLambert值为0时，tex2D函数会去搜索(0,0)UV坐标处的像素，在我们的示例里它会呈现梯度纹理对应的蜜桃色，当值是1是则会返回(1,1)处的白色。
Now it is possible for the artist to have some custom control over how the light looks on the surface of an object. This is why this technique is more commonly seen on a project where you need more of an illustrative look.
现在开始艺术家就拥有了一些自定义方法来控制灯光在物体表面的效果。因此这就是这个技术如此常见的原因，因为你需要更多的直观展示效果。
Creating a faked BRDF using a 2D ramp texture
用2D梯度纹理来模拟BRDF效果
We can take the ramp diffuse recipe one step further by using the view direction, provided by the lighting functions. to create a more advanced visual look to our lighting. By utilizing the view direction. we will be able to generate some faked rim lighting。
我们可以使用光照函数和视角方向生成的梯度漫反射结果来创建一个高级可视化效果，利用视角方向我们能够生成一些模拟的边缘光照。
If we look at the ramp diffuse technique. we are only using one value to place into the UV lookup of the ramp texture. This means that we will get a very linear type of lighting effect. In this recipe we will change our lighting function to take advantage of an additional argument. the view direction.
让我们回头来看看梯度纹理技术，可以看到我们才用了一个值来控制纹理的UV坐标，这意味着我们只能得到一个线性的光照效果。在这节我们会在光照函数里加入额外的视角参数来获得更好的效果。
The view direction is the user’s view of the object itself. It is a vector. pointing in a direction that we can use in conjunction with the normal and light direction. This view vector will provide us with the means to create a more advance texture lookup.
视角方向指的是我们看向物体自身的视线方向，这是一个向量，标注了方向，这意味着我们可以将之与法线以及灯光方向结合使用。来创建一个更先进的纹理查找技术。
In the Cg industry this technique is often referred to as a BRDF effect BRDF stands for bidirectional reflectance distribution function. While that is a mouthful. it simply means the way in which light is reflected off an opaque surface from both the view direction and the light direction. To see the effects of this BRDF Shader. let’s continue by setting up our scene and writing the Shader.
在CG行业这个技术通常叫做BRDF技术，代表双向反射分布函数。虽然这很拗口，简单来说就是光线在不透明物体表面被同时反射到视角和灯光两个方向。为了展示这种效果，让我们来设置环境并书写着色器
Getting ready
Before starting we will need a more embellished ramp texture this time. We need to include gradients for both dimensions of the texture.
开始前让我们先来修改下梯度纹理，我们需要让它包含两个方向的渐变。
1. Create a new texture with a size of 512 x 512.
创建一张512 x 512.的纹理
2. Create a gradient, diagonally starting from the bottom left of the image, going to the
top-right of the image.
从左下角到右上角创建一个梯度对角效果
3. Create another gradient from the top-left side. going until just before the middle of
the image.
同样从左上角再拉一条渐变效果，直到覆盖底部
4. Finally, create another ramp from the bottom-right side to just before the middle of
the image. You should end up with a texture shown in the following image:
最后，从右下角再创建一个渐变效果，最后，你会获得下图的图像

{% asset_img image03.png %}
How to do it…
Let’s go through this recipe by following the next few steps. using the basic diffuse Shader as our starting point:
让我们通过下面步骤来开始本节的学习，还有我们需要在前面创建的基础漫反射着色器上工作。
1. First we need to change our lighting function to include the viewDir variable that Unity provides us, to get the current view direction of the camera in the scene as it looks at our object. Modify your lighting function to look like the following code:
首先我们需要修改光照函数并加入Unity提供的视角变量，这样我们就可以获得当前场景的摄像机看此物体的方向，按照下图来修改你的代码：

{% asset_img image16-300x76.png %}
2. We then need to calculate the dot product of the view direction and the surface normal (as shown in the following code). This will produce a falloff type effect that we can use to drive our BRDF texture.
然后我们须将视角方向和表面法线进行点积运算，这样可以创建一个衰减效果用于展示BRDF纹理

{% asset_img image10-300x82.png %}
3. To complete the operation. we need to feed our dot product result into the float2 function of the tex2D( ) function. Modify your lighting function to the following code:
为了完成操作，还需要将点积的结果转换成float2类型来进行tex2D运算，按照下图来修改代码

{% asset_img image14-300x82.png %}
4. Save your Shader and re-enter Unity. Make sure you are using your new BRDF texture as the ramp texture in Unity. You should notice that your lighting now includes two rim light type effects: one for the bottom of the model and one for the top.
保存并再打开Unity，确保BRDF纹理已经在Unity里，然后你会发现你的光照函数产生了两种边缘高亮效果，上面和下面各一种效果。
The following image demonstrates the results of using a BRDF ramp texture to drive the overall diffuse color. This technique is great for a production team as it makes it easy for an artist to update a texture, in Photoshop. rather than tweak Iightinig in the game:
下图演示了使用BRDF渐变纹理带动整体漫反射颜色的结果。对于团队来说这是一个伟大的发明，因为它很容易让一个艺术家通过Photoshop来升级纹理效果。而不是在游戏中调整灯光。

{% asset_img image07-300x244.png %}
<h2>How it works…
运行原理</h2>
when using the view direction parameter, we can create a very simple falloff type effect. You can use this parameter to create a lot of different types of effects: a bubble type transparency. the start of a rim light effect. shield effects, or even the start of a toon outline effect.
当使用视角方向参数的时候，我们可以创建一个简单的衰减效果。你可以使用这个参数来制作任何不同类型的效果：一个透明的泡泡，边缘高光效果，盾牌效果，甚至可以制作卡通边缘线效果。

{% asset_img image15-300x294.png %}
The preceding image shows the dot product of the view direction and the surface nonnal. Consider if you were to look at the values being produce by taking the dot product of the view direction and the surface normal.
考虑到如果你想要看视角和发现点击的值的话，上面的截图展示了视角方向和表面法线点击的效果
In this case we are using it as one of the components in the BRDF ramp texture lookup. Since the diffLight calculation and the rimLight calculation both produce a linear range of values from 0 to 1, we can use both the ranges to pick different areas of the ramp texture.
在这种情况下我们将之做为BRDF 梯度纹理查找的条件之一，由于diffLight计算和rimLight计算的结果都是从0到1，所以我们可以使用这两个值来挑选梯度纹理的不同区域。

{% asset_img image13.png %}
A visualization of what is happening inside the Shader code and how it is picking the color to put on the surface
上图直观的展示了着色器代码是如何在表面上挑选颜色
So the key here is to understand what values we get from the dot product functions as well as how we can manipulate texture. inside of a lighting function. to wrap them around a surface in order to simulate a more complex lighting effect.
因此，关键就是去深入理解我们从点击运算获得的结果是如何操纵纹理的，以及在光照函数内部是如何围绕表面来模拟复杂光照效果的。
<h2>See also
参考</h2>
Refer to Polycount BRDF Map at wiki.polycount.com/BrdfMap
可以查看Polycount  wiki上的BRDF贴图