---
title: ChimeraX 基本使用
published: 2025-05-29
tags:
  - Protein
toc: false
lang: zh
draft: false
description: chimerX1.9 简易实用命令
---
<!-- abbrlink: birth-of-retypeset -->

- 结构对齐
    
    单个结构对齐：`matchmaker #1 to #2`
    
    同时对齐：`matchmaker #1-3 to #4` 
    
- 结合界面残基识别
    
    选择蛋白，随后计算界面残基：`sel #1` → `interface`
    
- 选择残基 - 标注 **label**
    
    关键参数：`size`, `height`, `offset`, `color` 
    
    应用如：`label sel size 80 height 1.3 offset 1,-1,1 color dark`
    
    `~label` : 取消所有标签
    

- 对 af3 预测结果按照 plddt 染色
    
    `color bfactor #3 palette alphafold`
    

- 绘图风格
    - 动画扁平风格 `lighting flat`
    - 标准风格 `lighting sample`