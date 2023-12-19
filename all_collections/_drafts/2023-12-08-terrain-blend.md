---
layout: post
title: Terrain Blend
date: 2023-12-08 00:20:00
categories: [WIP, graphics, shader, terrain]
---

# Intro

There is no good general solution for this problem, and I will tell about two of them. One is really bad, and another is ok. In addition one special case sollution.

What terrain blend is from visual perspective? It is an seamless transition from terrain to the object, another words terrain should cover bottom part of adjacent surface. Sounds easy, yes?

From rendering perspective it is not a trivial problem: terrain and other geometry renders separetly and don't know of each other. In Unity's implementation, terrain renders first and then other opaque geometry. We need some how to draw terrain on top of this opaque surface to achive therrain blend effect.

# Naive Blend

Lets start with naive solution. In realtime render is better to prepare the content to make rendering process more lightweight, such thing call baking. 

Let's make a scene with only one terrain and place some objects:

[scene]

Colormap...
Heightmap...

# Render Twice


# Depth Projection 


# Depth Projected Terrain Blend 
