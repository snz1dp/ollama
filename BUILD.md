## 1、安装必要工具

- cmake版本3.24或更高
- go版本1.22或更高
- gcc版本11.4.0或更高

```bash
brew install go cmake gcc
```

> 可选打开调试和更详细日志:

```bash
export CGO_CFLAGS="-g"
export OLLAMA_DEBUG=1
```

> 获取所需库并构建本地LLM代码:

```bash
go generate ./
```

> 然后构建Ollama:

```bash
go build .
```

> 现在可以运行ollama:

```bash
./ollama
```

### 1.2、Linux

#### 1.2.1、Linux CUDA(NVIDIA)

您的操作系统发行版可能已经提供NVIDIA CUDA软件包。发行版软件包通常更可取,但每个发行版的安装步骤可能不同。如果有的话请参考发行版相关文档获取依赖项信息!

安装cmake、golang以及NVIDIA CUDA开发和运行时软件包。

通常构建脚本会自动检测CUDA,但是,如果您的Linux发行版或安装方式使用非常规路径,您可以通过设置环境变量CUDA_LIB_DIR指定共享库位置,CUDACXX指定nvcc编译器位置来指定路径。您也可以通过设置CMAKE_CUDA_ARCHITECTURES(例如"50;60;70")来自定义CUDA架构目标。

然后生成依赖:

```bash
go generate ./
```

然后构建二进制文件:

```bash
go build .
```

#### 1.2.2、Linux ROCm(AMD)

您的操作系统发行版可能已经提供AMD ROCm和CLBlast软件包。发行版软件包通常更可取,但每个发行版的安装步骤可能不同。如果有的话请参考发行版相关文档获取依赖项信息!

首先安装CLBlast和ROCm开发软件包,以及cmake和golang。

通常构建脚本会自动检测ROCm,但是,如果您的Linux发行版或安装方式使用非常规路径,您可以通过设置环境变量ROCM_PATH指定ROCm安装位置(通常是/opt/rocm),CLBlast_DIR指定CLBlast安装位置(通常是/usr/lib/cmake/CLBlast)来指定路径。您也可以通过设置AMDGPU_TARGETS(例如AMDGPU_TARGETS="gfx1101;gfx1102")来自定义AMD GPU目标。


```bash
go generate ./
```

然后构建二进制文件:

```bash
go build .
```

ROCm运行时需要提升权限访问GPU。在大多数发行版上,您可以将用户账号添加到render组,或者直接以root用户运行。

#### 1.2.3、高级CPU设置

默认情况下,运行`go generate ./...`会根据常见CPU类型和矢量计算能力编译几个LLM库的变体,包括一个兼容大多数64位CPU但性能较低的通用版本。
运行时,Ollama会自动检测并加载最优版本。如果您想为特定处理器自定义编译,可以设置OLLAMA_CUSTOM_CPU_DEFS环境变量指定要使用的llama.cpp编译参数。
例如,为Intel i9-9880H优化:

```bash
OLLAMA_CUSTOM_CPU_DEFS="-DLLAMA_AVX=on -DLLAMA_AVX2=on -DLLAMA_F16C=on -DLLAMA_FMA=on" go generate ./...
go build .
```

### 1.3、Docker

如果您有Docker,可以使用./scripts/build_linux.sh在容器中构建Linux二进制文件。构建结果会放在./dist文件夹中。

### 1.4、Windows

> 注意:Ollama在Windows上的构建还在开发中。

安装必需工具:

- MSVC工具链 - C/C++和cmake作为最小要求 - 必须从设置了环境变量的“Developer Shell”构建
- [Windows CUDA(NVIDIA)](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html)
- go版本1.22或更高
- MinGW(选择一个版本)带GCC
  <https://www.mingw-w64.org/>
  <https://www.msys2.org/>

> 除了上述常见Windows开发工具外,还需要安装AMD的HIP包在安装MSVC后安装。

[AMD HIP]<https://www.amd.com/en/developer/resources/rocm-hub/hip-sdk.html>
