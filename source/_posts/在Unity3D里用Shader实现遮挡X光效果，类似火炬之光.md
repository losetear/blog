title: 在Unity3D里用Shader实现遮挡X光效果，类似火炬之光
date: 2015-05-20 21:36:27
categories:
- Unity3D
tags:
- Shader
- XRay
---

实现效果：{% asset_img 1.png %}
原理很简单，就是用两个Pass来渲染物体，第一遍渲染被遮挡部分，开启ZWrite Off Ztest Greater，显示X光效果，第二遍正常渲染未遮挡部分，代码如下：
{% codeblock lang:GLSL %}
Shader "XRay"
{
    Properties
    {
        _MainTex ("Base (RGB)", 2D) = "white" {}
        _MatCap ("MatCap (RGB)", 2D) = "white" {}
        _Color("_Color", Color) = (0,1,0,1)
    }
    Subshader
    {
        Tags { "Queue"="Transparent" "IgnoreProjector"="True" "RenderType"="Transparent" }
 
        Pass
        {
            Tags { "LightMode" = "Vertex" }
 
            Blend One OneMinusSrcColor
            Cull Off
            Lighting Off
            ZWrite Off
            Ztest Greater
 
            CGPROGRAM
                #pragma vertex vert
                #pragma fragment frag
                #pragma fragmentoption ARB_precision_hint_fastest
                #include "UnityCG.cginc"
 
                struct v2f
                {
                    float4 pos    : SV_POSITION;
                    float2 cap    : TEXCOORD0;
                };
 
                v2f vert (appdata_base v)
                {
                    v2f o;
                    o.pos = mul (UNITY_MATRIX_MVP, v.vertex);
 
                    half2 capCoord;
                    capCoord.x = dot(UNITY_MATRIX_IT_MV[0].xyz,v.normal);
                    capCoord.y = dot(UNITY_MATRIX_IT_MV[1].xyz,v.normal);
                    o.cap = capCoord * 0.5 + 0.5;
 
                    return o;
                }
 
                uniform float4 _Color;
                uniform sampler2D _MatCap;
 
                float4 frag (v2f i) : COLOR
                {
                    float4 mc = tex2D(_MatCap, i.cap);
 
                    return _Color * mc * 2.0;
                }
            ENDCG
        }
        Pass {
            Tags { "LightMode" = "Vertex" }
            ColorMaterial AmbientAndDiffuse
            Lighting On
            SetTexture [_MainTex] {
                combine texture
            } 
        }
    }
    FallBack "Mobile/VertexLit"
}
{% endcodeblock %}

Shader里用到了一张X光贴图，用于优化遮挡部分效果，请直接右键保存
{% asset_img 2.png %}