# API 接口文档

## 📋 文档概述

本文档定义了 QA Live Healthcare 在线医疗问诊平台的 API 接口规范。当前项目为纯前端应用，采用客户端状态管理。随着业务发展，可按本规范设计与后端服务对接的 API 接口。

---

## 🏗️ API 设计原则

### 基础规范

| 规范项 | 说明 |
|--------|------|
| **协议** | HTTPS |
| **数据格式** | JSON |
| **字符编码** | UTF-8 |
| **认证方式** | JWT Token |
| **版本控制** | URL 路径版本号 |

### 基础 URL

```
生产环境: https://api.qalive.com/v1
开发环境: https://api-dev.qalive.com/v1
本地开发: http://localhost:3000/v1
```

---

## 🔐 认证接口

### 1. 医生登录

**接口地址**: `POST /auth/doctor/login`

**请求参数**:

```json
{
  "username": "dr-zhang-wei",
  "password": "123456"
}
```

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| username | string | 是 | 医生用户名 |
| password | string | 是 | 登录密码 |

**成功响应**:

```json
{
  "code": 200,
  "message": "登录成功",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 86400,
    "doctor": {
      "id": "doc001",
      "username": "dr-zhang-wei",
      "name": "张伟医生",
      "title": "主任医师",
      "department": "心内科",
      "avatar": "https://images.example.com/doctor.jpg"
    }
  }
}
```

**错误响应**:

```json
{
  "code": 401,
  "message": "用户名或密码错误",
  "data": null
}
```

### 2. 患者身份验证

**接口地址**: `POST /auth/patient/verify`

**请求参数**:

```json
{
  "name": "张三",
  "birthday": "1990-05-15"
}
```

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| name | string | 是 | 患者姓名 |
| birthday | string | 是 | 生日 (YYYY-MM-DD) |

**成功响应**:

```json
{
  "code": 200,
  "message": "验证成功",
  "data": {
    "patientId": "patient001",
    "name": "张三",
    "birthday": "1990-05-15",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

## 👨‍⚕️ 医生接口

### 1. 获取医生列表

**接口地址**: `GET /doctors`

**查询参数**:

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| department | string | 否 | 科室筛选 |
| isActive | boolean | 否 | 在线状态筛选 |
| page | number | 否 | 页码 (默认1) |
| pageSize | number | 否 | 每页数量 (默认10) |

**成功响应**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "list": [
      {
        "id": "doc001",
        "username": "dr-zhang-wei",
        "name": "张伟医生",
        "title": "主任医师",
        "department": "心内科",
        "avatar": "https://images.example.com/doctor1.jpg",
        "experience": "15年临床经验",
        "specialties": ["高血压", "冠心病", "心律失常"],
        "isActive": true
      }
    ],
    "pagination": {
      "page": 1,
      "pageSize": 10,
      "total": 5,
      "totalPages": 1
    }
  }
}
```

### 2. 获取医生详情

**接口地址**: `GET /doctors/{doctorId}`

**路径参数**:

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| doctorId | string | 是 | 医生ID |

**成功响应**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "doc001",
    "username": "dr-zhang-wei",
    "name": "张伟医生",
    "title": "主任医师",
    "department": "心内科",
    "avatar": "https://images.example.com/doctor1.jpg",
    "experience": "15年临床经验",
    "specialties": ["高血压", "冠心病", "心律失常"],
    "isActive": true,
    "introduction": "擅长心血管疾病的诊断与治疗...",
    "workingHours": "周一至周五 9:00-18:00"
  }
}
```

### 3. 获取在线医生

**接口地址**: `GET /doctors/online`

**成功响应**:

```json
{
  "code": 200,
  "message": "success",
  "data": [
    {
      "id": "doc001",
      "name": "张伟医生",
      "title": "主任医师",
      "department": "心内科",
      "avatar": "https://images.example.com/doctor1.jpg",
      "specialties": ["高血压", "冠心病"]
    }
  ]
}
```

### 4. 更新医生在线状态

**接口地址**: `PUT /doctors/{doctorId}/status`

**请求头**: `Authorization: Bearer {token}`

**请求参数**:

```json
{
  "isActive": true
}
```

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| isActive | boolean | 是 | 是否在线 |

**成功响应**:

```json
{
  "code": 200,
  "message": "状态更新成功",
  "data": {
    "isActive": true
  }
}
```

---

## 👤 患者接口

### 1. 患者注册

**接口地址**: `POST /patients/register`

**请求参数**:

```json
{
  "name": "张三",
  "birthday": "1990-05-15",
  "phone": "138****1234",
  "gender": "男",
  "password": "******"
}
```

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| name | string | 是 | 患者姓名 |
| birthday | string | 是 | 生日 |
| phone | string | 否 | 手机号 |
| gender | string | 否 | 性别 |
| password | string | 否 | 设置密码(可选) |

**成功响应**:

```json
{
  "code": 200,
  "message": "注册成功",
  "data": {
    "patientId": "patient_new_001",
    "name": "张三",
    "birthday": "1990-05-15"
  }
}
```

### 2. 获取患者信息

**接口地址**: `GET /patients/{patientId}`

**请求头**: `Authorization: Bearer {token}`

**成功响应**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "patient001",
    "name": "赵明",
    "birthday": "1985-03-15",
    "phone": "138****1234",
    "gender": "男",
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
```

