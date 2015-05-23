title: Unity3D 优化系列纹章之《如何在手机场景中渲染成百上千个怪物而不卡》
date: 2015-05-23 09:00:00
categories:
- Unity3D
---
<h1>如何在Unity 3D中优化使用大量SkinedMeshRenderer</h1>

Unity3D的核心优化要点就是Combine! 意思就是合并，模型和材质以及贴图最好都要合并。因为一个独立的模型和材质，渲染会占用一次Draw Call，而Draw Call的多少，是现代显卡的瓶颈所在，在手机上尤甚！

比如你有一个怪物，自身的模型和盔甲以及武器分别是独立的模型和贴图，单独渲染这个怪物，就会占用你3个Draw Call。如果你本身的场景就已经占用了大量的Draw Call，还要刷十几个怪物的话，我相信你的游戏在低端手机上是无法跑的流畅的。关于场景制作的优化方式，我会在之后的文章里和大家分享，这里只讨论带骨骼动画的模型如何优化显示。

Unity自身已经替我们做了大量的优化工作，如果你的模型是不带骨骼并且顶点数在300一下，那么重复出现的这个模型是会被自动优化从而只占用一个DrawCall，如下图：
{% asset_img 1-300x193.png %}

但是如果你需要让这个模型带骨骼并动起来，不好意思，Unity不支持将之优化到一个DrawCall。在下图我们可以看到同时渲染了5个骷髅怪，每个占用3个DrawCall，一共消耗了15个DrawCall：
{% asset_img 2-300x245.png %}
 
当然，你可以将这个骷髅怪的模型装备武器合并到一起，纹理也合并掉，公用一个材质，这样五个怪物就只占用5个DrawCall。如果你不需要同时刷新成百上千个怪物的话，这样就够用了。不过有时候我们会希望同时刷更多的怪物出来，可以让玩家感受场面的宏伟，并且打的更爽快。这个时候怎么办呢？

这篇文章介绍的是使用插件优化的方式，插件名称叫做MeshBaker2，下载地址就不提供了，各位可以自行搜索。这是一个非常强大的插件，支持MeshRender和SkinedMeshRender的合并，还支持运行时动态合并，在这里我们使用的就是动态合并的方式，下图可以看到在合并前渲染25个矮人共消耗136个DrawCall：
{% asset_img 3-300x165.png %}

然后在开启MeshBaker的动态合并之后，我们可以很直接发现DrawCall变成了3个，其中地面和UI各占用一个DrawCall，而我们的25个怪物则已经合并到了一个DrawCall渲染。
{% asset_img 4-300x172.png %}
 
其中核心代码如下：
//newCombines指这次调用新增加的Render对象，oldCombines指已经不用需要删除的Render对象
{% codeblock lang:c# %}
mb.AddDeleteGameObjects (newCombines.ToArray (), oldCombines.ToArray ());
mb.Apply ();
mb.EnableDisableSourceObjectRenderers (false);
{% endcodeblock %}
源代码就不放出了，因为里面的插件涉及到版权问题。只要查看Mesh Baker的官方文档就可以很容易使用了。
下面是demo演示地址：https://db.tt/8C924w6A