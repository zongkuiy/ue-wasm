# ue-wasm

### unreal engine 支持 Cesium 的 wasm 打包功能调研

- 支持 Cesium 的 ue 版本   
  4.26 - 4.27, 5.0 - 5.3

- 支持 wasm 打包的 ue 版本
  - 原生支持：4.23及以下   
  已在4.23.1中尝试， 可以成功打包出 wasm 版本运行在 chrome 中
  - 非原生支持
  需要自己编译 ue， 参考   
https://www.bilibili.com/read/cv9954841/   
https://github.com/Xi3Chen/UE4.27PackingH5DDoc?tab=readme-ov-file

> 阶段性结论: 为了同时支持wasm导出和sesium， 需要自行编译一个ue版本

### ue 测试
- 5.3.2 成功运行 Cesium 示例 (2024-02-21)
- 4.26.2 不支持 wasm 打包  (2024-02-22)
- 4.23.1 成功运行 wasm 示例(不支持Cesium)  (2024-02-22)

### ue 编译记录
主要参考文档   
https://github.com/UnrealEngineHTML5/Documentation/blob/master/Platforms/HTML5/HowTo/README.md   
https://www.bilibili.com/read/cv9954841/    

0. 安装依赖的库/包/程序
   - VisualStudio2019
     - win10sdk
     - Game Devlopment with C++
     - .Net Framework 4.6.2 sdk和目标包
     - C++桌面开发
   - git
   - cmake
   - python3.x

2. 获取虚幻引擎源代码访问权限   
   https://www.unrealengine.com/zh-CN/ue-on-github
   https://docs.unrealengine.com/5.3/zh-CN/downloading-unreal-engine-source-code/

> 注意：官方库目前只有两个4.24的html版本, 暂时用下面的版本进行构建   
https://github.com/SpeculativeCoder/UnrealEngine-HTML5-ES3

2. 下载ue代码   
   编译需要的磁盘空间较大，一定放在余量超过150G的磁盘分区   
```bash
  git clone -b 4.27-html5-es3 --single-branch https://github.com/SpeculativeCoder/UnrealEngine.git ue-4.27-html5-es3
```
> 错误解决1：Git 克隆错误RPC failed; curl 56 Recv failure: Connection was reset.’   
https://www.kancloud.cn/maryong/maryong/1800760

3. 从以下地址的Assets下载Commit.gitdepth.xml并覆盖Engine/Build/Commit.gitdepth.xml
   https://github.com/EpicGames/UnrealEngine/releases/tag/4.27.2-release

4. 运行Setup.bat, 时间较长，需要下载约12G文件

