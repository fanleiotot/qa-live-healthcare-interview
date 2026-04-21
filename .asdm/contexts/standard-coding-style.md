# 代码风格标准文档

## 📋 概述

本文档定义了 QA Live Healthcare 项目的代码风格规范，旨在确保代码一致性、可读性和可维护性。项目基于 Vue 3 + TypeScript + Vite 技术栈，遵循现代化的前端开发最佳实践。

## 🎯 技术规范基础

### TypeScript 配置

项目使用严格的 TypeScript 配置（`tsconfig.app.json`）：

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  }
}
```

**核心规则**：
- 启用严格模式（`strict: true`）
- 禁止未使用的局部变量
- 禁止未使用的函数参数
- 禁止 switch 语句中的 fallthrough

## 📝 Vue 组件规范

### 组件结构模板

项目采用 Vue 3 的 `<script setup>` 语法糖，组件文件应遵循以下结构顺序：

```vue
<template>
  <!-- 1. 模板内容 -->
</template>

<script setup lang="ts">
// 2. 导入部分（按顺序）
// 2.1 Vue 核心模块
// 2.2 Vue Router
// 2.3 Pinia/Store
// 2.4 第三方库
// 2.5 项目内部模块
// 2.6 类型定义

// 3. Props 和 Emits 定义

// 4. 响应式数据（ref/reactive）

// 5. 计算属性（computed）

// 6. 观察者（watch/watchEffect）

// 7. 方法（普通函数）

// 8. 生命周期钩子
</script>

<style scoped>
/* 样式内容 */
</style>
```

### 导入顺序规范

```typescript
// 1. Vue 核心
import { ref, reactive, computed, watch } from 'vue';
import { onMounted, onUnmounted } from 'vue';

// 2. Vue Router
import { useRouter, useRoute } from 'vue-router';

// 3. 状态管理
import { store } from '@/store';

// 4. UI 库组件
import { message } from 'ant-design-vue';

// 5. UI 库图标
import { UserOutlined, LockOutlined } from '@ant-design/icons-vue';

// 6. 项目内部组件
import AppHeader from '@/components/AppHeader.vue';

// 7. 项目内部工具
import { formatDate, validatePhone } from '@/utils';

// 8. 类型定义
import type { Doctor, Patient } from '@/store';
```

### 组件命名规范

| 类型 | 命名规范 | 示例 | 说明 |
|------|---------|------|------|
| 组件文件 | PascalCase | AppHeader.vue | 首字母大写 |
| 组件名 | PascalCase | AppHeader | 与文件名一致 |
| 事件处理 | handle + 动作 | handleClick, handleSubmit | handle 前缀 |
| 业务方法 | 动词 + 名词 | submitForm, fetchData | 清晰表达意图 |
| 计算属性 | 名词或形容词短语 | fullName, isActive | 描述返回值 |

### Props 定义规范

**推荐使用 TypeScript 类型定义**：

```typescript
// ✅ 推荐：使用 TypeScript 类型
interface Props {
  title: string;
  count?: number;
  items: string[];
}

// 使用时
withDefaults(defineProps<Props>(), {
  count: 0,
});
```

```typescript
// ❌ 避免：使用普通对象
defineProps({
  title: String,
  count: Number,
});
```

### Emits 定义规范

```typescript
// ✅ 推荐：使用类型化 emit
const emit = defineEmits<{
  (e: 'update', value: string): void;
  (e: 'delete', id: number): void;
}>();

// 使用时
emit('update', newValue);
```

## 🔧 TypeScript 编码规范

### 类型定义

**接口命名**：使用 PascalCase，使用描述性名称

```typescript
// ✅ 推荐
interface Doctor {
  id: string;
  name: string;
  department: string;
}

// ❌ 避免
interface IDoc {
  id: string;
  name: string;
}
```

**类型别名**：用于联合类型或工具类型

```typescript
// ✅ 推荐
type Status = 'pending' | 'answered';
type Callback = (data: Response) => void;

