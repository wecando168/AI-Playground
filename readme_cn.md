
# AI Playground

![image](https://github.com/user-attachments/assets/66086f2c-216e-4a79-8ff9-01e04db7e71d)

此示例基于Intel Arc A系列独立显卡和Ultra系列核心显卡的xpu实现

欢迎来到AI Playground开源项目测试版和AI PC入门应用程序，用于在由Intel®Arc™GPU驱动的PC上进行AI图像创建、图像样式化和聊天机器人。AI Playground利用了GitHub和Huggingface的库，这些库可能并非在全球所有国家都可用。

## 自述文件.md
- 中文 (readme_cn.md)

## 最小需求
AI Playground测试版目前以打包安装程序的形式提供，也可以从我们的Github存储库中以源代码的形式提供。要运行AI Playground，您必须拥有符合以下规格的电脑

*	Windows 操作系统
*	英特尔Core Ultra-H处理器（即将推出）或配备8GB vRAM的Intel Arc 独立显卡

## 安装-打包安装程序： 
AI Playground有多个打包的安装程序，每个安装程序都对应特定于硬件。
1. 选择正确的安装程序（适用于配备Intel Arc 独立显卡的台式机系统或Intel Core Ultra-H系列CPU系统），下载到您的电脑上，然后运行安装程序。
2. 安装程序将分为两个阶段。它将首先从安装程序安装组件和环境。第二阶段将从源头引入组件。</b >
安装的第二阶段**需要几分钟**，并且需要稳定的互联网连接。
3. 首次运行时，加载屏幕最多需要一分钟
4. 下载用户指南以获取应用程序信息

*	独立显卡的AI Playground-[发行说明](https://github.com/intel/AI-Playground/releases/tag/v1.0beta) | [下载](https://github.com/intel/AI-Playground/releases/download/v1.0beta/AI.Playground-v1.0b-Desktop_dGPU.exe)

*	英特尔Core Ultra-H处理器的AI Playground-即将推出。

*	[AI Playground用户指南](https://github.com/intel/ai-playground/blob/main/AI%20Playground%20Users%20Guide.pdf)


## 项目开发
### 开发环境设置（后端，python）

假定已经将源文件下载到D:\AIPlayground\AI-Playground

1. 创建并切换conda环境。
注：假定安装Anaconda发行版本使用Anaconda Prompt命令行窗口（注意以管理员方式运行）
```cmd
Anaconda Prompt命令行窗口
conda create -n aipg_xpu python=3.10 -y
activate aipg_xpu
```

2. 然后转到服务目录。
```cmd
Anaconda Prompt命令行窗口
CD D:\AIPlayground\AI-Playground\service
```

3. 根据使用的是满足要求的英特尔Arc A系列独立显卡还是英特尔Core Ultra-H处理器进行安装。
英特尔Arc A系列独立显卡
```cmd
Anaconda Prompt命令行窗口
pip install -r requirements-arc.txt
```

英特尔Core Ultra-H处理器
```cmd
Anaconda Prompt命令行窗口
pip install -r requirements-ultra.txt
```

4. 下载英特尔Pytorch*AOT扩展包。根据您的硬件，请从以下链接下载cp310 whl文件。

英特尔Arc A系列独立显卡
https://github.com/Nuullll/intel-extension-for-pytorch/releases/tag/v2.1.10%2Bxpu

英特尔Core Ultra-H处理器
https://github.com/Nuullll/intel-extension-for-pytorch/releases/tag/v2.1.20%2Bmtl%2Boneapi

使用pip Install命令安装所有下载的whl文件

Anaconda Prompt命令行窗口内切换路径到下载路径，等待后续操作

提示：下载时可通过辅助工具wget，这样可以断点续传
wget工具：https://eternallybored.org/misc/wget/
wget命令行断点续传命令：wget 下载连接 -c

英特尔Arc A系列独立显卡安装扩展包命令示范
```cmd
Anaconda Prompt命令行窗口
pip install "intel_extension_for_pytorch-2.1.10+xpu-cp310-cp310-win_amd64.whl"
pip install "torch-2.1.0a0+cxx11.abi-cp310-cp310-win_amd64.whl"
pip install "torchaudio-2.1.0a0+cxx11.abi-cp310-cp310-win_amd64.whl"
pip install "torchvision-0.16.0a0+cxx11.abi-cp310-cp310-win_amd64.whl"
```

英特尔Core Ultra-H处理器安装扩展包命令示范
```cmd
Anaconda Prompt命令行窗口
pip install "intel_extension_for_pytorch-2.1.20+git4849f3b-cp310-cp310-win_amd64.whl"
pip install "torch-2.1.0a0+git7bcf7da-cp310-cp310-win_amd64.whl"
pip install "torchaudio-2.1.0+6ea1133-cp310-cp310-win_amd64.whl"
pip install "torchvision-0.16.0+fbb4cc5-cp310-cp310-win_amd64.whl"
```

4. 检查XPU环境是否正确（这里可以看到默认安装位置）
```cmd
Anaconda Prompt命令行窗口
python -c "import torch; import intel_extension_for_pytorch as ipex; print(torch.version); print(ipex.version); [print(f'[{i}]: {torch.xpu.get_device_properties(i)}') for i in range(torch.xpu.device_count())];"
```

### 将开发环境与项目环境关联

1. 切换到项目的根目录。(AI-Playground)

2. 运行以下命令查看conda虚拟环境的路径
```cmd
Anaconda Prompt命令行窗口
conda env list|findstr aipg_xpu
```

```cmd
Anaconda Prompt命令行窗口返回默认参考结果
aipg_xpu              *  C:\ProgramData\anaconda3\envs\aipg_xpu
```

3. 根据获得的环境路径，运行以下命令以创建env文件链接
```
mklink /J "./env" "{aipg_xpu_env_path}"
参考：mklink /J "./env" "C:\ProgramData\anaconda3\envs\aipg_xpu"
```

### WebUI (nodejs + electron)

1. 安装Nodejs开发环境，下载地址：https://nodejs.org/en/download
https://nodejs.org/dist/v20.16.0/node-v20.16.0-x64.msi

2. 切换到WebUI目录并安装所有Nodejs依赖项。
参考路径：D:\AIPlayground\AI-Playground\WebUI>

A、直接安装（这个可能会很慢，原因是使用了国外的源镜像）
```cmd
PowerShell命令行窗口
npm install
``` 
注：npm安装是根据package.json文件来进行下载配置这些，需要了解可以查看目录下这个文件（D:\AIPlayground\AI-Playground\WebUI）

B、换源安装
```cmd
PowerShell命令行窗口
npm config get registry/*查默认安装源，一般是https://registry.npmjs.org）*/
npm config set registry https://registry.npm.taobao.org/*更换源*/
npm install/*安装*/
```
 
C、安装异常处理
安装如果一直转圈，可以先清缓存、关闭ssl验证，然后在开始
```cmd
PowerShell命令行窗口
npm cache verify/*清缓存*/
npm config set strict-ssl false/*看日志有“CERT_HAS_EXPIRED”错误，可以通过关闭ssl验证来解决*/
npm install --cache /tmp/empty-cache/*使用临时缓存安装*/
```

D、安装问题排查
日志目录：C:\Users\用户目录\AppData\Local\npm-cache\_logs
缓存目录：C:\Users\用户目录\AppData\Local\npm-cache\_cacache
相关包查询命令
```cmd
PowerShell命令行窗口
npm fund
```

3. 在WebUI目录中，运行以下命令以开始开发
```
PowerShell命令行窗口
npm run dev
```

## 模型支撑
AI Playground支持PyTorch LLM、SD1.5和SDXL型号。AI Playground不附带任何模型，但可以直接从界面或通过用户从CivitAI.com的HuggingFace.com下载模型并将其放置在适当的模型文件夹中，间接提供所有功能的模型。 

当前从应用程序链接的模型 
| 模型                                      | 许可协议                                                                                                                                                                      | 背景信息/型号卡                                                                                      |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Dreamshaper 8 Model                        | [license](https://huggingface.co/spaces/CompVis/stable-diffusion-license)                                             | [site](https://huggingface.co/Lykon/dreamshaper-8)                               |
| Dreamshaper 8 Inpainting Model             | [license](https://huggingface.co/spaces/CompVis/stable-diffusion-license)                                             | [site](https://huggingface.co/Lykon/dreamshaper-8-inpainting)         |
| JuggernautXL v9 Model                      | [license](https://huggingface.co/spaces/CompVis/stable-diffusion-license)                                             | [site](https://huggingface.co/RunDiffusion/Juggernaut-XL-v9)           |
| Phi3-mini-4k-instruct                      | [license](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct/resolve/main/LICENSE)                 | [site](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct)     |
| bge-large-en-v1.5                          | [license](https://github.com/FlagOpen/FlagEmbedding/blob/master/LICENSE)                 | [site](https://huggingface.co/BAAI/bge-large-en-v1.5)                         |
| Latent Consistency Model (LCM) LoRA: SD1.5 | [license](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/blob/main/LICENSE.md) | [site](https://huggingface.co/latent-consistency/lcm-lora-sdv1-5) |
| Latent Consistency Model (LCM) LoRA:SDXL   | [license](https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/blob/main/LICENSE.md) | [site](https://huggingface.co/latent-consistency/lcm-lora-sdxl)     |

请务必检查AI Playground中使用的任何型号的许可条款，特别是注意任何限制。

### 使用替代模型
查看[用户指南](https://github.com/intel/ai-playground/blob/main/AI%20Playground%20Users%20Guide.pdf) for details or [watch this video](https://www.youtube.com/watch?v=1FXrk9Xcx2g) on how to add alternative Stable Diffusion models to AI Playground

### 通知和免责声明： 
有关AI Playground条款、许可和免责声明的信息，请访问GitHub repo上的项目和文件：</br >
[License](https://github.com/intel/ai-playground/blob/main/LICENSE) | [Notices & Disclaimers](https://github.com/intel/ai-playground/blob/main/notices-disclaimers.md)

软件可能包括带有单独法律声明或受其他协议管辖的第三方组件，如软件附带的第三方声明文件所述。
