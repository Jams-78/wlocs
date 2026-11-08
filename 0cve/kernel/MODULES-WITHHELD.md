# 内核模块：暂不提供

2026-11-08 用 `readelf` 与 SHA-256 核对过下列三个 ELF。它们保持原字节，没有加载，没有 `insmod`。仓库不提供下载。

三者的 vermagic 都是：

`4.18.0-553.6.1.el8.x86_64 SMP mod_unload modversions`

`.modinfo` 还有 `livepatch=Y`、`license=GPL`、`rhelversion=8.10`。这个 vermagic 只对应这一版内核构建。它不覆盖 CentOS 7，也不覆盖其他 EL8 或 CentOS Stream 8 内核。

| 模块名 | 字节数 | SHA-256 | Build ID (sha1) | 只读符号 |
|---|---:|---|---|---|
| `kpatch_cve98432` | 2378680 | `ca98b2d13cac13ba93ac7b88ed418420b6c258c234539f945d6166222b31c202` | `34946b5d98b3799e35bd6ab204f4ebd4d6b66f4e` | `inet_sock_destruct`、`tcp_v4_reqsk_destructor`，未定义符号 `kvfree_call_rcu` |
| `livepatch_cve_2026_99999` | 3541256 | `5d312d208dd6758902707cc86a62a77f1bd06737b8a6490e39a80b7e0c3ad894` | `0d37ce2232a3679b429c71cf6b520126eef4bd49` | `ip_do_fragment`、`xfrm_output`、`nfqnl_build_packet_message.isra.31`、`checksum_tg`，未定义符号 `skb_checksum_help` |
| `livepatch_cve_2026_99999_98432` | 5523784 | `a4efcea8aca6da9c93f60d8bef25c947146064de36ba5d78ac29425b0c144847` | `02b7f08fc066aa247036b038d085f208b20fcd2e` | 上表两组函数都在，未定义符号含 `kvfree_call_rcu` 与 `skb_checksum_help` |

累积模块的哈希是 `a4efcea8…`。审查材料里没有另一份 stage2 / KB 构建（历史记录里曾出现过另一个哈希前缀 `4a4f00e6`）。那份文件不在这里，也不能从本页推断它的内容。

## 挡住发布的条件

完整说明见 [`../NOTICES.md`](../NOTICES.md)。要点：

- `license=GPL` 只是 `.modinfo` 里的声明。GPL-2.0 分发这些目标文件所需要的完整对应源码和构建脚本并不在材料里。
- CVE-2026-99999 的 diff 是重建的，没有证明与 `5d312d20…` 逐字节对应。
- 累积 diff 自称为 as-built，不能充当可复现构建证明。符号表同时出现 `isra.31` 与 `isra.33`。
- 调试信息仍含构建机绝对路径。公开这些字节就会公开那些路径。

## 不能叠加

三个模块名各不相同，内核不会因为名字相同而拒绝同时加载。历史记录把累积模块写成带替换语义的后继构建，用来换下旧的 `livepatch_cve_2026_99999`。旧的 `kpatch_cve98432` 与 `livepatch_cve_2026_99999` 也不能和累积模块叠在一起。

本仓库没有可加载的文件，所以这里不写 `insmod`、`rmmod` 或 kpatch 命令。若某处已经持有这些私有副本：同一时刻只保留一个目标模块，先等 livepatch 的 `transition` 结束，再处理下一个。历史记录里 `kpatch unload` 命令曾卡住，手动写 sysfs 的 `enabled` 才卸下。那是旧环境里的观察，不是本仓库提供的操作步骤。