### 3. 更新患者信息

**接口地址**: `PUT /patients/{patientId}`

**请求头**: `Authorization: Bearer {token}`

**请求参数**:

```json
{
  "phone": "139****5678",
  "gender": "女"
}
```

**成功响应**:

```json
{
  "code": 200,
  "message": "更新成功",
  "data": null
}
```

---

## 💬 咨询问题接口

### 1. 提交问题

**接口地址**: `POST /questions`

**请求头**: `Authorization: Bearer {token}`

**请求参数**:

```json
{
  "doctorId": "doc001",
  "question": "最近总是感觉胸闷气短,特别是爬楼梯的时候,这是什么原因?"
}
```

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| doctorId | string | 是 | 医生ID |
| question | string | 是 | 问题内容 |

**成功响应**:

```json
{
  "code": 200,
  "message": "问题提交成功",
  "data": {
    "id": "q_new_001",
    "patientId": "patient001",
    "patientName": "赵明",
    "doctorId": "doc001",
    "doctorName": "张伟医生",
    "question": "最近总是感觉胸闷气短...",
    "submitTime": "2025-11-03T09:30:00Z",
    "status": "pending",
    "answer": null,
    "answerTime": null
  }
}
```

### 2. 获取患者的问题列表

**接口地址**: `GET /patients/{patientId}/questions`

**请求头**: `Authorization: Bearer {token}`

**查询参数**:

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| status | string | 否 | 状态筛选 (pending/answered) |
| page | number | 否 | 页码 |
| pageSize | number | 否 | 每页数量 |

**成功响应**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "list": [
      {
        "id": "q001",
        "doctorId": "doc001",
        "doctorName": "张伟医生",
        "question": "最近总是感觉胸闷气短...",
        "submitTime": "2025-11-02T09:30:00Z",
        "status": "answered",
        "answer": "根据您的描述,可能是心脏功能问题...",
        "answerTime": "2025-11-02T09:45:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "pageSize": 10,
      "total": 2,
      "totalPages": 1
    }
  }
}
```

### 3. 获取医生的问题列表

**接口地址**: `GET /doctors/{doctorId}/questions`

**请求头**: `Authorization: Bearer {token}`

**查询参数**:

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| status | string | 否 | 状态筛选 (pending/answered) |

**成功响应**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "pending": [
      {
        "id": "q004",
        "patientId": "patient001",
        "patientName": "赵明",
        "question": "血压最近有点高...",
        "submitTime": "2025-11-02T14:20:00Z",
        "status": "pending"
      }
    ],
    "answered": [
      {
        "id": "q001",
        "patientId": "patient001",
        "patientName": "赵明",
        "question": "最近总是感觉胸闷气短...",
        "submitTime": "2025-11-02T09:30:00Z",
        "status": "answered",
        "answer": "根据您的描述...",
        "answerTime": "2025-11-02T09:45:00Z"
      }
    ],
    "statistics": {
      "pendingCount": 1,
      "answeredCount": 1,
      "totalCount": 2
    }
  }
}
```

### 4. 回复问题

**接口地址**: `POST /questions/{questionId}/answer`

**请求头**: `Authorization: Bearer {token}`

**请求参数**:

```json
{
  "answer": "根据您的描述,可能是心脏功能问题。建议您做个心电图和心脏彩超检查,同时注意休息,避免剧烈运动。"
}
```

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| answer | string | 是 | 回复内容 |

**成功响应**:

```json
{
  "code": 200,
  "message": "回复成功",
  "data": {
    "id": "q001",
    "status": "answered",
    "answer": "根据您的描述,可能是心脏功能问题...",
    "answerTime": "2025-11-02T09:45:00Z"
  }
}
```

### 5. 标记问题为已解答

**接口地址**: `PUT /questions/{questionId}/mark-answered`

**请求头**: `Authorization: Bearer {token}`

**成功响应**:

