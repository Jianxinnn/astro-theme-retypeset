---
title: Docker使用GPU: Failed to initialize NVML
published: 2025-07-25
tags:
  - Linux
toc: false
lang: zh
draft: false
description: 解决 docker 容器中运行GPU 的 bug。
---

# 问题一：Docker with GPU: "Failed to initialize NVML: Unknown Error"
- 现象
    这个问题表现为容器（container）中CUDA无法正常调用，运行 nvidia-smi 报错 "Failed to initialize NVML: Unknown Error"，且 torch.cuda.is_available() 值为 False。重启容器可以暂时恢复正常，但宿主机（host）始终没有发生此类问题。


- 解决方案
    在 docker run 命令中，除了 --gpus all 外，加入 --privileged 参数。
例如：
```bash
docker run --gpus all --privileged <your_image_name>
```

# 问题二：Bus error. It is possible that dataloader's workers are out of shared memory. Please try to raise your shared memory limit.
- 现象  
    当使用PyTorch等框架进行数据加载时，可能会遇到 "Bus error" 报错，提示数据加载器的workers超出共享内存限制。

- 原因
    默认情况下，Docker容器的共享内存（/dev/shm）大小为 64MB，这对于需要大量共享内存的数据加载任务来说往往不足。

- 解决方案
    1. 新建容器时设置共享内存
        在启动容器时，通过向 docker run 追加 --shm-size 参数来解决。
        建议设置的共享内存大小不要超过物理内存的一半。

        例如，设置共享内存为 16GB：
        ```bash
        docker run --gpus all --shm-size=16gb <your_image_name>
        ```
    
    2. 已运行容器修改共享内存
        如果容器已经运行，则需要手动修改对应配置文件，然后重启容器。

        查看并记录容器ID：`docker ps`

        停止目标容器：`docker stop <container_id>`

        修改容器配置文件：`/var/lib/docker/containers/<container_id>/hostconfig.json` 文件。

        `sudo vi /var/lib/docker/containers/<container_id>/hostconfig.json`
        在文件中找到 "ShmSize" 字段，将其值修改为所需的字节数。
        
        例如，将 16GB 转换为字节为 17179869184（16 * 1024 * 1024 * 1024）。
        修改前可能类似："ShmSize": 67108864 (64MB)
        修改后："ShmSize": 17179869184

        启动容器：`docker start <container_id>`
        
        验证修改是否生效：进入容器内部，检查 /dev/shm 的大小。`docker exec <container_id> df -h /dev/shm`

---
参考资料
Nvml error: driver/library version mismatch - NVIDIA cuOpt / cuOpt - NVIDIA Developer Forums
[SOLVED] Docker with GPU: "Failed to initialize NVML: Unknown Error" / Applications & Desktop Environments / Arch Linux Forums
Can I increase shared memory after launching a docker session - Stack Overflow
