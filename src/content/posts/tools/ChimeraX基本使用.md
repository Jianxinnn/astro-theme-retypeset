---
title: ChimeraX 基本使用
published: 2025-05-29
tags:
  - 蛋白质
toc: false
lang: zh
draft: false
description: chimerX1.9 简易实用命令
---
<!-- abbrlink: birth-of-retypeset -->
<!-- updated: 2025-05-29 -->

- 结构对齐
    
    单个结构对齐：`matchmaker #1 to #2`
    
    同时对齐：`matchmaker #1-3 to #4` 
    
- 结合界面残基识别(相互作用分析)
    
    选择蛋白，随后计算界面残基：`sel #1` → `interface`

    > 在菜单栏点击“Select > User-Defined Selectors > ligand sel”选择我们之前定义的配体组，然后点击“Tools > Structure Analysis > H-bonds”；在弹出的“H-Bonds”面板中，确认“Limit by Selection”被勾选，并且下拉菜单设置为“With At Least One End Selected”然后点击“OK”进行分析；
    
- 选择残基 - 标注 **label**
    
    关键参数：`size`, `height`, `offset`, `color` 
    
    应用如：`label sel size 80 height 1.3 offset 1,-1,1 color dark`
    
    `~label` : 取消所有标签
    

- 对 af3 预测结果按照 plddt 染色
    
    `color bfactor #3 palette alphafold`
    

- 绘图风格
    - 动画扁平风格 `lighting flat`
    - 标准风格 `lighting sample`

- 选择接触残基
    在菜单栏点击“Select > Zone/Contacts，Select的下拉菜单选择“residues”，勾选“< 5.000 Å from the currently selected atoms”选项，然后“OK”确认

    Command：`select sel :<5` 

- 显示 surface
    `Actions→Surface→mesh`

- 透明度设置
    command: `transparency 50`  # 设置透明度50%