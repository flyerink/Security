* 名称：TCG 兼容测量日志
* 日期：2021-05-07
* 拉取请求：[#17](https://github.com/opencomputeproject/Security/pull/17)

# 客观的

Cerberus 挑战协议提供了多种访问测量和测量数据的机制。这包括证明日志，它生成一个单独的测量列表。虽然此结构的灵感来自 TCG 样式的日志记录，但它与该格式的解析器不兼容。从证明的角度来看，能够从设备获取完全合规的 TCG 日志是有利的。

这可以为 TPM 证明流程提供一些统一性，尤其是与 Unseal 命令配对时。

# 提议

更新获取日志命令以支持额外的日志类型，该类型将提供设备生成的 TCG 日志。此时不会更新获取日志信息命令，因为日志大小，尤其是 TCG 日志，在实践中并不那么相关。

与 Get Log 命令是可选命令一样，并不要求每个设备都支持生成符合 TCG 标准的日志。

# 规范变更列表

将日志类型“4”添加到 TCG 日志支持类型表中。Get Log 命令的描述性文本也需要更新以反映新类型的添加。

日志结构在 TCG PC 客户端特定平台固件配置文件规范的第 10 节（事件日志记录）中定义：https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification

# 实施指南

虽然生成和维护完整的 TCG 日志可能看起来很复杂，但实际上可以通过一种相当节省内存的方式来实现。即使日志包含具有大量数据的事件。无需在内存或存储中制作数据副本和格式化日志，日志可以通过注册的回调处理程序轻松按需重建，这些回调处理程序可以从内存、闪存或其他任意位置提取数据。对于实施获取证明数据命令的设备尤其如此，因为此命令将报告进入 TCG 日志的相同事件数据。

可以在此处的开源 Cerberus 源代码中找到示例实现：https://github.com/Azure/Project-Cerberus/blob/master/core/attestation/pcr_store.c#L544
