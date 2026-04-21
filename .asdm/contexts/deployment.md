# QA Live Healthcare 部署配置文档

## 1. 文档概述

### 1.1 文档目的

本文档详细描述了 QA Live Healthcare 项目的部署配置流程、环境要求、构建优化和运维指南，为开发团队和运维人员提供完整的部署参考。

### 1.2 适用范围

- 开发环境部署
- 测试环境部署
- 生产环境部署
- 云平台部署（阿里云、腾讯云等）
- 容器化部署（Docker）

### 1.3 技术栈概览

| 技术类别 | 技术选型 | 版本要求 | 部署环境 |
|---------|---------|---------|---------|
| 前端框架 | Vue 3 | 3.5+ | 所有环境 |
| 构建工具 | Vite | 5.4+ | 所有环境 |
| UI 组件库 | Ant Design Vue | 4.2+ | 所有环境 |
| 开发语言 | TypeScript | 5.5+ | 所有环境 |
| Node.js | Node.js | 18.0+ | 构建服务器 |
| 包管理器 | npm | 9.0+ | 构建服务器 |

---

## 2. 环境准备

### 2.1 服务器环境要求

#### 2.1.1 开发环境要求

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            开发环境配置要求                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                        最低配置 (Development)                         │    │
│  ├──────────────────────────────────────────────────────────────────────┤    │
│  │  CPU:        2 核心                                                    │    │
│  │  内存:       4 GB                                                      │    │
│  │  磁盘:       10 GB 可用空间                                            │    │
│  │  操作系统:   Windows 10+ / macOS 10.14+ / Ubuntu 18.04+               │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                        推荐配置 (Development)                         │    │
│  ├──────────────────────────────────────────────────────────────────────┤    │
│  │  CPU:        4 核心或以上                                              │    │
│  │  内存:       8 GB 或以上                                               │    │
│  │  磁盘:       20 GB 可用空间 (SSD 推荐)                                 │    │
│  │  操作系统:   Windows 11 / macOS 12+ / Ubuntu 22.04+                 │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### 2.1.2 生产环境要求

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            生产环境配置要求                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                        最低配置 (Production - 小型)                   │    │
│  ├──────────────────────────────────────────────────────────────────────┤    │
│  │  CPU:        2 核心                                                    │    │
│  │  内存:       2 GB                                                      │    │
│  │  磁盘:       5 GB 可用空间                                             │    │
│  │  带宽:       1 Mbps                                                    │    │
│  │  说明:       适用于日活 < 1000 的场景                                  │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                        中型配置 (Production - 中型)                  │    │
│  ├──────────────────────────────────────────────────────────────────────┤    │
│  │  CPU:        4 核心                                                    │    │
│  │  内存:       4 GB                                                      │    │
│  │  磁盘:       20 GB 可用空间                                            │    │
│  │  带宽:       5 Mbps                                                    │    │
│  │  说明:       适用于日活 1000-10000 的场景                              │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                        大型配置 (Production - 大型)                  │    │
│  ├──────────────────────────────────────────────────────────────────────┤    │
│  │  CPU:        8 核心或以上                                              │    │
│  │  内存:       8 GB 或以上                                               │    │
│  │  磁盘:       50 GB 可用空间 (SSD)                                      │    │
│  │  带宽:       10 Mbps 或以上                                            │    │
│  │  说明:       适用于日活 > 10000 的场景，需要 CDN 加速                   │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 软件环境安装

#### 2.2.1 Node.js 安装

**Windows 系统安装：**

```powershell
# 方法一：使用 Node.js 官网下载安装包
# 访问 https://nodejs.org/ 下载 LTS 版本

# 方法二：使用 Chocolatey 安装
choco install nodejs-lts

# 方法三：使用 NVM 管理多版本 Node.js
# 安装 NVM for Windows
scoop install nvm

# 安装指定版本
nvm install 20.10.0
nvm use 20.10.0
```

**macOS 系统安装：**

```bash
# 方法一：使用 Homebrew 安装
brew install node@20

# 方法二：使用 NVM 管理
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 20.10.0
nvm use 20.10.0
```

**Linux 系统安装：**

```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# CentOS/RHEL
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
sudo yum install -y nodejs

# 验证安装
node --version    # 应显示 v20.x.x
npm --version     # 应显示 10.x.x
```

#### 2.2.2 pnpm 安装（可选，推荐）

```bash
# 使用 npm 全局安装
npm install -g pnpm

# 或使用 Corepack 启用
corepack enable
corepack prepare pnpm@latest --activate

# 验证安装
pnpm --version
```

---

## 3. 项目构建配置

### 3.1 构建命令详解

```json
{
  "scripts": {
    "dev": "vite",                    // 开发环境启动
    "build": "vue-tsc -b && vite build",  // 生产环境构建
    "preview": "vite preview"         // 预览生产构建
  }
}
```

