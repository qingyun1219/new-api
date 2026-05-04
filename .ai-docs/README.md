# new-api - 项目总览

## 项目概述
- 项目名称：new-api
- 项目简介：Go 后端 + React 前端的 API 管理与计费平台，包含用户、渠道、令牌、兑换码、订阅和支付等模块。
- 创建日期：2026-05-04_18:36:53
- 最后更新：2026-05-04_18:36:53

## 架构设计
后端以 Gin 注册 REST API，controller 处理 HTTP 请求，model 负责 GORM 数据访问，middleware 负责鉴权与限流。前端存在 default 与 classic 两套 UI，default 使用 TypeScript/React，classic 使用 JSX/Semi UI。

## 技术选型
- 后端：Go、Gin、GORM
- 前端：React、TypeScript/JavaScript、TanStack Router、react-hook-form、zod
- 数据库：通过 GORM 适配，包含 MySQL/PostgreSQL 相关处理

## 核心模块
- `controller/`：HTTP handler 与请求校验
- `model/`：数据模型与持久化逻辑
- `router/`：API 路由注册
- `middleware/`：认证、权限、限流
- `web/default/`：新版前端
- `web/classic/`：旧版前端

## 部署与配置
仓库包含 `docker-compose.dev.yml`、`.env.example`、`new-api.service`。具体运行配置需结合环境变量和数据库配置确认。