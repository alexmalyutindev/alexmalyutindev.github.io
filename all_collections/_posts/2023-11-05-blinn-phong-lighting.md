---
layout: post
title: Blinn-Phong Lighting
date: 2023-11-05 10:18:00
categories: [WIP, graphics, shader, lighting]
---

<p align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/0/01/Blinn_Vectors.svg" alt="drawing" style="width:400px;"/>
</p>

Phong Lighting Model:
---

[WIP]

```hlsl
half3 Phong(half3 normalWS, half3 lightDirectionWS, half3 lightColor)
{
    half NdotL = dot(normalWS, lightDirectionWS);

    // Max with zero is needed to eliminate negative values
    return max(0, NdotL) * lightColor;
}
```

Blinn-Phong Lighting Model:
---

[WIP]

```hlsl
half3 BlinnPhong(half3 normalWS, half3 lightDirectionWS, half3 lightColor)
{
    // Halfway vector
    half3 h = normalize(normalWS + lightDirectionWS);
    half NdotH = dot(normalWS, h);

    return max(0, HdotN) * lightColor;
}
```


Example Shader:
---

```hlsl
Shader "AlexMalyutinDev/BlinnPhong"
{
    Properties
    {
        _Color ("Main Color", Color) = (1.0, 1.0, 1.0, 1.0)
    }

    SubShader
    {
        Tags
        {
            "RenderType" = "Opaque"
            "RenderPipeline" = "UniversalPipeline"
        }

        Pass
        {
            Name "Phong"
            Tags
            {
                "LightMode" = "UniversalForward"
            }

            HLSLPROGRAM

            #pragma vertex Vertex
            #pragma fragment Fragment

            #include "Packages/com.unity.render-pipelines.core/ShaderLibrary/SpaceTransforms.hlsl"
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Input.hlsl"

            half4 _Color;

            struct Attributes
            {
                float4 positionOS : POSITION;
                float3 normalOS : NORMAL;
            };
            
            struct Varyings
            {
                half3 normalWS : TEXCOORD0;
                float4 positionCS : SV_POSITION;
            }

            half3 BlinnPhong(half3 normalWS, half3 lightDirectionWS, half3 lightColor)
            {
                // Halfway vector
                half3 h = normalize(normalWS + lightDirectionWS);
                half NdotH = dot(normalWS, h);

                return max(0, HdotN) * lightColor;
            }

            Varyings Vertex(Attributes input)
            {
                Varyings output = (Varyings)0;
                output.normalWS = TransformObjectToWorldNormal(input.normalOS.xyz);
                output.positionCS = TransformObjectToHClip(input.positionOS.xyz);
            }

            half4 Fragment(Varyings input) : SV_Target
            {
                half4 color = _Color;
                // mainLightDirection is a negative vector of actual light direction
                half3 mainLightDirection = -_MainLightPosition.xyz;

                half3 lighting = half3(0.0h, 0.0h, 0.0h);
                lighting = BlinnPhong(input.normalWS, mainLightDirection, _MainLightColor.rgb);

                color.rgb *= lighting;

                return color;
            }

            ENDHLSL
        }
    }
}
```