// ❌ 避免
type TCallback = (data: Response) => void;
```

### 函数定义

**箭头函数优先**：用于短函数和回调

```typescript
// ✅ 推荐
const handleClick = () => {
  console.log('clicked');
};

const fetchData = async (id: string): Promise<Data> => {
  const response = await api.get(id);
  return response.data;
};

// ❌ 避免
function handleClick() {
  console.log('clicked');
}
```

**明确返回类型**：复杂函数应标注返回类型

```typescript
// ✅ 推荐
function getStatistics(): Statistics {
  return {
    total: state.items.length,
    active: state.items.filter(i => i.isActive).length,
  };
}

// ✅ 推荐：短函数可省略
const double = (n: number) => n * 2;
```

### 响应式数据

**ref vs reactive**：

```typescript
// ✅ 推荐：简单类型使用 ref
const count = ref(0);
const name = ref<string>('');
const loading = ref(false);

// ✅ 推荐：复杂对象使用 reactive
const formState = reactive({
  username: '',
  password: '',
});

// ❌ 避免：简单类型使用 reactive
const count = reactive({ value: 0 });
```

**计算属性**：始终使用 computed

```typescript
// ✅ 推荐
const doubled = computed(() => count.value * 2);
const fullName = computed(() => `${firstName.value} ${lastName.value}`);

// ✅ 推荐：基于其他响应式数据
const activeItems = computed(() => 
  items.value.filter(item => item.isActive)
);
```

## 🎨 CSS 样式规范

### 样式组织

**使用 scoped 样式**：避免样式污染

```vue
<style scoped>
.header {
  /* 样式内容 */
}
</style>
```

### 选择器命名

**采用 kebab-case**：

```css
/* ✅ 推荐 */
.app-header { }
.content-wrapper { }
.user-avatar { }

/* ❌ 避免：camelCase */
.appHeader { }
.contentWrapper { }
```

### 样式顺序

```css
/* 建议的样式顺序 */
.selector {
  /* 1. 定位 */
  position: fixed;
  top: 0;
  left: 0;
  
  /* 2. 盒模型 */
  width: 100%;
  height: 64px;
  padding: 0 24px;
  margin: 0;
  
  /* 3. 背景和边框 */
  background: #fff;
  border: 1px solid #e8e8e8;
  
  /* 4. 字体 */
  font-size: 16px;
  font-weight: 600;
  color: #333;
  
  /* 5. 其他 */
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}
```

### 响应式设计

**使用媒体查询**：

```css
/* ✅ 推荐：移动优先 */
.card {
  width: 100%;
  padding: 16px;
}

@media (min-width: 768px) {
  .card {
    width: 50%;
    padding: 24px;
  }
}

@media (min-width: 1200px) {
  .card {
    width: 33.333%;
  }
}

/* ❌ 避免：内联媒体查询 */
.card {
  @media (max-width: 768px) {
    width: 100%;
  }
}
```

### 颜色使用

**使用设计系统颜色变量**：

```css
/* ✅ 推荐：使用 Ant Design 颜色 */
.text-primary {
  color: #1890ff;
}

.text-success {
  color: #52c41a;
}

/* ✅ 推荐：使用 CSS 变量 */
:root {
  --primary-color: #1890ff;
  --success-color: #52c41a;
}

.button {
  background: var(--primary-color);
}
```

## 📐 布局规范

### 固定布局模式

项目使用固定顶部导航栏（高度 64px）：

```css
/* 页面容器 */
.page {
  min-height: calc(100vh - 64px);  /* 减去 Header 高度 */
  padding-top: 64px;               /* 为 Header 留出空间 */
}
```

### 内容区域

**最大宽度限制**：内容区域使用 max-width 居中

```css
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 24px;
}
```

### 栅格系统

**使用 CSS Grid 或 Flexbox**：

```css
/* Grid 布局 */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 24px;
}

