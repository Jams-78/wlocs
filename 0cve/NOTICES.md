# 分发说明

审查日期：2026-11-08。本文件记录已经能满足的义务，以及仍然挡住二进制发布的缺口。这里没有新写一份许可证，也没有把模块里的 `license=GPL` 当成已经完成的法律审查。

## 内核源码 diff

已发布的三份内核 diff 含有 Linux 内核源码片段，以及针对 `4.18.0-553.6.1.el8` 上下文的局部修改。内核公开许可证是仅 GPL-2.0。GPL-2.0 文本放在 [`licenses/GPL-2.0.txt`](licenses/GPL-2.0.txt)，来源见 [`licenses/SOURCES.md`](licenses/SOURCES.md)。

这些 diff 的补丁头里出现过 `sandbox <sandbox@local>`。这不是可供联系的版权人，本仓库不另加版权声明。

发布 diff 本身，是因为 diff 就是这份修改的源码形式，并且随附了 GPL-2.0 文本。这不表示：

- 任何 `.ko` 都可以按 GPL 再分发；
- 这些 diff 是对应 `.ko` 的完整对应源码；
- 用这些 diff 可以重新编出审查过的模块；
- 模块 `.modinfo` 里的 `license=GPL` 已经构成许可证文本、版权声明或构建脚本。

`license=GPL` 是内核模块加载器读取的 `MODULE_LICENSE` 声明。本次用 `readelf -p .modinfo` 读到了它。它只说明模块自称 GPL，不代替 GPL-2.0 要求的完整对应源码、编译安装脚本和许可证副本。

## 内核模块为什么没有放进仓库

三份 livepatch 模块都暂不提供。原因同时成立：

1. GPL-2.0 分发目标代码时，需要该可执行作品的完整对应源码，包括全部模块源码、接口定义，以及控制编译和安装的脚本。现有 diff 只有几 KB。模块是 2.3 MB 到 5.3 MB 的未剥离 ELF，含有 kpatch / create-diff-object 生成的代码。构建脚本、精确工具链、内核配置和 `Module.symvers` 都不在审查材料里。
2. CVE-2026-99999 的 diff 是重建文本，没有证明与模块逐字节对应。
3. 累积 diff 的文件名是 as-built。文件名不是可复现构建的证明。模块符号表里，函数节是 `nfqnl_build_packet_message.isra.31`，同时能看到 `isra.33` 的只读数据名。
4. CVE-2026-98432 的 diff 与公开上游提交的两处 `kfree` 到 `kfree_rcu` 修改一致，但历史记录写明模块来自手工 create-diff-object 流程，不是一次已复现的 `kpatch-build`。本次没有重编。
5. 三份模块都带调试信息。只读检查能看到构建机绝对路径，前缀在 `/root` 与 `/var/tmp`。公开树不收录这些路径。按原字节发布就会带上它们；改掉它们就会变成另一个文件，原 SHA-256 不再适用。

因此仓库里没有 `.ko`，也没有下载这些模块的链接。

## PHP 源码 diff

`php72-unofficial/` 里的 diff 含有 PHP 7.2.24 与 Zend Engine 的源码片段。随附文本：

- [`licenses/PHP-LICENSE-3.01.txt`](licenses/PHP-LICENSE-3.01.txt)，来自标签 `php-7.2.24` 的 `LICENSE`
- [`licenses/ZEND-ENGINE-LICENSE-2.00.txt`](licenses/ZEND-ENGINE-LICENSE-2.00.txt)，来自同一标签的 `Zend/LICENSE`

PHP License 3.01 要求保留的声明：

> This product includes PHP software, freely available from <http://www.php.net/software/>.

Zend Engine License 2.00 要求保留的声明：

> This product includes the Zend Engine, freely available at http://www.zend.com

> The Zend Engine is freely available at http://www.zend.com

本目录描述的是一份非官方源码缓解，给已经在自行编译 PHP 7.2.24 的人对照。它不叫做 PHP，也不是 PHP Group 或 Zend 的发布、背书或产品。目录名使用 `php72-unofficial`。补丁文件名保留原名，只用来标识它针对的上游源码包 `php-7.2.24.tar.gz`。

本地改动与所修改文件的既有许可证一起提供。仓库没有为这些改动另写许可证。没有发布 PHP 二进制，因此不存在“把 CentOS 二进制当作官方或 Debian 构建的替代品”这一说。

PHP License 3.01 第 4 条限制派生作品的命名。若后续要把这份缓解做成独立软件产品，需要先处理该条款，不能靠本文件授权使用 PHP 这个名字。