```json
{
  "code": 200,
  "message": "标记成功",
  "data": {
    "id": "q001",
    "status": "answered",
    "answer": "已口述解答",
    "answerTime": "2025-11-02T09:50:00Z"
  }
}
```

---

## 📊 统计接口

### 1. 获取系统统计

**接口地址**: `GET /statistics`

**成功响应**:

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "totalDoctors": 5,
    "onlineDoctors": 4,
    "totalPatients": 5,
    "totalQuestions": 7,
    "pendingQuestions": 4,
    "answeredQuestions": 3
  }
}
```

---

## 📝 响应状态码

| 状态码 | 说明 |
|--------|------|
| 200 | 请求成功 |
| 201 | 创建成功 |
| 400 | 请求参数错误 |
| 401 | 未授权/认证失败 |
| 403 | 权限不足 |
| 404 | 资源不存在 |
| 500 | 服务器内部错误 |

### 统一错误响应格式

```json
{
  "code": 400,
  "message": "请求参数错误",
  "data": null,
  "errors": {
    "field": "doctorId",
    "message": "医生ID不能为空"
  }
}
```

---

## 🔒 安全规范

### 认证流程

```mermaid
sequenceDiagram
    participant C as 客户端
    participant S as 服务器
    
    C->>S: POST /auth/login (用户名密码)
    S->>S: 验证凭证
    S-->>C: 返回 JWT Token
    C->>C: 存储 Token
    C->>S: 请求 API (Header: Authorization)
    S->>S: 验证 Token
    S-->>C: 返回请求数据
```

### Token 刷新机制

```typescript
// 请求头格式
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

// Token 有效期
const TOKEN_EXPIRES_IN = 86400; // 24小时

// Token 刷新时机
// 在 Token 过期前1小时自动刷新
```

### 数据加密

| 数据类型 | 加密方式 |
|----------|----------|
| 用户密码 | BCrypt 哈希 |
| JWT Secret | AES-256 |
| 敏感字段 | HTTPS 传输 |

---

## 📦 请求示例

### cURL 示例

```bash
# 医生登录
curl -X POST https://api.qalive.com/v1/auth/doctor/login \
  -H "Content-Type: application/json" \
  -d '{"username":"dr-zhang-wei","password":"123456"}'

# 提交问题
curl -X POST https://api.qalive.com/v1/questions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer {token}" \
  -d '{"doctorId":"doc001","question":"最近总是感觉胸闷..."}'
```

### JavaScript (Fetch) 示例

```typescript
// 基础请求封装
const BASE_URL = 'https://api.qalive.com/v1';

async function request(endpoint, options = {}) {
  const token = localStorage.getItem('token');
  
  const defaultHeaders = {
    'Content-Type': 'application/json',
    ...(token && { 'Authorization': `Bearer ${token}` }),
  };
  
  const response = await fetch(`${BASE_URL}${endpoint}`, {
    ...options,
    headers: {
      ...defaultHeaders,
      ...options.headers,
    },
  });
  
  const result = await response.json();
  
  if (result.code !== 200) {
    throw new Error(result.message);
  }
  
  return result.data;
}

// 使用示例
async function submitQuestion(doctorId: string, question: string) {
  return await request('/questions', {
    method: 'POST',
    body: JSON.stringify({ doctorId, question }),
  });
}
```

### JavaScript (Axios) 示例

```typescript
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.qalive.com/v1',
  timeout: 10000,
});

// 请求拦截器
api.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// 响应拦截器
api.interceptors.response.use(
  response => response.data,
  error => {
    if (error.response?.status === 401) {
      // Token 过期，跳转登录
      localStorage.removeItem('token');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

// API 方法
export const doctorAPI = {
  login: (username: string, password: string) =>
    api.post('/auth/doctor/login', { username, password }),
  
  getOnlineDoctors: () =>
    api.get('/doctors/online'),
};

export const questionAPI = {
  submit: (doctorId: string, question: string) =>
    api.post('/questions', { doctorId, question }),
  
  answer: (questionId: string, answer: string) =>
    api.post(`/questions/${questionId}/answer`, { answer }),
};
```

---

## 🏥 科室数据字典

| 科室代码 | 科室名称 |
|----------|----------|
| CARDIO | 心内科 |
| PEDIA | 儿科 |
| ORTHO | 骨科 |
| OBGY | 妇产科 |
| GI | 消化内科 |
| NEURO | 神经内科 |
| DERMA | 皮肤科 |
| OPHTH | 眼科 |

---

## 📋 接口变更记录

| 版本 | 日期 | 变更内容 |
|------|------|----------|
| v1.0.0 | 2025-11-02 | 初始版本 |

---

*最后更新: 2026年4月21日*
*本文档由 Context Builder 工具集自动生成*
