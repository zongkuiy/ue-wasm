# ue-wasm

### unreal engine 支持 Cesium 的 wasm 打包功能调研

- 支持 Cesium 的 ue 版本   
  4.26 - 4.27, 5.0 - 5.3

- 支持 html5 打包的 ue 版本
  - 原生支持：4.23及以下   
  已在4.23.2中尝试， 可以成功打包出 wasm 版本运行在 chrome 中
  - 非原生支持
  需要自己编译 ue， 参考   
https://www.bilibili.com/read/cv9954841/   
https://github.com/Xi3Chen/UE4.27PackingH5DDoc?tab=readme-ov-file

> 阶段性结论: 为了同时支持wasm导出和sesium， 需要自行编译一个ue版本

### ue 编译记录
1. 获取虚幻引擎源代码访问权限   
   https://www.unrealengine.com/zh-CN/ue-on-github   
![image](https://github.com/zongkuiy/ue-wasm/assets/3311506/c5793b08-9db7-453d-aaac-c0b1822f168c)

2. 