/* Flex 布局 */
.flex-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
}
```

## 🧩 最佳实践

### 1. 组件拆分原则

**单一职责**：每个组件应专注于一个功能

```vue
<!-- ✅ 推荐：单一职责组件 -->
<template>
  <div class="doctor-card">
    <img :src="doctor.avatar" />
    <h3>{{ doctor.name }}</h3>
    <p>{{ doctor.title }}</p>
  </div>
</template>

<!-- ❌ 避免：职责过多的组件 -->
<!-- 一个组件同时包含列表、详情、弹窗等多种功能 -->
```

### 2. 避免 Props  drilling

```typescript
// ✅ 推荐：使用 provide/inject 或 Store
// 在父组件中
provide('theme', 'dark');

// 在子组件中
const theme = inject('theme');

// ❌ 避免：多层传递 props
<Parent :data="data">
  <Child :data="data">
    <GrandChild :data="data" />
  </Child>
</Parent>
```

### 3. 状态管理规范

```typescript
// ✅ 推荐：集中的状态管理
// src/store/index.ts
export const store = {
  state: reactive({}),
  
  // 方法命名：动词 + 名词
  loginDoctor(username: string, password: string) { },
  logoutDoctor() { },
  getActiveDoctors(): Doctor[] { },
};

// ❌ 避免：组件内部随意管理共享状态
```

### 4. 异步操作处理

```typescript
// ✅ 推荐：处理加载状态和错误
const loading = ref(false);
const error = ref<string | null>(null);

const fetchData = async () => {
  loading.value = true;
  error.value = null;
  
  try {
    const data = await api.getData();
    // 处理数据
  } catch (e) {
    error.value = '获取数据失败';
  } finally {
    loading.value = false;
  }
};

// ❌ 避免：无 loading 状态
const fetchData = async () => {
  const data = await api.getData();
};
```

### 5. 常量定义

```typescript
// ✅ 推荐：集中定义常量
// src/constants/index.ts
export const ROUTES = {
  HOME: '/',
  CONSULTATION: '/consultation',
  DOCTORS: '/doctors',
} as const;

export const API_URL = {
  BASE: 'https://api.example.com',
  DOCTORS: '/doctors',
} as const;

// ❌ 避免：硬编码常量
router.push('/consultation');
```

## 🚫 代码禁区

### 避免的操作

| 类别 | 禁止做法 | 替代方案 |
|------|---------|---------|
| 类型安全 | 使用 `any` 类型 | 使用具体类型或 `unknown` |
| 响应式 | 直接修改数组索引 | 使用响应式方法 |
| 样式 | 内联样式过多 | 使用 CSS 类 |
| 副作用 | 副作用不清理 | 在 onUnmounted 清理 |
| 性能 | 在模板中调用函数 | 使用计算属性 |
| 安全 | 用户输入不验证 | 使用验证规则 |

### any 类型禁用

```typescript
// ❌ 严格禁止
function handleData(data: any) { }

// ✅ 替代方案：使用 unknown
function handleData(data: unknown) {
  if (isValidData(data)) {
    // 处理数据
  }
}

// ✅ 或使用具体类型
function handleData(data: UserData) { }
```

## 📊 代码审查清单

提交代码前请确认：

- [ ] TypeScript 编译无错误
- [ ] 无未使用的变量或导入
- [ ] Props 和 Emits 已正确类型定义
- [ ] 样式使用 scoped 或 CSS 类
- [ ] 响应式数据使用 ref/reactive
- [ ] 复杂逻辑已添加注释
- [ ] 遵循组件命名规范
- [ ] 移动端适配已完成

## 🔧 IDE 配置建议

### VS Code 扩展

推荐安装以下扩展：

- **Volar** - Vue 3 语法高亮和提示
- **ESLint** - 代码规范检查
- **Prettier** - 代码格式化
- **TypeScript Vue Plugin** - TS 支持

### 自动格式化配置

在项目根目录创建 `.prettierrc`：

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 100,
  "bracketSpacing": true,
  "arrowParens": "always"
}
```

---

*最后更新: 2026年4月21日*
*本文档由 Context Builder 工具集自动生成*
