# RK Qwen3-VL Optimization

面向 RK1828 的 Qwen3-VL C++ 推理优化项目，重点关注多轮对话、图像预处理和 Token 输出。

## 代码修改

- **多轮对话**：复用 Session 和 KV Cache，后续轮次仅传入新增文本，避免重复执行 Vision。
- **图像预处理**：将 RGA 目标 Handle 的创建与释放放到模型生命周期中，减少每次图像转换的重复操作。
- **异步 Token 输出**：将分词解码和终端打印移到后台线程，推理回调负责记录首 Token 时间和提交 Token 队列。
- **性能计时**：分别统计 Vision 预处理、输入同步、NPU 推理、输出同步及复制；分开显示 Decode 主路径和输出排空时间。

这些说明涵盖基础代码及后续优化迭代。当前仓库代码以已提交的基础版本为准。

## 优化程度

相对于上一版 RGA 直写实现，目标 Handle 常驻后的结果为：

| 指标 | 上一版直写 | Handle 常驻 | 耗时减少 |
| --- | ---: | ---: | ---: |
| 图像预处理 | 3.920 ms | 3.357 ms | **14.4%** |
| Vision 总耗时 | 102.778 ms | 102.345 ms | **约 0.4%** |

上一版数据为两次平均，常驻版本数据为单次测量。
14.4% 对应图像预处理阶段的耗时减少。
测试条件：RK1828、384 × 384、完整 NHWC Vision 模型、首轮 191 个 Prefill Token。

具体代码调整见 [代码修改与优化程度](OPTIMIZATION_RESULTS.md)。

## 源码结构

- `cpp/main.cc`：入口参数、回调与性能输出。
- `cpp/qwen3_vl.cc`：Vision 与 LLM 调度。
- `cpp/llm/`：LLM Session 与推理流程。
- `cpp/vision/`：图像预处理与 Vision 推理。
- `cpp/utils/`：图像、文件及计时工具。

## 许可

保留 Rockchip 示例代码的原始版权声明。许可及第三方说明见
[LICENSE](LICENSE) 和 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