#### 3.1.1 开发环境构建

```bash
# 启动开发服务器
npm run dev

# 输出信息
#   VITE v5.4.8  ready in 1234 ms
#   ➜  Local:   http://localhost:5173/
#   ➜  Network: http://192.168.1.100:5173/
#   ➜  press h + enter to show help
```

#### 3.1.2 生产环境构建

```bash
# 执行生产构建
npm run build

# 构建输出示例
# vite v5.4.8 building for production...
# ✓ 45 modules transformed.
# dist/index.html                 0.46 kB │ gzip:  0.30 kB
# dist/assets/index-BKMKpSDA.css  2.34 kB │ gzip:  0.87 kB
# dist/assets/index-CkxK9Hda.js  45.67 kB │ gzip: 15.23 kB
# ✓ built in 2.34s
```

#### 3.1.3 预览生产构建

```bash
# 在本地预览构建结果
npm run preview

# 输出信息
#   VITE v5.4.8  ready in 567 ms
#   ➜  Local:   http://localhost:4173/
```

### 3.2 Vite 配置优化

```typescript
// vite.config.ts 优化配置
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import path from 'path';

export default defineConfig({
  plugins: [vue()],
  
  // 基础路径配置
  base: './',  // 相对路径部署，适用于多种部署场景
  
  // 生产构建配置
  build: {
    // 目标浏览器
    target: 'es2015',
    
    // 输出目录
    outDir: 'dist',
    
    // 资源目录
    assetsDir: 'assets',
    
    // CSS 代码分割
    cssCodeSplit: true,
    
    // 子完整性检查
    subresourceIntegrity: true,
    
    // 构建压缩
    minify: 'terser',
    terserOptions: {
      compress: {
        drop_console: true,      // 生产环境移除 console
        drop_debugger: true      // 移除 debugger
      }
    },
    
    // 资源内联限制
    assetsInlineLimit: 4096,     // 4KB 以下的资源内联
    
    // 分割 chunks
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor': ['vue', 'vue-router'],
          'ant-design': ['ant-design-vue']
        },
        chunkFileNames: 'assets/js/[name]-[hash].js',
        entryFileNames: 'assets/js/[name]-[hash].js',
        assetFileNames: 'assets/[ext]/[name]-[hash].[ext]'
      }
    },
    
    // 生成 sourcemap
    sourcemap: false,  // 生产环境关闭以减小体积
    
    // 开启 gzip 压缩
    chunkSizeWarningLimit: 500   // KB
  },
  
  // 开发服务器配置
  server: {
    port: 5173,
    host: true,           // 允许外部访问
    open: true,           // 自动打开浏览器
    cors: true,           // 允许跨域
    
    // 代理配置
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, '')
      }
    }
  },
  
  // 路径别名配置
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src'),
      '@components': path.resolve(__dirname, 'src/components'),
      '@views': path.resolve(__dirname, 'src/views'),
      '@store': path.resolve(__dirname, 'src/store'),
      '@data': path.resolve(__dirname, 'src/data'),
      '@assets': path.resolve(__dirname, 'src/assets'),
      '@router': path.resolve(__dirname, 'src/router')
    }
  },
  
  // 预览服务器配置
  preview: {
    port: 4173,
    host: true,
    cors: true
  }
});
```

### 3.3 TypeScript 构建配置

```json
// tsconfig.app.json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    
    // 跳过库检查，提高构建速度
    "skipLibCheck": true,
    
    // 模块解析策略
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    
    // 不输出文件，用于 vue-tsc 检查
    "noEmit": true,
    
    // JSX 配置
    "jsx": "preserve",
    
    // 严格模式
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true,
    
    // 路径别名
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"],
      "@components/*": ["./src/components/*"],
      "@views/*": ["./src/views/*"],
      "@store/*": ["./src/store/*"],
      "@data/*": ["./src/data/*"],
      "@assets/*": ["./src/assets/*"],
      "@router/*": ["./src/router/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.tsx", "src/**/*.vue"]
}
```

---

## 4. 多环境配置

### 4.1 环境变量文件结构

```
项目根目录
├── .env                    # 默认环境变量（所有环境加载）
├── .env.development        # 开发环境变量
├── .env.test               # 测试环境变量
├── .env.staging            # 预发布环境变量
├── .env.production         # 生产环境变量
└── .env.local             # 本地覆盖（不提交到版本控制）
```

### 4.2 环境变量定义

#### 4.2.1 默认环境变量 (.env)

```env
# 应用基础配置
VITE_APP_TITLE=QA Live Healthcare
VITE_APP_VERSION=1.0.0

# API 基础地址
VITE_API_BASE_URL=/api

# 应用特性开关
VITE_ENABLE_ANALYTICS=false
VITE_ENABLE_ERROR_LOGGING=true

# 默认语言
VITE_DEFAULT_LOCALE=zh-CN
```

