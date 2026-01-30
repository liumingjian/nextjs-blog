# Product Requirements Document: Next.js 个人技术博客

**Version**: 1.0
**Date**: 2026-01-30
**Author**: Sarah (Product Owner)
**Quality Score**: 92/100

---

## Executive Summary

本项目旨在构建一个基于 Next.js 的个人技术博客系统，为博主提供一个现代化、易于管理的内容发布平台。该博客将采用简约现代的设计风格，专注于技术文章的创作和分享。

系统将包含完整的内容管理功能，包括文章的创建、编辑、分类和标签管理，以及基于 Tiptap 的现代化富文本编辑体验。通过 SEO 优化和响应式设计，确保博客内容能够被搜索引擎良好索引，并在各种设备上提供优质的阅读体验。

采用 SQLite 数据库和 Docker 容器化部署方案，使系统轻量、易于维护和迁移，非常适合个人技术博客的使用场景。

---

## Problem Statement

**Current Situation**:
技术博主需要一个专业的平台来分享编程知识和技术文章，但现有的博客平台要么功能过于复杂，要么缺乏自定义能力，要么需要支付高额费用。

**Proposed Solution**:
构建一个自托管的 Next.js 博客系统，提供完整的内容管理功能、现代化的编辑体验和优秀的 SEO 支持，同时保持系统的轻量和易维护性。

**Business Impact**:
- 完全掌控自己的内容和数据
- 零平台费用，仅需服务器成本
- 可根据需求自由定制和扩展
- 建立个人技术品牌和影响力

---

## Success Metrics

**Primary KPIs:**

| 指标 | 目标值 | 测量方法 |
|------|--------|----------|
| 页面加载时间 | < 2秒 (首屏) | Lighthouse 性能测试 |
| SEO 评分 | > 90/100 | Lighthouse SEO 审计 |
| 移动端适配 | 100% 响应式 | 多设备测试验证 |

**Validation**:
- 部署后使用 Lighthouse 进行性能和 SEO 审计
- 在主流设备和浏览器上进行兼容性测试
- 通过 Google Search Console 监控索引状态

---

## User Personas

### Primary: 博客管理员 (博主)

- **Role**: 博客所有者和内容创作者
- **Goals**: 高效创作和发布技术文章，管理博客内容
- **Pain Points**: 需要简单直观的后台管理，不想花太多时间在技术维护上
- **Technical Level**: 高级（技术博主）

### Secondary: 访客读者

- **Role**: 博客内容消费者
- **Goals**: 快速找到感兴趣的技术文章，获得良好的阅读体验
- **Pain Points**: 页面加载慢、移动端体验差、难以找到相关内容
- **Technical Level**: 中级（技术从业者或学习者）

---

## User Stories & Acceptance Criteria

### Story 1: 用户认证

**As a** 博客管理员
**I want to** 通过密码安全登录后台
**So that** 只有我能管理博客内容

**Acceptance Criteria:**
- [ ] 提供登录页面，包含用户名/邮箱和密码输入
- [ ] 密码使用 bcrypt 加密存储
- [ ] 登录成功后生成 JWT token 或 session
- [ ] 未登录用户访问后台自动跳转到登录页
- [ ] 支持登出功能，清除认证状态

### Story 2: 文章管理 (CRUD)

**As a** 博客管理员
**I want to** 创建、编辑、删除和查看文章
**So that** 我能完整管理博客内容

**Acceptance Criteria:**
- [ ] 文章列表页显示所有文章，支持分页
- [ ] 创建文章时可设置标题、内容、分类、标签、封面图
- [ ] 支持文章草稿和发布状态切换
- [ ] 编辑已发布文章时保留原有数据
- [ ] 删除文章前需二次确认
- [ ] 支持文章排序（按时间、阅读量）

### Story 3: 分类和标签管理

**As a** 博客管理员
**I want to** 创建和管理文章分类与标签
**So that** 读者能更好地浏览和发现内容

**Acceptance Criteria:**
- [ ] 支持创建、编辑、删除分类
- [ ] 支持创建、编辑、删除标签
- [ ] 一篇文章只能属于一个分类
- [ ] 一篇文章可以有多个标签
- [ ] 前台按分类和标签筛选文章

### Story 4: 富文本编辑

**As a** 博客管理员
**I want to** 使用现代化的富文本编辑器撰写文章
**So that** 我能创作格式丰富的技术内容

**Acceptance Criteria:**
- [ ] 集成 Tiptap 编辑器
- [ ] 支持标题、段落、列表、引用等基本格式
- [ ] 支持代码块并带语法高亮
- [ ] 支持图片上传和插入
- [ ] 支持链接插入
- [ ] 编辑内容自动保存草稿

### Story 5: 文章搜索

**As a** 访客读者
**I want to** 搜索博客文章
**So that** 我能快速找到感兴趣的内容

**Acceptance Criteria:**
- [ ] 提供搜索输入框
- [ ] 支持按标题和内容搜索
- [ ] 搜索结果高亮显示匹配关键词
- [ ] 搜索结果按相关性排序

---

## Functional Requirements

