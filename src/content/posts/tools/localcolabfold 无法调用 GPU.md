---
title: localcolabfold 无法调用 GPU
published: 2025-05-30
tags:
  - 蛋白质
toc: false
lang: zh
draft: false
description: 一次就解决 colabfold 本地部署出现的识别不了 GPU 的问题
---


- 目的：
    
    **为实现本地AlphaFold蛋白质结构预测，我尝试部署[localcolabfold](https://github.com/YoshitakaMo/localcolabfold) 环境。根据官方推荐的安装步骤：**
    

# 安装过程

## **脚本安装**

```bash
wget https://raw.githubusercontent.com/YoshitakaMo/localcolabfold/main/install_colabbatch_linux.sh
bash install_colabbatch_linux.sh
export PATH="/path/to/your/localcolabfold/colabfold-conda/bin:$PATH"
colabfold_batch input outputdir/
```

在linux中，确保安装好gcc和nvcc等。

- gcc -v
    
    ```bash
    	$ gcc --version
    gcc (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
    Copyright (C) 2019 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    ```
    
- nvcc -v
    
    ```bash
    $ nvcc --version
    nvcc: NVIDIA (R) Cuda compiler driver
    Copyright (c) 2005-2022 NVIDIA Corporation
    Built on Wed_Sep_21_10:33:58_PDT_2022
    Cuda compilation tools, release 11.8, V11.8.89
    Build cuda_11.8.r11.8/compiler.31833905_0
    ```
    

## conda 安装

更简单的安装方法

```bash
# https://github.com/YoshitakaMo/localcolabfold/issues/210#issuecomment-2456052121
conda create -n localcolabfold python=3.10
conda activate localcolabfold
pip install --upgrade "jax[cuda12]" -f [https://storage.googleapis.com/jax-releases/jax_releases.html](https://storage.googleapis.com/jax-releases/jax_releases.html)
pip install "colabfold[alphafold] @ git+https://github.com/sokrypton/ColabFold"
```

# 问题

在完成所有依赖安装后运行：

```bash
colabfold_batch input.fasta output_dir
```

却持续抛出错误：

> **WARNING: no GPU detected, will be using CPU**
> 

## **疑难排查过程**

### **初步验证**

1. **Jax GPU检测验证**

翻了大部分 issue，基本上问题在于 jax 识别不了gpu，显示为：

```
$ /path/to/your/localcolabfold/colabfold-conda/bin/python3.10
Python 3.10.13 | packaged by conda-forge | (main, Dec 23 2023, 15:36:39) [GCC 12.3.0] on linux
>>> import jax
>>> print(jax.local_devices()[0].platform)
cpu
```

本地测试了jax，但是显示 jax 能够检测到 gpu。运行 colabfold 时仍然无法调用 gpu。感觉不是jax问题。

```bash
>>> import jax
>>> print(jax.local_devices()[0].platform)
gpu
```

2. **环境依赖版本检查**

在本地执行中，根据上述的要求，确认所有软件都已经安装成功：

```bash
$ nvcc --version
CUDA V11.8已正确安装
$ nvidia-smi
GPU状态正常显示
```

### **深度排查**

折腾非常多时间，尝试以下措施均无效：

- 重新安装CUDA Toolkit与NVIDIA驱动
- 更新GCC到12.3.0
- 设置**`CUDA_VISIBLE_DEVICES=xx`**
- 尝试Jax版本0.4.33-0.4.38之间的多个版本
- 创建干净环境并排除其他conda包的干扰

试错到筋疲力尽时，在 issue 中看到以前使用的 0.4.2x 版本的 jax。鬼使神差尝试更老的版本（官方不建议安装），结果神奇的检测到了 GPU。

> 2025-03-06 17:39:48,925 Running on GPU
> 

没高兴太早，后续直接报错，找不到 jax 的某库，显然是 jax 版本不对。

好在，至少定位了jax问题。

重头创建了一个新的linux账号，构建一个全新的环境是否复现BUG。

运行时BUG居然没了！检测到GPU，顺利预测。

于是猜测，可能是环境变量的问题。

几乎全部试错完，终于找到问题所在：

```bash
export XLA_FLAGS="--xla_gpu_enable_triton_gemm=false"
export XLA_PYTHON_CLIENT_PREALLOCATE=true
export XLA_CLIENT_MEM_FRACTION=0.95
```

这些变量是之前安装AlphaFold3(NF-old)时添加的配置，具体影响如下：

1. **`XLA_GPU_ENABLE_TRITON_GEMM=false`**
    
    禁用了最新的Triton矩阵乘法优化，该参数在较新Jax版本中已被弃用，导致XLA无法正确初始化GPU计算后端。
    
2. **`XLA_CLIENT_MEM_FRACTION=0.95`**
    
    过高的内存分配比例可能引发显存预分配失败，但更核心的是与上述参数的联合作用破坏了Jax的环境感知。
    

> 坑爹的 `alphafold3`，安装 af3 时添加的环境变量直接导致了 jax 无法调用 GPU。折腾了两天。


## **解决方案**

### **修复步骤**

1. **临时排除环境变量**

```bash
unset XLA_FLAGS
unset XLA_PYTHON_CLIENT_PREALLOCATE
unset XLA_CLIENT_MEM_FRACTION
colabfold_batch test.fa output/
> GPU成功被识别并运行
```

> 注意：此时的jax版本等就与官方的一致，无需手动升降版本。


控制排查后，问题由 `1XLA_CLIENT_MEM_FRACTION`引起。