#### 4.2.2 开发环境变量 (.env.development)

```env
# 开发环境 API 地址
VITE_API_BASE_URL=http://localhost:3000/api

# 开启调试模式
VITE_DEBUG_MODE=true

# API 请求超时时间（毫秒）
VITE_API_TIMEOUT=30000

# Mock 数据开关
VITE_USE_MOCK=true
```

#### 4.2.3 生产环境变量 (.env.production)

```env
# 生产环境 API 地址
VITE_API_BASE_URL=https://api.qa-live-healthcare.com/api

# 关闭调试模式
VITE_DEBUG_MODE=false

# API 请求超时时间（毫秒）
VITE_API_TIMEOUT=10000

# 关闭 Mock 数据
VITE_USE_MOCK=false

# 性能监控
VITE_ENABLE_PERFORMANCE_MONITOR=true
```

### 4.3 环境变量使用

```typescript
// 在代码中使用环境变量
// src/config/index.ts

interface AppConfig {
  title: string;
  version: string;
  apiBaseUrl: string;
  debugMode: boolean;
  enableAnalytics: boolean;
}

export const appConfig: AppConfig = {
  title: import.meta.env.VITE_APP_TITLE || 'QA Live Healthcare',
  version: import.meta.env.VITE_APP_VERSION || '1.0.0',
  apiBaseUrl: import.meta.env.VITE_API_BASE_URL || '/api',
  debugMode: import.meta.env.VITE_DEBUG_MODE === 'true',
  enableAnalytics: import.meta.env.VITE_ENABLE_ANALYTICS === 'true',
};

console.log('当前环境:', import.meta.env.MODE);
console.log('API 地址:', appConfig.apiBaseUrl);
```

### 4.4 构建命令与环境对应

```json
{
  "scripts": {
    "dev": "vite --mode development",
    "build:test": "vue-tsc -b && vite build --mode test",
    "build:staging": "vue-tsc -b && vite build --mode staging",
    "build": "vue-tsc -b && vite build --mode production",
    "preview": "vite preview --mode production"
  }
}
```

---

## 5. Web 服务器配置

### 5.1 Nginx 配置

#### 5.1.1 单项目部署配置

```nginx
# /etc/nginx/conf.d/qa-live-healthcare.conf

# 基础配置
worker_processes auto;
error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
    worker_connections 1024;
    use epoll;
    multi_accept on;
}

http {
    # 基础配置
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    
    # 日志格式
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for" '
                    '$request_time $upstream_response_time';
    
    access_log /var/log/nginx/access.log main;
    
    # 性能优化
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    
    # Gzip 压缩
    gzip on;
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_min_length 1000;
    gzip_types text/plain text/css text/xml application/json 
               application/javascript application/rss+xml 
               application/atom+xml image/svg+xml;
    
    # 上游应用服务器
    upstream qa_live_backend {
        server 127.0.0.1:3000;
        keepalive 32;
    }
    
    # QA Live Healthcare 应用配置
    server {
        listen 80;
        server_name qa-live-healthcare.com www.qa-live-healthcare.com;
        
        # 重定向到 HTTPS（生产环境）
        # return 301 https://$server_name$request_uri;
        
        # 文档根目录
        root /var/www/qa-live-healthcare/dist;
        index index.html;
        
        # 日志
        access_log /var/log/nginx/qa-live-healthcare.access.log;
        error_log /var/log/nginx/qa-live-healthcare.error.log;
        
        # 安全头
        add_header X-Frame-Options "SAMEORIGIN" always;
        add_header X-Content-Type-Options "nosniff" always;
        add_header X-XSS-Protection "1; mode=block" always;
        
        # 静态资源缓存
        location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
            expires 1y;
            add_header Cache-Control "public, immutable";
            access_log off;
        }
        
        # HTML 文件不缓存
        location ~* \.html$ {
            expires -1;
            add_header Cache-Control "no-store, no-cache, must-revalidate";
        }
        
        # API 代理
        location /api/ {
            proxy_pass http://qa_live_backend/;
            proxy_http_version 1.1;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_connect_timeout 60s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
        }
        
        # Vue Router History 模式路由
        location / {
            try_files $uri $uri/ /index.html;
        }
        
        # 健康检查
        location /health {
            access_log off;
            return 200 'OK';
            add_header Content-Type text/plain;
        }
    }
}
```

#### 5.1.2 HTTPS 配置（生产环境）

```nginx
# HTTPS 服务器配置
server {
    listen 443 ssl http2;
    server_name qa-live-healthcare.com www.qa-live-healthcare.com;
    
    # SSL 证书配置
    ssl_certificate /etc/ssl/certs/qa-live-healthcare.crt;
    ssl_certificate_key /etc/ssl/private/qa-live-healthcare.key;
    
    # SSL 安全配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256;
    ssl_prefer_server_ciphers off;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 1d;
    
    # HSTS 配置
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    
    # 其他配置同上...
}

# HTTP 到 HTTPS 重定向
server {
    listen 80;
    server_name qa-live-healthcare.com www.qa-live-healthcare.com;
    return 301 https://$server_name$request_uri;
}
```