5. 运行Engine/Platforms/HTML5/HTML5Setup.sh（最难搞的一步）   
> 需要安装CMake、Python3， 并正确设置环境变量 
> 注意：***最后必须出现success字样才算最终setup成功***，如果没有出现success，一般是emsdk的问题，请参考https://github.com/Xi3Chen/UE4.27PackingH5DDoc?tab=readme-ov-file#41-%E5%9C%A8%E7%BD%91%E7%BB%9C%E9%80%9A%E5%B8%B8%E6%A2%AF%E5%AD%90%E8%AE%BF%E9%97%AE%E9%80%9A%E7%95%85%E7%9A%84%E6%97%B6%E5%80%99%E8%BF%90%E8%A1%8Chtml5setupsh%E5%90%8E%E6%9C%AA%E5%87%BA%E7%8E%B0success%E6%97%B6 解决   
> powershell可能无法正确使用系统的代理， 建议使用命令手动设置代理   
> $Env:http_proxy="http://127.0.0.1:7890";$Env:https_proxy="http://127.0.0.1:7890"   
> 切换到git shell时务必设置好emsdk_env给出的环境变量，并将emsdk的PATH设置在前面
```
export PATH=/D/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.53:/D/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.53/upstream/emscripten:/D/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.53/node/16.20.0_64bit/bin:$PATH

export EMSDK=/D/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.53
export EMSDK_NODE=/D/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.53/node/16.20.0_64bit/bin/node.exe
export EMSDK_PYTHON=/D/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.53/python/3.9.2-nuget_64bit/python.exe
export JAVA_HOME=/D/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.53/java/8.152_64bit
```
emsdk安装日志如下：
```
PS D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42> .\emsdk.bat install 3.1.42
Resolving SDK version '3.1.42' to 'sdk-releases-9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-64bit'
Installing SDK 'sdk-releases-9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-64bit'..
Installing tool 'node-16.20.0-64bit'..
Downloading: D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/node-v16.20.0-win-x64.zip from https://storage.googleapis.com/webassembly/emscripten-releases-builds/deps/node-v16.20.0-win-x64.zip, 28623474 Bytes
Unpacking 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/node-v16.20.0-win-x64.zip' to 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/node/16.20.0_64bit'
Done installing tool 'node-16.20.0-64bit'.
Installing tool 'python-3.9.2-nuget-64bit'..
Downloading: D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/python-3.9.2-4-amd64+pywin32.zip from https://storage.googleapis.com/webassembly/emscripten-releases-builds/deps/python-3.9.2-4-amd64+pywin32.zip, 14413267 Bytes
Unpacking 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/python-3.9.2-4-amd64+pywin32.zip' to 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/python/3.9.2-nuget_64bit'
Done installing tool 'python-3.9.2-nuget-64bit'.
Installing tool 'java-8.152-64bit'..
Downloading: D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/portable_jre_8_update_152_64bit.zip from https://storage.googleapis.com/webassembly/emscripten-releases-builds/deps/portable_jre_8_update_152_64bit.zip, 69241499 Bytes
Unpacking 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/portable_jre_8_update_152_64bit.zip' to 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/java/8.152_64bit'
Done installing tool 'java-8.152-64bit'.
Installing tool 'releases-9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-64bit'..
Downloading: D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-wasm-binaries.zip from https://storage.googleapis.com/webassembly/emscripten-releases-builds/win/9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e/wasm-binaries.zip, 428889237 Bytes
Unpacking 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-wasm-binaries.zip' to 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/upstream'
Done installing tool 'releases-9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-64bit'.
Done installing SDK 'sdk-releases-9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-64bit'.
PS D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42> .\emsdk.bat install mingw-7.1.0-64bit
Installing tool 'mingw-7.1.0-64bit'..
Downloading: D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/mingw_7.1.0_64bit.zip from https://storage.googleapis.com/webassembly/emscripten-releases-builds/deps/mingw_7.1.0_64bit.zip, 131994877 Bytes
Unpacking 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/zips/mingw_7.1.0_64bit.zip' to 'D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42/mingw/7.1.0_64bit'
Done installing tool 'mingw-7.1.0-64bit'.
PS D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42> .\emsdk.bat activate 3.1.42
Resolving SDK version '3.1.42' to 'sdk-releases-9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-64bit'
Setting the following tools as active:
   node-16.20.0-64bit
   python-3.9.2-nuget-64bit
   java-8.152-64bit
   releases-9d73bf4bd5b5c9ce6e51be0ed5ce6599fcb28e9e-64bit

Adding directories to PATH:
PATH += D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42
PATH += D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\node\16.20.0_64bit\bin
PATH += D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\upstream\emscripten

Setting environment variables:
PATH = D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42;D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\node\16.20.0_64bit\bin;D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\upstream\emscripten;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;C:\Program Files\NVIDIA Corporation\NVIDIA NvDLISR;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;C:\WINDOWS\System32\OpenSSH\;C:\Program Files\dotnet\;C:\Program Files\Git\cmd;C:\Program Files\CMake\bin;C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python39_64\;C:\Users\COSMO\AppData\Local\Microsoft\WindowsApps;C:\Users\COSMO\.dotnet\tools
EMSDK = D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42
EMSDK_NODE = D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\node\16.20.0_64bit\bin\node.exe
EMSDK_PYTHON = D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\python\3.9.2-nuget_64bit\python.exe
JAVA_HOME = D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\java\8.152_64bit
Clearing existing environment variable: EMSDK_PY
The changes made to environment variables only apply to the currently running shell instance. Use the 'emsdk_env.bat' to re-enter this environment later, or if you'd like to register this environment permanently, rerun this command with the option --permanent.
PS D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42> .\emsdk_env.bat
Setting up EMSDK environment (suppress these messages with EMSDK_QUIET=1)
Adding directories to PATH:
PATH += D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42
PATH += D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\upstream\emscripten
PATH += D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\node\16.20.0_64bit\bin

Setting environment variables:
PATH = D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42;D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\upstream\emscripten;D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\node\16.20.0_64bit\bin;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;C:\Program Files\NVIDIA Corporation\NVIDIA NvDLISR;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;C:\WINDOWS\System32\OpenSSH\;C:\Program Files\dotnet\;C:\Program Files\Git\cmd;C:\Program Files\CMake\bin;C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python39_64\;C:\Users\COSMO\AppData\Local\Microsoft\WindowsApps;C:\Users\COSMO\.dotnet\tools
EMSDK = D:/ue-4.27-html5-es3/Engine/Platforms/HTML5/Build/emsdk/emsdk-3.1.42
EMSDK_NODE = D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\node\16.20.0_64bit\bin\node.exe
EMSDK_PYTHON = D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\python\3.9.2-nuget_64bit\python.exe
JAVA_HOME = D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42\java\8.152_64bit
Clearing existing environment variable: EMSDK_PY
PS D:\ue-4.27-html5-es3\Engine\Platforms\HTML5\Build\emsdk\emsdk-3.1.42>
```

6. 运行GenerateProjectFiles.bat

7. VisualStudio2019打开UE4.sln
   
9. 将HTML5LauncherHelper项目添加到解决方案中
   Engine\Platforms\HTML5\Source\Programs\HTML5\HTML5LaunchHelper\HTML5LauncherHelper.csproj

10. 按住Ctrl按键点击以下项目，然后点击生成选定内容。【成功】
```
UE4
AutomationTool
AutomationToolLauncher
HTML5LaunchHelper
ShaderCompileWorker
UnrealBuildTool
UnrealFrontend
UnrealHeaderTool
UnrealLightmass
UnrealPak
```

11. 运行ue4editor.exe, 启动后安装cesium for unreal插件 【成功】

12. 尝试普通项目桌面编译 【成功】

13. 尝试普通项目html5打包 【成功】   
> 如果没有html5设备，运行   
> python embuilder.py build struct_info   

14. 在原生4.27.2上安装Cesium插件

15. 在编译出来的ue4.27.2上创建一个项目并将原生4.27.2上的Cesium插件作为项目插件拷贝到项目plugins中

16. 删除项目plugins中的Binary，启动项目让ueeditor重新编译插件

17. 编译成功后得到的插件放入到ueeditor的插件库中

18. 新建Cesium项目并打包成wasm

### 其他
- 一家支持ue + Cesium + wasm的公司   
http://uipower.com/ueplus.html
