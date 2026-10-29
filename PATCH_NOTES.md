# st-chatu8 · DeepSeek 工具调用修复版

本仓库基于 [damoshen123/st-chatu8](https://github.com/damoshen123/st-chatu8)（提交 `6e15948`，版本 3.1.7）修改，原作者：从前跟你一样。
许可证沿用原项目的 Aladdin Free Public License（见 LICENSE），仅供非商业使用。

## 修改内容（仅 `index.js`）

问题：开启「工具调用」模式时，DeepSeek 等思考模型经常不调用工具，而是把结果写在正文里，或者想完就结束、正文为空。
原逻辑会把 `reasoning_content`（思考内容）拼进待解析文本；思考里常提到 `<images>` 等字样，于是报
「图片标签格式不完整」或解析不到图片。另外 DeepSeek 思考模式不支持强制 `tool_choice`，无法从请求端强制调用工具。

1. `normalizeNonStreamingToolCall`：模型未调用工具时只返回正文 `content`，不再拼接思考内容。
2. 非流式 LLM 请求的两处调用方：工具模式下只采用工具参数或正文；只有思考内容时视为空回复，交给已有的重试逻辑
   （需在设置中把 LLM 重试次数 `llm_retry_count` 设为大于 0）。

流式分支未改动。