### 5.2 Apache 配置

```apache
# .htaccess 文件配置
<IfModule mod_rewrite.c>
    RewriteEngine On
    
    # 强制 HTTPS（生产环境）
    # RewriteCond %{HTTPS} off
    # RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
    
    # 处理 History 模式的路由
    RewriteBase /
    RewriteRule ^index\.html$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.html [L]
</IfModule>

# 浏览器缓存配置
<IfModule mod_expires.c>
    ExpiresActive On
    
    # 静态资源缓存 1 年
    ExpiresByType text/css "access plus 1 year"
    ExpiresByType application/javascript "access plus 1 year"
    ExpiresByType image/png "access plus 1 year"
    ExpiresByType image/jpeg "access plus 1 year"
    ExpiresByType image/svg+xml "access plus 1 year"
    ExpiresByType font/woff2 "access plus 1 year"
    
    # HTML 不缓存
    ExpiresByType text/html "access plus 0 seconds"
</IfModule>

# Gzip 压缩配置
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/plain text/css application/json application/javascript text/xml application/xml text/javascript
</IfModule>
```

---

## 6. Docker 容器化部署

### 6.1 Docker 配置

#### 6.1.1 Dockerfile

```dockerfile
# 构建阶段
FROM node:20-alpine AS builder

# 设置工作目录
WORKDIR /app

# 复制 package 文件
COPY package*.json ./

# 安装依赖
RUN npm ci --only=production

# 复制源码
COPY . .

# 设置环境变量
ENV NODE_ENV=production
ENV VITE_APP_TITLE="QA Live Healthcare"

# 构建应用
RUN npm run build

# 运行阶段
FROM nginx:alpine AS runner

# 复制自定义 nginx 配置
COPY nginx.conf /etc/nginx/conf.d/default.conf

# 从构建阶段复制构建产物
COPY --from=builder /app/dist /usr/share/nginx/html

# 暴露端口
EXPOSE 80

# 启动 nginx
CMD ["nginx", "-g", "daemon off;"]
```

#### 6.1.2 Nginx 配置文件（Docker）

```nginx
# nginx.conf (Docker 版本)
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    # Gzip 压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml;

    # 静态资源缓存
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # API 代理（需要链接后端容器时启用）
    # location /api/ {
    #     proxy_pass http://backend:3000/;
    # }

    # Vue Router History 模式
    location / {
        try_files $uri $uri/ /index.html;
    }

    # 健康检查
    location /health {
        access_log off;
        return 200 'OK';
    }
}
```

#### 6.1.3 Docker Compose 配置

```yaml
# docker-compose.yml
version: '3.8'

services:
  # 前端应用
  frontend:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80:80"
      - "443:443"
    environment:
      - NODE_ENV=production
    depends_on:
      - backend
    networks:
      - app-network
    restart: unless-stopped

  # 后端服务（未来扩展）
  backend:
    image: node:20-alpine
    working_dir: /app
    command: npm run start
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://user:password@db:5432/qa_live
    depends_on:
      - db
    networks:
      - app-network
    restart: unless-stopped

  # 数据库服务（未来扩展）
  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=qa_live
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - app-network
    restart: unless-stopped

networks:
  app-network:
    driver: bridge

volumes:
  postgres_data:
```

### 6.2 Docker 构建和运行

```bash
# 构建 Docker 镜像
docker build -t qa-live-healthcare:latest .

# 运行容器
docker run -d \
  --name qa-live-healthcare \
  -p 80:80 \
  -p 443:443 \
  --restart unless-stopped \
  qa-live-healthcare:latest

# 使用 Docker Compose 运行
docker-compose up -d

# 查看容器状态
docker-compose ps

# 查看容器日志
docker-compose logs -f

# 停止容器
docker-compose down

# 重新构建并运行
docker-compose up -d --build
```

---

## 7. CI/CD 流水线配置

### 7.1 GitHub Actions 配置

