+++
author = "MOLO"
title = "论文批量下载 Skill：从 Excel 清单到能验证的 PDF"
slug = "paper-batch-download"
date = "2026-10-25"
description = "把论文清单变成完整 PDF 库：DOI 补全、多源开放获取并发下载、Sci-Hub 兜底、浏览器辅助机构下载，自动生成复核表并做完整性审计。"
tags = ["项目", "Python", "科研工具", "自动化"]
categories = ["项目"]
image = "https://opengraph.githubassets.com/1/tu-MOLO/paper-batch-download-skill"
+++

写综述时最耗时的不是读，而是把几十上百篇文献一个个找齐、命名、确认下到的不是坏文件。这个 SKILL 把这条流水线自动化了。

仓库：<https://github.com/tu-MOLO/paper-batch-download-skill>

<!--more-->

## 工作流

```text
论文清单 (xlsx / csv / tsv)
    │
    ├─ DOI 补全      Crossref / OpenAlex / Semantic Scholar
    ├─ OA 下载       arXiv、Unpaywall、OpenAlex、Semantic Scholar、Europe PMC、DOAJ
    ├─ 兜底          OA 全无结果时回退 Sci-Hub
    ├─ 机构访问      浏览器辅助：用户自己登录后手动点下载，脚本负责校验、改名、归档
    ├─ 完整性校验     magic bytes + 最小体积 + pypdf 三重检查
    └─ 审计与复核    未成功的记录生成带样式的 Excel 复核表
```

## 几个关键设计

**并发只在 OA 层，不做无谓重试**
`collect_open_access_multi.py` 支持 `fast` 与 `exhaustive` 两种策略：前者命中即停，后者把所有源都问一遍取最优。请求带 `mailto`，走各家的礼貌池。

**把“拿不到”交回给人**
机构订阅的论文脚本没权限，硬爬只会被封。所以 `browser_assisted_download.py` 只做它擅长的事：打开 DOI 页面、等你手动下载、然后从下载目录里自动认领文件、校验、按 `(年份_引用数引用) 题名.pdf` 重命名归档。

**校验比下载更重要**
只判断 HTTP 200 是不够的——校园网门户经常返回一个登录页 HTML 存成 `.pdf`。所以 magic bytes、最小大小、pypdf 能否解析三层都过才算成功，避免最后打开一看全是空壳。

**清单即状态**
所有中间结果写在 `论文清单.json` 里（状态、SHA256、下载记录），可以中断续跑，`audit_integrity.py` 再做 7 项检查：状态完整性、哈希一致、重复、孤立 PDF、计数、引用覆盖、下载记录。

## 目录结构

```text
scripts/
├── collect_open_access_multi.py   # 主入口：多源 OA + Sci-Hub 回退
├── browser_assisted_download.py   # 机构登录后辅助下载
├── enrich_manifest_dois.py        # 补全缺失 DOI
├── generate_review_excel.py       # 复核表
├── rename_organize.py             # 规范化重命名
├── audit_integrity.py             # 完整性审计
└── paper_common.py                # 匹配、状态、样式等共享逻辑
```

## 使用前提

需要机构订阅的论文，请用浏览器辅助模式**以你自己的权限**下载；Sci-Hub 兜底是否可用、是否合规，取决于你所在地区与所属机构的规定，请自行判断并使用。工具只负责整理，不替你获得权限。

```bash
pip install requests pypdf pandas openpyxl xlrd

python scripts/collect_open_access_multi.py papers.xlsx \
  --output-dir .\论文PDF --state-file .\论文清单.json \
  --mailto you@example.edu --strategy fast
```
