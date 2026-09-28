# 来源与第三方说明

`cpp/main.cc`、`cpp/qwen3_vl.*`、`cpp/llm/`、`cpp/vision/` 和工具代码来源于
Rockchip RKNN3 Model Zoo 示例。各文件原有的版权及许可声明保持有效。
带 Apache-2.0 声明的文件须遵循 Apache License 2.0。

本工作区的构建调整包括：将 Model Zoo 外部依赖路径改为本地 `cpp/3rdparty`；
安装图片路径改为 `cpp/data/demo.jpg`；将无需使用的音频工具设为默认关闭的构建选项。

本仓库不发布 RKNN3 SDK 二进制、模型权重或未核实再分发许可的第三方依赖。
使用者应从有权访问的 SDK / Model Zoo 获取依赖，并遵守其各自许可。
本说明不向第三方软件或模型授予额外许可。