```yaml
# .github/workflows/deploy.yml
name: Deploy QA Live Healthcare

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '20'

jobs:
  # 代码检查
  lint:
    name: Lint & Type Check
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint
        run: npm run lint

      - name: Run TypeScript check
        run: npm run type-check

  # 构建和测试
  build-and-test:
    name: Build & Test
    runs-on: ubuntu-latest
    needs: lint
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build application
        run: npm run build
        env:
          VITE_API_BASE_URL: ${{ secrets.VITE_API_BASE_URL }}

      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/

  # 部署到测试环境
  deploy-test:
    name: Deploy to Test
    runs-on: ubuntu-latest
    needs: build-and-test
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Download build artifacts
        uses: actions/download-artifact@v4
        with:
          name: dist
          path: dist/

      - name: Deploy to test server
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.TEST_SERVER_HOST }}
          username: ${{ secrets.TEST_SERVER_USER }}
          key: ${{ secrets.TEST_SERVER_SSH_KEY }}
          source: "dist/*"
          target: "/var/www/qa-live-healthcare-test"
          strip_components: 0

      - name: Run deployment script
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.TEST_SERVER_HOST }}
          username: ${{ secrets.TEST_SERVER_USER }}
          key: ${{ secrets.TEST_SERVER_SSH_KEY }}
          script: |
            cd /var/www/qa-live-healthcare-test
            # 执行部署后脚本
            nginx -s reload

  # 部署到生产环境
  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: deploy-test
    if: github.event_name == 'push' && github.event.commits[0].message contains 'deploy:production'
    
    environment:
      name: production
      url: https://qa-live-healthcare.com
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Download build artifacts
        uses: actions/download-artifact@v4
        with:
          name: dist
          path: dist/

      - name: Deploy to production server
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.PROD_SERVER_HOST }}
          username: ${{ secrets.PROD_SERVER_USER }}
          key: ${{ secrets.PROD_SERVER_SSH_KEY }}
          source: "dist/*"
          target: "/var/www/qa-live-healthcare"
          backup: true

      - name: Run production deployment
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.PROD_SERVER_HOST }}
          username: ${{ secrets.PROD_SERVER_USER }}
          key: ${{ secrets.PROD_SERVER_SSH_KEY }}
          script: |
            cd /var/www/qa-live-healthcare
            # 备份旧版本
            mv dist dist.backup.$(date +%Y%m%d%H%M%S)
            mv dist.new dist
            # 重启服务
            nginx -s reload
            # 清理旧备份（保留最近3个）
            ls -dt dist.backup.* | tail -n +4 | xargs rm -rf
```

### 7.2 GitLab CI 配置

```yaml
# .gitlab-ci.yml
stages:
  - install
  - lint
  - build
  - test
  - deploy

variables:
  NODE_VERSION: "20"

# 安装依赖
install:
  stage: install
  image: node:${NODE_VERSION}-alpine
  script:
    - npm ci
  artifacts:
    paths:
      - node_modules/
    expire_in: 1 hour

# 代码检查
lint:
  stage: lint
  image: node:${NODE_VERSION}-alpine
  script:
    - npm run lint
    - npm run type-check
  needs: ["install"]

# 构建应用
build:
  stage: build
  image: node:${NODE_VERSION}-alpine
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 week
  needs: ["install"]

# 部署到测试环境
deploy_test:
  stage: deploy
  image: alpine:latest
  script:
    - apk add --no-cache openssh-client
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - ssh-keyscan $TEST_SERVER_HOST >> ~/.ssh/known_hosts
    - scp -r dist/* $TEST_SERVER_USER@$TEST_SERVER_HOST:/var/www/qa-live-healthcare-test
    - ssh $TEST_SERVER_USER@$TEST_SERVER_HOST "nginx -s reload"
  environment:
    name: test
    url: https://test.qa-live-healthcare.com
  only:
    - main
  needs: ["build"]

# 部署到生产环境
deploy_production:
  stage: deploy
  image: alpine:latest
  script:
    - apk add --no-cache openssh-client
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - ssh-keyscan $PROD_SERVER_HOST >> ~/.ssh/known_hosts
    - scp -r dist/* $PROD_SERVER_USER@$PROD_SERVER_HOST:/var/www/qa-live-healthcare
    - ssh $PROD_SERVER_USER@$PROD_SERVER_HOST "nginx -s reload"
  environment:
    name: production
    url: https://qa-live-healthcare.com
  when: manual
  needs: ["build"]
```

---

## 8. 云平台部署

### 8.1 阿里云部署

#### 8.1.1 ECS 实例部署

```bash
# 1. 连接服务器
ssh root@your-ecs-ip

# 2. 安装 Nginx
apt update && apt install -y nginx

# 3. 创建应用目录
mkdir -p /var/www/qa-live-healthcare

# 4. 上传构建产物（使用 scp）
scp -r dist/* root@your-ecs-ip:/var/www/qa-live-healthcare/

# 5. 配置 Nginx
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/qa-live-healthcare
# 编辑配置文件...

# 6. 启用站点
ln -s /etc/nginx/sites-available/qa-live-healthcare /etc/nginx/sites-enabled/

# 7. 测试配置
nginx -t

# 8. 重启 Nginx
systemctl restart nginx
```

#### 8.1.2 OSS 对象存储部署

