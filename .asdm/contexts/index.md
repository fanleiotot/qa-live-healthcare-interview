# QA Live Healthcare 工作区索引

## 📋 项目概述

**QA Live Healthcare** 是一个基于 Vue 3 + TypeScript + Vite 构建的在线医疗咨询平台。该项目提供医患在线咨询、医生管理等功能，旨在为医疗行业提供高质量的在线服务解决方案。

### 项目基本信息
- **项目名称**: QA Live Healthcare
- **技术栈**: Vue 3 + TypeScript + Vite + Ant Design Vue
- **项目类型**: 单页应用 (SPA)
- **开发语言**: TypeScript
- **UI框架**: Ant Design Vue

## 🏗️ 技术架构

### 核心技术栈
- **前端框架**: Vue 3.5.10
- **开发语言**: TypeScript 5.5.3
- **构建工具**: Vite 5.4.8
- **UI组件库**: Ant Design Vue 4.2.6
- **路由管理**: Vue Router 4.6.3
- **日期处理**: Day.js 1.11.19

### 开发工具
- **包管理**: npm
- **TypeScript编译**: vue-tsc
- **热重载**: Vite 开发服务器

## 📁 项目结构

```
qa-live-healthcare/
├── src/                    # 源代码目录
│   ├── assets/            # 静态资源
│   ├── components/        # 通用组件
│   ├── data/              # 数据文件
│   ├── router/            # 路由配置
│   ├── store/             # 状态管理
│   ├── views/             # 页面视图
│   ├── App.vue            # 根组件
│   ├── main.ts            # 应用入口
│   ├── style.css          # 全局样式
│   └── vite-env.d.ts      # Vite 类型定义
├── public/                # 公共资源
├── package.json           # 项目配置
├── vite.config.ts         # Vite 配置
├── tsconfig.json          # TypeScript 配置
└── README.md              # 项目说明
```

## 🚀 功能模块

### 页面路由
项目包含以下主要页面：

| 页面名称 | 路由路径 | 功能描述 |
|---------|---------|---------|
| 首页 | `/` | 应用首页展示 |
| 咨询页面 | `/consultation` | 在线咨询功能 |
| 医生详情咨询 | `/consultation/:doctorUsername` | 指定医生咨询 |
| 医生列表 | `/doctors` | 医生信息展示 |
| 关于页面 | `/about` | 项目介绍 |
| 医生登录 | `/doctor/login` | 医生登录入口 |
| 医生诊室 | `/doctor/room/:username` | 医生工作台 |

### 核心功能
1. **用户咨询功能** - 患者与医生在线交流
2. **医生管理** - 医生登录和诊室管理
3. **路由导航** - 单页应用路由管理
4. **响应式UI** - 基于 Ant Design Vue 的现代化界面

## 🔧 开发配置

### 构建命令
```bash
npm run dev      # 开发环境启动
npm run build    # 生产环境构建
npm run preview  # 构建预览
```

### 开发环境
- **端口**: Vite 默认端口 (通常为 5173)
- **热重载**: 支持文件修改自动刷新
- **TypeScript**: 严格类型检查

## 📚 上下文文件索引

### 可用的上下文文件
以下是本工作区的完整上下文文档，建议按需查看：

1. **[标准项目结构](standard-project-structure.md)** - 详细的项目目录结构说明
2. **[编码规范](standard-coding-style.md)** - 代码风格和最佳实践指南
3. **[数据模型](data-models.md)** - 应用数据结构和关系
4. **[部署配置](deployment.md)** - 部署环境和流程说明
5. **[API接口](api.md)** - API 定义和文档
6. **[系统架构](architecture.md)** - 整体架构设计说明

### 快速开始
- 对于新开发者，建议先阅读 **[标准项目结构](standard-project-structure.md)**
- 需要了解代码规范时，查看 **[编码规范](standard-coding-style.md)**
- 进行功能开发前，参考 **[数据模型](data-models.md)** 和 **[API接口](api.md)**

## 🔗 相关资源

### 技术文档
- [Vue 3 官方文档](https://vuejs.org/)
- [TypeScript 文档](https://www.typescriptlang.org/)
- [Vite 文档](https://vitejs.dev/)
- [Ant Design Vue 文档](https://antdv.com/)

### 开发工具
- 代码编辑器: 支持 TypeScript 和 Vue 的 IDE
- 浏览器: Chrome/Firefox/Safari 现代浏览器
- Node.js: 版本 16+ (推荐 18+)

---

*最后更新: 2026年4月21日*  
*本文档由 Context Builder 工具集自动生成*