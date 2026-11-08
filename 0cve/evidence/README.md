# 证据范围

[`module-metadata.txt`](module-metadata.txt) 是三份内核模块 `.modinfo` 的原样摘录，SHA-256 `d15396e73e0aaab92f25e6fc9e0f9b895a09ce097e979e24039791b6118d2760`。2026-11-08 用 `readelf -p .modinfo` 对同一批字节复核，vermagic、`livepatch=Y`、`license=GPL`、`rhelversion=8.10` 和模块名与该文件一致。

历史测试正文写在各 CVE 指南里，并标明本次没有重跑。原始日志、构建目录、虚拟机磁盘和内部审查包没有放进仓库。指南不收录构建机绝对路径。
