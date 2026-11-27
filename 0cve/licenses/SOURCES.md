# 许可证文本从哪里来

本目录里的许可证文件是从公开仓库原样复制的，用来满足这些 diff 所依据的上游许可证。它们不是本仓库新写的许可证，也不表示内核模块二进制已经通过分发审查。

| 文件 | 来源 | SHA-256 |
|---|---|---|
| `GPL-2.0.txt` | Linux `v7.3` 的 [`LICENSES/preferred/GPL-2.0`](https://github.com/torvalds/linux/blob/v7.3/LICENSES/preferred/GPL-2.0) | `8780e78a1a737e127f25a65f6d95269bffd36158dc261114de7859b490bfc5aa` |
| `Linux-syscall-note.txt` | Linux `v7.3` 的 [`LICENSES/exceptions/Linux-syscall-note`](https://github.com/torvalds/linux/blob/v7.3/LICENSES/exceptions/Linux-syscall-note) | `8e378ab93586eb55135d3bc119cce787f7324f48394777d00c34fa3d0be3303f` |
| `PHP-LICENSE-3.01.txt` | PHP 标签 `php-7.2.24` 的 [`LICENSE`](https://github.com/php/php-src/blob/php-7.2.24/LICENSE) | `f689b8fa63bea7950ce6a21bf52ed88ea0d77673ee76e6de12f51191174d91b8` |
| `ZEND-ENGINE-LICENSE-2.00.txt` | PHP 标签 `php-7.2.24` 的 [`Zend/LICENSE`](https://github.com/php/php-src/blob/php-7.2.24/Zend/LICENSE) | `509aeb3be64e428d7e0004ce224620ab01bf94adaacefbb449611939df56bc5b` |

内核树的 `COPYING` 写明内核是 `GPL-2.0 WITH Linux-syscall-note`，也就是仅 GPL-2.0，另加系统调用例外。例外的正文说明它覆盖的是用户程序通过正常系统调用使用内核，以及 UAPI 头文件。本仓库的内核 diff 修改的是内核内部网络代码，不使用该例外。

PHP 7.2.24 源文件头与上面两份许可证文本的版权年份并不写成同一行。2026-11-08 核对官方 `php-7.2.24.tar.gz` 时，`main/` 与 `ext/` 下被本 diff 碰到的文件头为 `Copyright (c) 1997-2018 The PHP Group`，并指向 PHP License 3.01；`Zend/zend_alloc.c` 与 `Zend/zend_alloc.h` 的文件头为 `Copyright (c) 1998-2018 Zend Technologies Ltd.`，并指向 Zend Engine License 2.00。diff 不删除这些文件头。应用 diff 之后必须保留它们。