```bash
# 使用阿里云 OSS CLI 部署静态资源

# 1. 安装 ossutil
curl -o ossutil64 http://gosstest.oss-cn-shanghai.aliyuncs.com/ossutil/ossutil64
chmod +x ossutil64

# 2. 配置 ossutil
./ossutil64 config

# 3. 上传构建产物到 OSS
./ossutil64 cp -rf dist oss://your-bucket-name/

# 4. 配置静态网站托管
./ossutil64 oss://your-bucket-name --website-mode=index

# 5. 配置 CDN（可选）
# 在阿里云 CDN 控制台配置自定义域名
```

#### 8.1.3 ACK 容器服务部署

```yaml
# k8s-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: qa-live-healthcare
  namespace: production
spec:
  replicas: 2
  selector:
    matchLabels:
      app: qa-live-healthcare
  template:
    metadata:
      labels:
        app: qa-live-healthcare
    spec:
      containers:
      - name: frontend
        image: registry.cn-shanghai.aliyuncs.com/your-namespace/qa-live-healthcare:latest
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: qa-live-healthcare-service
  namespace: production
spec:
  type: LoadBalancer
  selector:
    app: qa-live-healthcare
  ports:
  - port: 80
    targetPort: 80
```

### 8.2 腾讯云部署

#### 8.2.1 COS 对象存储部署

```bash
# 使用腾讯云 COS CLI 部署

# 1. 安装 coscli
brew install tencentyun/coscli/coscli

# 2. 配置 COS CLI
coscli config

# 3. 上传构建产物
coscli cp -r dist/ cos://your-bucket-1300000000/

# 4. 配置静态网站托管
coscli website put cos://your-bucket-1300000000/ --website

# 5. 配置 CDN（可选）
# 在腾讯云 CDN 控制台配置自定义域名和 HTTPS
```

#### 8.2.2 云开发 CloudBase 部署

```bash
# 使用 CloudBase CLI 部署

# 1. 安装 CloudBase CLI
npm install -g @cloudbase/cli

# 2. 登录
tcb login

# 3. 初始化项目（如首次）
tcb init

# 4. 部署静态网站
tcb hosting deploy dist -e your-env-id

# 5. 查看部署结果
tcb hosting list -e your-env-id
```

---

## 9. 性能优化配置

### 9.1 构建优化

#### 9.1.1 依赖优化

```json
{
  "dependencies": {
    "vue": "^3.5.10",
    "vue-router": "^4.6.3",
    "ant-design-vue": "^4.2.6",
    "dayjs": "^1.11.19"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.1.4",
    "typescript": "^5.5.3",
    "vite": "^5.4.8",
    "vue-tsc": "^2.1.6"
  }
}
```

#### 9.1.2 Tree Shaking 配置

```typescript
// vite.config.ts
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        // 手动分包配置
        manualChunks: {
          // Vue 核心库
          'vue-core': ['vue', 'vue-router'],
          
          // Ant Design Vue
          'ant-ui': ['ant-design-vue'],
          
          // 其他工具库
          'utils': ['dayjs']
        }
      }
    }
  }
});
```

### 9.2 运行时优化

#### 9.2.1 路由懒加载

```typescript
// src/router/index.ts
import { createRouter, createWebHistory } from 'vue-router';

// 路由懒加载配置
const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('../views/Home.vue')
  },
  {
    path: '/consultation',
    name: 'Consultation',
    component: () => import('../views/Consultation.vue')
  },
  {
    path: '/doctors',
    name: 'Doctors',
    component: () => import('../views/Doctors.vue')
  },
  {
    path: '/doctor/login',
    name: 'DoctorLogin',
    component: () => import('../views/DoctorLogin.vue')
  },
  {
    path: '/doctor/room/:username',
    name: 'DoctorRoom',
    component: () => import('../views/DoctorRoom.vue')
  },
  {
    path: '/about',
    name: 'About',
    component: () => import('../views/About.vue')
  }
];

export default createRouter({
  history: createWebHistory(),
  routes
});
```

#### 9.2.2 组件按需加载

```typescript
// src/main.ts
import { createApp } from 'vue';
import App from './App.vue';

// 按需引入 Ant Design Vue
import {
  Button,
  Form,
  Input,
  Select,
  Table,
  Card,
  Layout,
  Menu,
  Modal,
  Message
} from 'ant-design-vue';

// 按需引入样式
import 'ant-design-vue/dist/reset.css';

const app = createApp(App);

// 注册组件
app.use(Button)
   .use(Form)
   .use(Input)
   .use(Select)
   .use(Table)
   .use(Card)
   .use(Layout)
   .use(Menu)
   .use(Modal);

app.mount('#app');
```

### 9.3 CDN 配置优化

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  
  <!-- 预连接 CDN -->
  <link rel="preconnect" href="https://cdn.your-cdn.com">
  
  <!-- DNS 预解析 -->
  <link rel="dns-prefetch" href="https://cdn.your-cdn.com">
  
  <!-- 预加载关键资源 -->
  <link rel="preload" href="/assets/js/vendor.js" as="script">
  
  <!-- 异步加载第三方脚本 -->
  <script src="https://cdn.your-cdn.com/lib/vue@3.5.10/vue.global.prod.js" defer></script>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

