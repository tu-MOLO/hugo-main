+++
author = "MOLO"
title = "AI培智教学平台：把“一人一案”做成能跑的系统"
slug = "ai-teaching-platform"
date = "2026-10-25"
description = "面向培智（特殊教育）的个别化教育计划工作台：六领域评估、长短期目标、AI 起草教师审定、成长报告，FastAPI + React 全栈，可本地自托管。"
tags = ["项目", "FastAPI", "React", "特殊教育"]
categories = ["项目"]
image = "https://opengraph.githubassets.com/1/tu-MOLO/ai-teaching-platform"
+++

培智学校的个别化教育计划（IEP，俗称“一人一案”）长期靠 Word 模板手工维护：评估、目标、学期记录散落在不同文件里，期末要对比进步只能翻旧表。这个项目把它做成了一套可自托管的 Web 系统。

仓库：<https://github.com/tu-MOLO/ai-teaching-platform>

<!--more-->

## 解决的核心问题

| 传统做法 | 这套系统 |
|---|---|
| 评估用主观分数，无从解释 | 六领域评估 + 0–3 级掌握度评级，有明确评判口径 |
| 目标写得笼统，期末无法判断是否达成 | 短期目标必须包含条件、行为、达成标准、辅助等级 |
| 历史版本被覆盖 | 计划版本链 + 期初/期末双快照雷达对比 |
| 报告从头写 | AI 出草稿，教师逐条采纳，全程留痕 |

## 三个设计取舍

**1. 评估快照而不是实时引用**
评估模板允许教师自行配置、一键恢复默认，但每份历史计划保存的是**当时的模板快照**。这样后续改模板不会污染已经归档的评估结果。

**2. AI 只写草稿**
AI 起草 IEP 与学期成长报告时，仅生成草稿供教师逐条采纳，不能自动定稿，也不允许杜撰医学诊断。落地页面上“AI 说的”和“教师确认的”是有区别的。

**3. 不内置正式量表**
系统不带任何受版权或资质保护的量表，避免把工具做成了“评估资质”本身。

## 技术栈

**后端**：FastAPI 0.109 + SQLAlchemy 2.0（异步） + Alembic + Pydantic v2，本地默认 SQLite（aiosqlite），Docker 部署切 PostgreSQL 15；文件存储默认本地，可选 MinIO；Redis 可选。

**前端**：React 18 + TypeScript + Ant Design 5 + Vite + Zustand + ECharts。

**认证**：JWT（Access 30 分钟 / Refresh 7 天），Refresh Token 走 HttpOnly Cookie 防 XSS；5 次失败登录锁定 30 分钟；改密时通过 Token 版本化立即吊销旧令牌。

**AI**：多服务商（智谱、OpenAI、DeepSeek、Moonshot、通义千问、自定义），用户自带 API Key，密钥用 cryptography 加密存储，Function Calling 可直接操作课程、学生、教案、资源等数据，响应走 SSE 流式。

**导出**：IEP 一键导出 PDF（WeasyPrint）、教案导出 PDF/Word（python-docx）。

## 怎么跑起来

```bash
# Docker（推荐）
docker compose up -d

# 或本地开发
cd backend && pip install -r requirements.txt && uvicorn app.main:app --reload
cd frontend && npm install && npm run dev
```

仓库里还有 `start.bat` 一键本地启动，接口文档在 `/docs`。

## 适合谁

主要是培智学校、特教班的一线教师和小型教务团队——想有一个数据留在本地、不被云端平台绑定的 IEP 工具。如果你只是想要个评估量表，这个项目并不合适，它刻意不提供。
