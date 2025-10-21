---
title: "DeepSeek-OCR论文阅读记录"
author: curya
date: 2025-10-21
categories: [Blogs,BasicKnow,LLM]
tags: [BasicKnow]
math: true
mermaid: true
---

论文标题：DeepSeek-OCR: Contexts Optical Compression

**核心：**
- **光学2D映射（optical 2D mapping） --> 上下文压缩**
- 模型构成：Encoder-Decoder结构
    - DeepEncoder - 在高分辨率输入下保持低激活，同时实现高压缩比，以确保最佳且可管理的视觉tokens数量。【token数量控制 - 文本token转视觉token】
    - DeepSeek3B-MoE-A570M

## 1. Introduction
利用视觉模态作为文本信息有效压缩的介质 - 简单来讲，就是把文本绘制为图像，可以使用相较于原始文本对应的文本tokens更少的视觉tokens表征丰富的信息。

DeepEncoder：即使使用高分辨率输入，也可以保持低激活内存和最少的视觉tokens。
- 通过16x卷积压缩器串行连接window attention和global attention编码器组件。
- 窗口注意力组件处理大量的视觉令牌；压缩器在视觉tokens进入密集全局注意力组件之前减少了token数量，实现了有效的记忆和令牌压缩。

在OCR任务上进行实验验证效果。

## 2. Related Works
### 2.1 VLM模型中的视觉编码器
<img width="2924" height="956" alt="image" src="https://github.com/user-attachments/assets/32defdba-abe9-4364-8940-d8087553ea19" />

类型一：双塔架构（e.g. Vary），使用并行SAM编码器增加高分辨率图像处理的视觉词汇参数。*提供了可控的参数和激活内存，但需要双重图像预处理、部署复杂、训练阶段pipeline并行困难。*

类型二：tile-based方法（e.g. InternVL 2.0），将图像分为小的图像块进行并行计算处理图像。*能够处理极高分辨率图像，但大图像的视觉token量也很大。*

类型三：自适应分辨率编码（e.g Qwen-VL系列），使用NaViT范式，通过基于patch的分割直接处理完整图像。*可以灵活处理不同分辨率，但可能导致GPU显存溢出，序列打包也需要很长的序列长度。*

### 2.2 端到端OCR模型
传统pipeline架构（单独的检测 & 识别模块） --> VLM（端到端OCR）

## 3. 方法
<img width="2942" height="1236" alt="image" src="https://github.com/user-attachments/assets/478ad7a6-f83b-4ced-b69c-3556277224e3" />

### 3.1 模型结构
编码器-解码器结构的端到端VLM
- 编码器：提取图像特征、tokenizing、以及压缩视觉表征。**SAM-Base(80M) & CLIP-large(300M)**
- 解码器：基于图像tokens和提示词生成所需的结果。**DeepSeek3B-MoE-A570M**

### 3.2 DeepEncoder（编码器）
为了探索上下文光学压缩的可行性，视觉编码器要求如下: 
1. 能够处理高分辨率;
2. 高分辨率下的低激活;
3. 少的视觉tokens;
4. 支持多分辨率输入;
5. 中等参数计数。

#### 3.2.1 DeepEncoder结构
包含两个组件：
1. 以窗口注意力为主的**视觉感知特征提取**组件：SAM-Base (patch-size 16) - 低维特征提取
   * 2-layer卷积，实现视觉token的16x下采样：每个卷积kernal为3，stride为2，padding为1，channel 256->1024
3. 具有密集全局注意力的**视觉知识特征提取**组件：CLIP-large - 高维特征提取
   * 移除初始的patch embedding层，因为输入不是图像，而是前面的tokens输出
  
Example: 1024 x 1024的输入图像，SAM-Base输出1024/16 x 1024/16 = 4096个patch token，卷积下采样后为 4096 / 16 = 256 个tokens。

#### 3.2.2 多分辨率支持
<img width="3378" height="1458" alt="image" src="https://github.com/user-attachments/assets/465d7458-61d2-4298-91f4-1a47673556d3" />
<img width="2160" height="622" alt="image" src="https://github.com/user-attachments/assets/b3b4eb2b-a8b0-4d89-a47a-4fda68f952c4" />

原生分辨率和动态分辨率：
* 原生分辨率：512×512 (64), 640×640 (100), 1024×1024 (256), 1280×1280 (400)
* 动态分辨率：
    * n×640×640 tiles (local views) & 1024×1024 (global view) - **n * 100 + 256**
    * n×1024×1024 tiles (local views) & 1280×1280 (global view) - **n * 256 + 400**

### 3.3 The MoE Decoder
xxx

### 3.4 数据

- **OCR 1.0数据**：主要包括场景图像OCR和文档OCR等传统OCR任务
    - 30M pages PDF data：约100种语言，中文&英文 25M，其他语言 5M
    - 标注：
        - 1）Coarse annotations：直接使用fitz处理pdf，教会模型识别光学文本，特别是小语种；
        - 2）Fine annotations：2M pages（中文&英文），使用layout模型和OCR模型
- **OCR 2.0数据**：主要包括对复杂人工图像的解析任务，如常见图表、化学公式、和平面几何解析数据
- **通用视觉数据**：主要用于在DeepSeek-OCR中注入一定的通用图像理解能力
- **文本数据**

### 3.5 训练流程

### 4. 评估