### Core Features

**Feature 1: 用户认证系统**
- Description: 基于密码的管理员认证，保护后台管理功能
- User flow: 访问后台 → 跳转登录页 → 输入凭证 → 验证成功 → 进入后台
- Edge cases: 密码错误时显示错误提示，连续失败可考虑限流
- Error handling: 认证失败返回 401，token 过期自动跳转登录

**Feature 2: 文章管理系统**
- Description: 完整的文章 CRUD 功能，支持草稿和发布状态
- User flow: 后台文章列表 → 新建/编辑 → 填写内容 → 保存草稿/发布
- Edge cases: 空标题不允许发布，重复 slug 自动添加后缀
- Error handling: 保存失败时提示用户，支持重试

**Feature 3: 分类与标签系统**
- Description: 内容组织系统，支持单分类多标签
- User flow: 后台管理分类/标签 → 文章编辑时选择 → 前台按分类/标签筛选
- Edge cases: 删除分类时文章归入"未分类"，删除标签时从文章移除
- Error handling: 重复名称提示错误

**Feature 4: Tiptap 富文本编辑器**
- Description: 现代化块编辑器，支持代码高亮和图片上传
- User flow: 编辑文章 → 使用工具栏或快捷键 → 插入内容块 → 自动保存
- Edge cases: 图片上传失败时显示占位符，大图自动压缩
- Error handling: 上传失败提示重试，网络断开时本地缓存

**Feature 5: SEO 优化**
- Description: 搜索引擎优化，提升内容可发现性
- 功能点:
  - 自动生成 meta title 和 description
  - 支持自定义 URL slug
  - 自动生成 sitemap.xml
  - 结构化数据 (JSON-LD)
  - Open Graph 和 Twitter Card 支持

**Feature 6: 阅读量统计**
- Description: 记录并展示文章阅读次数
- User flow: 访客访问文章 → 后端记录访问 → 前台显示阅读量
- Edge cases: 同一用户短时间内多次访问只计一次
- Error handling: 统计失败不影响页面正常显示

### Out of Scope (MVP 不包含)
- 评论系统
- 用户注册（仅管理员）
- RSS 订阅
- 社交分享按钮
- 多语言支持
- 文章版本历史

---

## Technical Constraints

### Performance
- 首屏加载时间 < 2秒
- API 响应时间 < 200ms
- 图片懒加载和优化

### Security
- 密码使用 bcrypt 哈希存储
- JWT/Session 认证
- CSRF 防护
- XSS 防护（内容转义）
- 文件上传类型和大小限制

### Technology Stack

| 层级 | 技术选型 |
|------|----------|
| 框架 | Next.js 14+ (App Router) |
| 语言 | TypeScript |
| 数据库 | SQLite + Prisma ORM |
| 认证 | NextAuth.js |
| 编辑器 | Tiptap |
| 样式 | Tailwind CSS |
| 部署 | Docker |

---

## MVP Scope & Phasing

### Phase 1: MVP (核心功能)
- 用户认证（登录/登出）
- 文章 CRUD 管理
- 分类和标签系统
- Tiptap 富文本编辑器
- 基础 SEO 优化
- 响应式前台展示
- 文章搜索
- 阅读量统计

**MVP Definition**: 能够完成文章发布、管理和展示的完整流程

### Phase 2: 增强功能 (Post-MVP)
- 评论系统
- RSS 订阅
- 社交分享
- 文章版本历史

---

## Risk Assessment

| 风险 | 概率 | 影响 | 缓解策略 |
|------|------|------|----------|
| SQLite 并发限制 | 低 | 中 | 个人博客访问量有限，必要时可迁移到 PostgreSQL |
| 图片存储空间不足 | 中 | 中 | 实施图片压缩，监控磁盘使用 |
| SEO 效果不佳 | 中 | 中 | 遵循最佳实践，持续优化 |

---

## Data Model

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│    User     │     │    Post     │     │  Category   │
├─────────────┤     ├─────────────┤     ├─────────────┤
│ id          │     │ id          │     │ id          │
│ email       │     │ title       │     │ name        │
│ password    │     │ slug        │     │ slug        │
│ name        │     │ content     │     │ createdAt   │
│ createdAt   │     │ excerpt     │     └─────────────┘
└─────────────┘     │ coverImage  │
                    │ status      │     ┌─────────────┐
                    │ views       │     │    Tag      │
                    │ categoryId  │     ├─────────────┤
                    │ authorId    │     │ id          │
                    │ createdAt   │     │ name        │
                    │ updatedAt   │     │ slug        │
                    │ publishedAt │     │ createdAt   │
                    └─────────────┘     └─────────────┘
```

---

## Appendix

### Glossary
- **Tiptap**: 基于 ProseMirror 的现代化富文本编辑器框架
- **Prisma**: Node.js 和 TypeScript 的下一代 ORM
- **NextAuth.js**: Next.js 的认证解决方案
- **Slug**: URL 友好的文章标识符

---

*This PRD was created through interactive requirements gathering with quality scoring to ensure comprehensive coverage of business, functional, UX, and technical dimensions.*
