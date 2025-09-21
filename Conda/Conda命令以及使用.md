# Conda命令以及使用

---

昨天属于是花了大量的时间来配置这个`conda`，相关的文章找不太全了，这里还是先把命令记录下

## 常用 Conda 虚拟环境命令

1. **创建新环境**：

  ```
  conda create --name myenv  # 创建一个名为 myenv 的默认环境
  conda create --name py39 python=3.9  # 创建并指定 Python 版本
  ```

2. **激活环境**：

	```
	conda activate myenv  # 新版本通用写法
	source activate myenv # 旧版本 macOS/Linux
	activate myenv        # 旧版本 Windows
	```

3. **查看所有环境列表**：

	```
	conda env list
	```

4. **退出当前环境的命令：**

	```
	conda deactivate
	```

	> 如果使用旧版本 Conda（<4.6），可能需要用 `source deactivate` 或直接输入 `deactivate`。

4. **删除环境**：

	```
	conda env remove --name myenv
	```

5. **克隆环境**（复制已有环境配置）：

	```
	conda create --name new_env --clone old_env
	```

6. **导出环境配置**（用于跨设备复用）：

	```
	conda env export > environment.yml  # 导出到文件
	conda env create -f environment.yml # 根据文件重建环境
	```

7. **安装/卸载包**：

	```bash
	conda install numpy   # 安装包
	conda remove numpy    # 卸载包
	```

8. **查看当前环境的包列表**：

	```
	conda list
	```

---

## 配置镜像以及下载路径的命令

1. **添加镜像源**

  ```
  conda config --add channels soumith
  ##设置搜索时显示通道地址
  conda config --set show_channel_urls yes
  ```

2. **显示已经有的镜像源**

	```
	conda config --show
	```

3. **删除单个镜像源**

	```
	conda config --remove channels 镜像源地址
	conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/
	```

4. **删除全部的镜像源**

	```
	conda config --remove-key channels
	```