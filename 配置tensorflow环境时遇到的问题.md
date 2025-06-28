# Deepface中出现的tensorflow版本问题导致GPU无法使用的解决方案

首先是关于GPU的相关配置问题，我的环境是：

| win11 | CUDA 11.5 | cuDNN 8.9.5 |
| ----- | --------- | ----------- |

主要原因是我安装的CUDA版本是11.5，在deepface库安装的过程中，会自动下载的tensorflow匹配版本是tensorflow2.19，此时发现想用GPU跑代码的时候配置好了但是没有办法用GPU加速

当我使用tensorflow-gpu的2.7版本后还出现了这个问题

```
TypeError: Unable to convert function return value to a Python type! The signature was
	() -> handl
```

而使用tensorflow 2.12版本后，即便新版tensorflow已经取消了GPU和CPU版本的区别还是无法使用gpu，最终参考文章解决了这个问题：

参考文章：[CUDA11.8安装tensorflow2.12找不到GPU问题解决办法]([CUDA11.8安装tensorflow2.12找不到GPU问题解决办法_tensorflow 2.12 gpu-CSDN博客](https://blog.csdn.net/qq_45904458/article/details/133830702))
[Tensorflow-gpu保姆级安装教程（Win11, Anaconda3，Python3.9）](https://blog.csdn.net/weixin_43412762/article/details/129824339)

---

**问题：找不到GPU/可用数量为0**

问题原因：在tensorflow官网主页上有一条注释：

```
Caution: TensorFlow 2.10 was the last TensorFlow release that supported GPU on native-Windows.Starting with TensorFlow 2.11, you will need to install TensorFlow in WSL2 or install tensorflow-cpu and , optionally, try the TensorFlow-DirectML-Plugin
```

这段话讲的就是tensorflow2.10版本是最后一个在本机Windows上支持GPU的版本。换句话说**如果想在tensorflow框架下在Windows中使用GPU运行程序，最高版本只能安装2.10**

**解决方法：**

首先重新创建一个conda虚拟环境并激活，根据tensorflow官网查询可以使用3.9版本的python环境

```
conda create --name training python=3.9
conda activate training
```

然后安装编译好的dlib文件：

```
pip install dlib-19.22.99-cp39-cp39-win_amd64.whl
```

接下来先使用镜像安装需要的face-recognition和deepface

```
pip install face_recognition -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install DeepFace -i https://pypi.tuna.tsinghua.edu.cn/simple
```

此时在安装Deepface的时候会自动下载tensorflow2.19版本，需要重装2.10版本

```
pip uninstall tensorflow
pip install tensorflow==2.10.0 -i https://pypi.tuna.tsinghua.edu.cn/simple  
```

重装完后发现numpy2.x版本会出现兼容性问题，同样卸载重装

```
pip uninstall numpy  
conda install numpy=1.24 
```

到这里问题基本解决了，在deepface和face-recognition中可以使用gpu进行加速，接下来附一份用于检验gpu问题的ai生成代码

---

```
# ======================
# TensorFlow GPU 诊断工具
# 在 Jupyter Notebook 中逐单元格运行
# ======================

# 1. 检查基础环境
import sys
import tensorflow as tf
from tensorflow.python.client import device_lib
import numpy as np
import subprocess

print(f"Python 版本: {sys.version}")
print(f"TensorFlow 版本: {tf.__version__}")

# 2. 检查 GPU 是否被 TensorFlow 识别
print("\n=== TensorFlow 可用的设备 ===")
gpus = tf.config.list_physical_devices('GPU')
cpus = tf.config.list_physical_devices('CPU')
print("GPU 设备:", gpus)
print("CPU 设备:", cpus)

# 3. 检查 CUDA/cuDNN 是否可用
print("\n=== CUDA/cuDNN 信息 ===")
try:
    from tensorflow.python.platform import build_info
    print("CUDA 版本:", build_info.build_info['cuda_version'])
    print("cuDNN 版本:", build_info.build_info['cudnn_version'])
except:
    print("无法获取 CUDA/cuDNN 信息")

# 4. 检查 GPU 设备详细信息
print("\n=== GPU 设备详情 ===")
try:
    for device in gpus:
        print("设备名称:", device.name)
        print("设备类型:", device.device_type)
except:
    print("无法获取设备详情")

# 5. 运行 GPU 基准测试
print("\n=== GPU 基准测试 ===")
def benchmark_matmul(size=4096, device='/GPU:0'):
    with tf.device(device):
        a = tf.random.normal([size, size])
        b = tf.random.normal([size, size])
        %timeit tf.matmul(a, b)  # Jupyter 的魔法命令

if gpus:
    print("GPU 矩阵乘法测试 (4096x4096):")
    benchmark_matmul(device='/GPU:0')
    print("\nCPU 矩阵乘法测试 (4096x4096):")
    benchmark_matmul(device='/CPU:0')
else:
    print("未检测到 GPU，跳过基准测试")

# 6. 检查 GPU 内存信息
print("\n=== GPU 内存信息 ===")
try:
    for gpu in gpus:
        print(f"{gpu.name} 内存限制:", 
              tf.config.experimental.get_memory_info(gpu.name)['limit'] / (1024**3), "GB")
except:
    print("无法获取内存信息")

# 7. 检查 NVIDIA 驱动状态 (Linux/Windows)
print("\n=== NVIDIA 驱动状态 ===")
try:
    nvidia_smi = subprocess.check_output(['nvidia-smi', '--query-gpu=name,driver_version,memory.total', 
                                        '--format=csv,noheader'])
    print(nvidia_smi.decode('utf-8').strip())
except:
    print("无法运行 nvidia-smi (可能需要安装驱动或不在 Linux 系统)")

# 8. 设备放置日志 (检查操作是否真的在 GPU 上运行)
print("\n=== 设备放置日志示例 ===")
tf.debugging.set_log_device_placement(True)

# 运行一个简单操作看设备分配
a = tf.constant([1.0, 2.0])
b = tf.constant([3.0, 4.0])
c = a + b
print("计算结果:", c)
print("操作设备:", c.device)

# 关闭日志避免后续输出混乱
tf.debugging.set_log_device_placement(False)

# 9. 检查常见问题
print("\n=== 常见问题诊断 ===")
if not gpus:
    print("❌ 问题: TensorFlow 未检测到 GPU")
    print("可能原因:")
    print("1. CUDA/cuDNN 未正确安装")
    print("2. TensorFlow 版本与 CUDA 不匹配")
    print("3. 驱动程序未安装")
else:
    gpu_time = %timeit -o -q tf.matmul(tf.random.normal([4096,4096]), tf.random.normal([4096,4096])) if gpus else None
    cpu_time = %timeit -o -q tf.matmul(tf.random.normal([4096,4096]), tf.random.normal([4096,4096]), device='/CPU:0')
    
    if gpus and (gpu_time.average / cpu_time.average > 0.8):
        print("⚠️ 警告: GPU 与 CPU 性能差异小于 20%")
        print("可能原因:")
        print("1. 数据传输瓶颈 (检查 prefetch/cache 使用)")
        print("2. 计算任务太小 (尝试增大矩阵尺寸)")
        print("3. GPU 未充分利用 (检查 nvidia-smi 利用率)")
    else:
        print("✅ GPU 运行正常 (加速比: {:.1f}x)".format(cpu_time.average/gpu_time.average))

print("\n诊断完成！")
```