---

## 10. 监控与运维

### 10.1 错误监控配置

```typescript
// src/utils/error-handler.ts
interface ErrorLog {
  message: string;
  stack?: string;
  url: string;
  userAgent: string;
  timestamp: string;
}

export const setupErrorHandling = () => {
  // 全局错误处理
  window.addEventListener('error', (event) => {
    const errorLog: ErrorLog = {
      message: event.message,
      stack: event.error?.stack,
      url: window.location.href,
      userAgent: navigator.userAgent,
      timestamp: new Date().toISOString()
    };
    
    // 上报错误日志
    console.error('Global Error:', errorLog);
  });

  // Promise 错误处理
  window.addEventListener('unhandledrejection', (event) => {
    console.error('Unhandled Promise Rejection:', event.reason);
  });

  // Vue 错误处理
  app.config.errorHandler = (err, instance, info) => {
    console.error('Vue Error:', err);
    console.error('Component:', instance);
    console.error('Info:', info);
  };
};
```

### 10.2 性能监控配置

```typescript
// src/utils/performance.ts
export const setupPerformanceMonitoring = () => {
  // 监控页面加载性能
  window.addEventListener('load', () => {
    const timing = performance.timing;
    
    const metrics = {
      DNS查询: timing.domainLookupEnd - timing.domainLookupStart,
      TCP连接: timing.connectEnd - timing.connectStart,
      请求耗时: timing.responseEnd - timing.requestStart,
      DOM解析: timing.domContentLoadedEventEnd - timing.domContentLoadedEventStart,
      页面加载: timing.loadEventEnd - timing.navigationStart
    };
    
    console.log('Performance Metrics:', metrics);
    
    // 上报性能数据
    // reportPerformance(metrics);
  });

  // 监控资源加载
  const observer = new PerformanceObserver((list) => {
    for (const entry of list.getEntries()) {
      if (entry.initiatorType === 'resource') {
        console.log('Resource Loaded:', entry.name, entry.duration);
      }
    }
  });
  
  observer.observe({ entryTypes: ['resource'] });
};
```

### 10.3 日志管理

```typescript
// src/utils/logger.ts
enum LogLevel {
  DEBUG = 0,
  INFO = 1,
  WARN = 2,
  ERROR = 3
}

class Logger {
  private level: LogLevel;
  
  constructor() {
    // 生产环境默认 INFO 级别
    this.level = import.meta.env.PROD ? LogLevel.INFO : LogLevel.DEBUG;
  }
  
  debug(...args: any[]) {
    if (this.level <= LogLevel.DEBUG) {
      console.debug('[DEBUG]', ...args);
    }
  }
  
  info(...args: any[]) {
    if (this.level <= LogLevel.INFO) {
      console.info('[INFO]', ...args);
    }
  }
  
  warn(...args: any[]) {
    if (this.level <= LogLevel.WARN) {
      console.warn('[WARN]', ...args);
    }
  }
  
  error(...args: any[]) {
    if (this.level <= LogLevel.ERROR) {
      console.error('[ERROR]', ...args);
    }
  }
}

export const logger = new Logger();
```

---

## 11. 备份与恢复

### 11.1 构建产物备份

```bash
#!/bin/bash
# backup.sh - 备份脚本

# 配置
APP_NAME="qa-live-healthcare"
BACKUP_DIR="/var/backups/$APP_NAME"
DEPLOY_DIR="/var/www/$APP_NAME"
TIMESTAMP=$(date +%Y%m%d%H%M%S)

# 创建备份目录
mkdir -p $BACKUP_DIR

# 备份当前版本
if [ -d "$DEPLOY_DIR/dist" ]; then
    tar -czf "$BACKUP_DIR/dist-$TIMESTAMP.tar.gz" -C $DEPLOY_DIR dist
    echo "Backup created: $BACKUP_DIR/dist-$TIMESTAMP.tar.gz"
    
    # 清理旧备份（保留最近10个）
    cd $BACKUP_DIR
    ls -t dist-*.tar.gz | tail -n +11 | xargs -r rm
fi

# 上传到远程存储（可选）
# ossutil cp $BACKUP_DIR/dist-$TIMESTAMP.tar.gz oss://your-bucket/backups/
```

### 11.2 回滚脚本

