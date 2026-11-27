# 0cve 审查索引

这个目录是 `Jams-78/wlocs` 审查分支上的发布说明。它还没有合并进 `main`，也没有 GitHub Release。`gh-pages` 上的现有站点没有改动。

2026-11-08 核对了公开 CVE 记录、上游提交、PHP 公告、官方 `php-7.2.24.tar.gz`，以及审查材料里的字节。历史测试没有重跑。内核模块没有加载。

许可证和暂不发布的原因写在 [`NOTICES.md`](NOTICES.md)。机器可读目录是 [`catalog.json`](catalog.json)。

## 现在仓库里有的文件

| 指南 | 工件 | 类型 | SHA-256 |
|---|---|---|---|
| [CVE-2026-98432](kernel/cve-2026-98432/README.md) | [kfree_rcu diff](kernel/cve-2026-98432/cve-2026-98432-kfree_rcu.patch) | 内核源码 diff | `d83b4f3661c4d9fde90570ed2ed58fea6c765bd09b9424912c1aacf8f06412bb` |
| [CVE-2026-99999](kernel/cve-2026-99999/README.md) | [重建 diff](kernel/cve-2026-99999/rhel8-backport-reconstructed.patch) | 未与模块对上的重建文本 | `124bb4415346aa0a2e41ade18f915c41eebd5f2d567e5b37133d002ea66fa0f2` |
| [累积记录](kernel/cve-2026-99999-98432-cumulative/README.md) | [as-built diff](kernel/cve-2026-99999-98432-cumulative/combined-cve-2026-99999-98432.as-built.patch) | 记录用源码 diff，不是复现证明 | `f634fa0a1d9c7c90f6d46f08207e871825d3b1e68bf11d9ca95096760ad5c5bc` |
| [PHP 7.2.24 非官方缓解](php72-unofficial/cve-2026-109618-109619/README.md) | [源码 diff](php72-unofficial/cve-2026-109618-109619/php-7.2.24-cve-2026-109618-109619.patch) | 需重编译的源码缓解 | `3b727991f308551db0fb4a9b4ea2ae143d4ea415597fab7e07535a80fa9280bf` |

## 暂不提供

三份内核 livepatch 模块都不在仓库里，没有下载链接。标识、哈希和原因见 [内核模块：暂不提供](kernel/MODULES-WITHHELD.md)。

| 模块 | SHA-256 | 状态 |
|---|---|---|
| `kpatch_cve98432` | `ca98b2d13cac13ba93ac7b88ed418420b6c258c234539f945d6166222b31c202` | 暂不提供 |
| `livepatch_cve_2026_99999` | `5d312d208dd6758902707cc86a62a77f1bd06737b8a6490e39a80b7e0c3ad894` | 暂不提供 |
| `livepatch_cve_2026_99999_98432` | `a4efcea8aca6da9c93f60d8bef25c947146064de36ba5d78ac29425b0c144847` | 暂不提供 |

同时不在本仓库的还有：stage2 / KB 模块、累积 diff 的 inline 变体、PHP 二进制、EL8 安装脚本、原始测试日志。

## 适用范围

内核材料只对应 vermagic：

`4.18.0-553.6.1.el8.x86_64 SMP mod_unload modversions`

这是 CentOS Stream 8 上这一版内核的历史测试对象。它不覆盖 CentOS 7，也不覆盖其他 EL8 内核。

累积模块用来替换 `kpatch_cve98432` 和 `livepatch_cve_2026_99999`。三个模块名不同，不能叠着加载。本仓库没有这些模块。

PHP 材料只针对官方 `php-7.2.24.tar.gz`（SHA-256 `01baf7a34c856d2c552121fbad7296a8cde18389ce83db32f18252bc1cee4dd6`，2026-11-08 从 php.net 下载后核对）。它是非官方部分缓解。官方修复版本是 8.2.35、8.3.36、8.4.27、8.5.12。用它之前要重新编译，并重启全部 worker。

## 本次审查实际做了的检查

- CVE-2026-98432、CVE-2026-99999 在 MITRE 的记录为 PUBLISHED，标题与本地主题一致。98432 的上游两行 `kfree_rcu` 改动已与 e3f33b0a1d89 对照。99999 的上游提交 6785011f8b16 比本地重建 diff 多 `nf_conntrack_bridge.c`。
- PHP 公告列出 CVE-2026-109618 / 109619，以及 8.2.35 及以上修复版本；7.4、8.0、8.1 才是官方提到的 EOL 源码缓解。
- 三份 `.ko` 的 SHA-256、vermagic 和符号用原字节复核。模块未进入仓库。
- PHP diff 对官方 7.2.24 源码 `patch --dry-run --fuzz=0` 退出码为 0。没有编译。

历史测试的数字写在各指南中。那些测试不是这次做的。