```bash
#!/bin/bash
# rollback.sh - 回滚脚本

# 配置
APP_NAME="qa-live-healthcare"
BACKUP_DIR="/var/backups/$APP_NAME"
DEPLOY_DIR="/var/www/$APP_NAME"

# 获取最新备份
LATEST_BACKUP=$(ls -t $BACKUP_DIR/dist-*.tar.gz | head -n 1)

if [ -z "$LATEST_BACKUP" ]; then
    echo "No backup found!"
    exit 1
fi

echo "Rolling back to: $LATEST_BACKUP"

# 备份当前版本
if [ -d "$DEPLOY_DIR/dist" ]; then
    mv $DEPLOY_DIR/dist $DEPLOY_DIR/dist.rollback-$(date +%Y%m%d%H%M%S)
fi

# 解压备份
tar -xzf $LATEST_BACKUP -C $DEPLOY_DIR

echo "Rollback completed!"

# 重启服务
nginx -s reload
```

---

## 12. 安全配置

### 12.1 安全头配置

```nginx
# Nginx 安全头配置
server {
    # 防止点击劫持
    add_header X-Frame-Options "SAMEORIGIN" always;
    
    # 防止 MIME 类型嗅探
    add_header X-Content-Type-Options "nosniff" always;
    
    # XSS 保护
    add_header X-XSS-Protection "1; mode=block" always;
    
    # 引用来源策略
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    
    # 权限策略
    add_header Permissions-Policy "camera=(), microphone=(), geolocation=()" always;
    
    # HSTS（仅 HTTPS）
    # add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
}
```

### 12.2 CSP 内容安全策略

```html
<!-- index.html -->
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; 
               script-src 'self' 'unsafe-inline' https://cdn.your-cdn.com; 
               style-src 'self' 'unsafe-inline' https://cdn.your-cdn.com; 
               img-src 'self' data: https:; 
               font-src 'self' https://cdn.your-cdn.com;
               connect-src 'self' https://api.your-domain.com;
               frame-ancestors 'none';">
```

---

## 13. 文档总结

### 13.1 部署流程总览

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              部署流程总览                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  1. 环境准备                                                                 │
│     ├── 安装 Node.js 18+                                                     │
│     ├── 安装 Git                                                              │
│     └── 配置包管理器（npm/pnpm）                                             │
│                                    │                                        │
│                                    ▼                                        │
│  2. 项目构建                                                                 │
│     ├── 安装依赖 (npm install / npm ci)                                     │
│     ├── 配置环境变量                                                         │
│     └── 执行构建 (npm run build)                                            │
│                                    │                                        │
│                                    ▼                                        │
│  3. 部署配置                                                                 │
│     ├── 配置 Web 服务器（Nginx/Apache）                                     │
│     ├── 配置域名和 SSL 证书                                                 │
│     └── 配置 CDN 加速（可选）                                               │
│                                    │                                        │
│                                    ▼                                        │
│  4. 服务部署                                                                 │
│     ├── 上传构建产物到服务器                                                 │
│     ├── 配置服务器环境                                                       │
│     └── 启动服务                                                             │
│                                    │                                        │
│                                    ▼                                        │
│  5. 验证部署                                                                 │
│     ├── 健康检查                                                             │
│     ├── 功能测试                                                             │
│     └── 性能验证                                                             │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 13.2 快速部署命令参考

| 环境 | 命令 | 说明 |
|------|------|------|
| 安装依赖 | `npm install` 或 `npm ci` | 安装项目依赖 |
| 开发启动 | `npm run dev` | 启动开发服务器 |
| 生产构建 | `npm run build` | 构建生产版本 |
| 预览构建 | `npm run preview` | 本地预览生产版本 |
| Docker 构建 | `docker build -t app .` | 构建 Docker 镜像 |
| Docker 运行 | `docker-compose up -d` | 启动 Docker 容器 |

### 13.3 相关文档链接

| 文档名称 | 描述 | 关联性 |
|---------|------|---------|
| [系统架构文档](./architecture.md) | 整体架构设计 | 部署架构参考 |
| [项目结构文档](./standard-project-structure.md) | 目录结构规范 | 构建配置参考 |
| [API 文档](./api.md) | 接口定义规范 | 前后端集成参考 |

---

## 附录：常见问题排查

### A.1 构建失败问题

```bash
# 问题：node_modules 版本冲突
# 解决：删除 node_modules 和 package-lock.json，重新安装
rm -rf node_modules package-lock.json
npm install

# 问题：TypeScript 类型错误
# 解决：运行类型检查，修复错误
npm run type-check

# 问题：构建产物过大
# 解决：检查依赖，启用 Tree Shaking
npm run build -- --mode production
```

### A.2 部署常见问题

```bash
# 问题：页面空白
# 原因：路由 History 模式未正确配置
# 解决：检查 Nginx 配置中的 try_files

# 问题：资源加载 404
# 原因：静态资源路径配置错误
# 解决：检查 base 配置和资源路径

# 问题：API 请求失败
# 原因：代理配置错误
# 解决：检查 Nginx proxy_pass 配置
```

---

*文档版本：1.0.0*  
*最后更新：2026-04-21*  
*维护团队：QA Live Healthcare 开发团队*
