# 代码风格标准文档

## 概述

本文档从实际代码中提炼 QA Live Healthcare 项目的编码规范。所有规则均基于现有源码的一致性分析，新增或修改代码应与存量风格保持一致。

---

## TypeScript 配置

```jsonc
// tsconfig.app.json 关键配置
{
  "compilerOptions": {
    "strict": true,              // 严格模式全开
    "noUnusedLocals": true,      // 禁止未使用局部变量
    "noUnusedParameters": true,  // 禁止未使用参数
    "noFallthroughCasesInSwitch": true,
    "target": "ES2020",
    "module": "ESNext",
    "moduleResolution": "bundler"
  }
}
```

---

## Vue SFC 规范

### 文件结构顺序

所有 `.vue` 文件严格遵循以下三段式顺序（源自项目全部 9 个 SFC 文件的一致模式）：

```vue
<template>
  <!-- 1. 模板 -->
</template>

<script setup lang="ts">
  // 2. 逻辑
</script>

<style scoped>
  /* 3. 样式 */
</style>
```

**唯一例外**: `App.vue` 的全局 CSS 重置使用非 scoped 的 `<style>`。

### script setup 内部代码组织

基于实际代码（`Consultation.vue`, `DoctorRoom.vue`, `Home.vue` 等）总结的导入和声明顺序：

```typescript
<script setup lang="ts">
// === 第 1 层: Vue 核心 ===
import { ref, reactive, computed, onMounted } from 'vue';

// === 第 2 层: Vue Router ===
import { useRoute, useRouter } from 'vue-router';

// === 第 3 层: UI 库 ===
import { message } from 'ant-design-vue';

// === 第 4 层: 第三方库 ===
import dayjs from 'dayjs';

// === 第 5 层: Ant Design 图标 ===
import {
  UserOutlined,
  LogoutOutlined,
  PlusOutlined
} from '@ant-design/icons-vue';

// === 第 6 层: 项目内部模块 ===
import { store, Doctor, Question } from '../store';

// --- 以下为逻辑声明 ---

// Router 实例
const route = useRoute();
const router = useRouter();

// 计算属性（依赖 store 的状态派生）
const currentPatient = computed(() => store.state.currentPatient);
const myQuestions = computed(() => ...);

// ref 响应式变量
const submitModalVisible = ref(false);
const submitting = ref(false);
const selectedQuestion = ref<Question | null>(null);

// reactive 表单对象
const authForm = reactive({
  name: '',
  birthday: null as Dayjs | null,
});

// 生命周期钩子
onMounted(() => { ... });

// 方法/事件处理函数
const verifyPatient = () => { ... };
const submitQuestion = () => { ... };
const formatTime = (time: string) => { ... };
</script>
```

**实际代码证据**（`src/views/Consultation.vue:165-303`）:
```typescript
// 导入顺序：vue → vue-router → ant-design-vue → dayjs → icons-vue → store
import { ref, reactive, computed, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { message } from 'ant-design-vue';
import dayjs, { Dayjs } from 'dayjs';
import { UserOutlined, LogoutOutlined, PlusOutlined } from '@ant-design/icons-vue';
import { store, Doctor } from '../store';
```

---

## 命名约定

基于全部源码的实际使用模式：

| 类别 | 风格 | 实际示例 | 来源 |
|------|------|----------|------|
| Vue 文件 | PascalCase | `AppHeader.vue`, `DoctorRoom.vue` | 全部 .vue 文件 |
| TS 接口 | PascalCase（无 I 前缀） | `Doctor`, `Patient`, `Question`, `State` | `store/index.ts:6-46` |
| 响应式变量 | camelCase | `currentPatient`, `submitModalVisible`, `selectedDoctor` | 各组件 |
| reactive 属性 | camelCase | `authForm.name`, `questionForm.doctorId` | `Consultation.vue:188-204` |
| 计算属性 | camelCase（名词/名词短语） | `myQuestions`, `activeDoctors`, `pendingQuestions` | 各组件 |
| 事件处理 | 动词开头（无统一前缀） | `verifyPatient`, `submitQuestion`, `copyRoomUrl`, `logout` | 各组件 |
| 工具函数 | camelCase | `formatTime`, `navigateTo` | `Consultation.vue:301`, `Home.vue:124` |
| CSS 类名 | kebab-case | `consultation-container`, `doctor-avatar`, `room-card` | 各组件 |
| 路由路径 | kebab-case | `/doctor/login`, `/doctor/room/:username` | `router/index.ts` |
| JSON 数据文件 | kebab-case | `doctor-user-list.json`, `patient-user.json` | `src/data/` |
| store 方法 | camelCase（动词+名词） | `loginDoctor`, `getActiveDoctors`, `addQuestion` | `store/index.ts:59-157` |
| store 导出 | camelCase | `store`, `state` | `store/index.ts:48-57` |

**注意**: 事件处理函数**不使用**统一的 `handle` 前缀，而是直接使用动词（`verifyPatient`, `submitQuestion`, `logout`）。

---

## TypeScript 类型规范

### 接口定义

```typescript
// 实际代码风格：PascalCase，无 I 前缀，无注释分隔
export interface Doctor {
  id: string;
  username: string;
  password: string;
  name: string;
  title: string;
  department: string;
  avatar: string;
  experience: string;
  specialties: string[];
  isActive: boolean;
}

export interface Patient {
  id: string;
  name: string;
  birthday: string;
  phone: string;
  gender: string;
}
```

**特点**:
- 字段无 JSDoc 注释
- 字段间无空行分隔
- 联合类型用字面量联合: `'pending' | 'answered'`
- 可空字段用 `| null`: `answer: string | null`

### 函数签名

```typescript
// 实际风格：方法签名简洁，返回值类型显式标注
loginDoctor(username: string, password: string): Doctor | null { ... }
verifyPatient(name: string, birthday: string): Patient { ... }
getActiveDoctors(): Doctor[] { ... }
getStatistics(): { totalDoctors: number; totalQuestions: number; ... } { ... }

// 新增记录时使用 Omit 工具类型
addQuestion(
  question: Omit<Question, 'id' | 'submitTime' | 'status' | 'answer' | 'answerTime'>
): Question { ... }
```

### 导出模式

```typescript
// 接口导出：具名 export（在 store 中与 state 混合导出）
export interface Doctor { ... }
export interface Patient { ... }
export interface Question { ... }

// Store 导出：具名导出对象
export const store = { state, loginDoctor(), ... };
```

---

## 响应式数据使用模式

### ref vs reactive 判断规则

基于实际代码的使用模式：

| 场景 | 使用 | 实际示例 |
|------|------|----------|
| 布尔开关（模态框、加载态） | `ref` | `const submitModalVisible = ref(false)` |
| 单个可选对象 | `ref<T \| null>` | `const selectedDoctor = ref<Doctor \| null>(null)` |
| 单个字符串/数字 | `ref` | `const answerText = ref('')` |
| 表单对象（多字段） | `reactive` | `const authForm = reactive({ name: '', birthday: null as Dayjs \| null })` |
| 全局应用状态 | `reactive` | `const state = reactive<State>({...})` |

### computed 使用模式

```typescript
// 模式 1: 直接代理 store 方法（纯缓存层）
const statistics = computed(() => store.getStatistics());
const activeDoctors = computed(() => store.getActiveDoctors());

// 模式 2: 带条件过滤
const myQuestions = computed(() =>
  currentPatient.value
    ? store.getQuestionsByPatient(currentPatient.value.id)
    : []
);

// 模式 3: 条件选择
const availableDoctors = computed(() => {
  return selectedDoctor.value
    ? [selectedDoctor.value]
    : store.getActiveDoctors();
});
```

---

## 模板规范

### 条件渲染

```html
<!-- 实际使用的模式：v-if / v-else 二选一 -->
<div v-if="!currentPatient" class="auth-section">...</div>
<div v-else class="patient-portal">...</div>

<!-- 空状态处理 -->
<a-empty v-if="myQuestions.length === 0" description="您还没有提交过问题" />
<div v-else class="my-questions-list">...</div>

<!-- 可选提示 -->
<div class="selected-doctor" v-if="selectedDoctor">...</div>
```

### 列表渲染

```html
<!-- 标准模式 -->
<a-card v-for="question in myQuestions" :key="question.id" class="question-item">
  ...
</a-card>

<!-- 网格卡片 -->
<div v-for="doctor in activeDoctors" :key="doctor.id" class="room-card"
     @click="navigateTo(`/consultation/${doctor.username}`)">
  ...
</div>

<!-- 折叠面板 -->
<a-collapse-panel v-for="question in answeredQuestions" :key="question.id"
                  :header="`${question.patientName}: ${question.question.substring(0, 50)}...`">
  ...
</a-collapse-panel>
```

### Ant Design 组件使用

```html
<!-- 按钮 + 图标 -->
<a-button type="primary" @click="showSubmitModal">
  <PlusOutlined />
  提交问题
</a-button>

<!-- 表单（layout="vertical"） -->
<a-form :model="authForm" :rules="authRules" @finish="verifyPatient" layout="vertical">
  <a-form-item label="姓名" name="name">
    <a-input v-model:value="authForm.name" size="large" placeholder="请输入您的姓名">
      <template #prefix><UserOutlined /></template>
    </a-input>
  </a-form-item>
</a-form>

<!-- 模态框 -->
<a-modal v-model:open="submitModalVisible" title="提交问题"
         @ok="submitQuestion" @cancel="closeSubmitModal"
         :confirmLoading="submitting" width="600px">
  ...
</a-modal>

<!-- 消息提示（script 中） -->
import { message } from 'ant-design-vue';
message.success('问题提交成功');
message.error('请输入问题');
```

**尺寸偏好**: 按钮和输入框统一使用 `size="large"`。
**布局偏好**: 表单统一使用 `layout="vertical"`。

---

## CSS 样式规范

### 基础规则

```css
/* 全部组件使用 <style scoped>，避免样式污染 */
/* App.vue 的全局重置使用非 scoped <style> */
```

### 设计令牌（从代码中提取的实际值）

| 令牌 | 值 | 用途 |
|------|-----|------|
| **主色** | `#1890ff` | 标题、图标、链接 |
| **成功色** | `#52c41a` | 在线状态、特色图标 |
| **渐变主色** | `linear-gradient(135deg, #667eea 0%, #764ba2 100%)` | Hero 区、统计区、卡片头部、门户头部 |
| **页面背景** | `#f0f2f5` | Consultation、DoctorRoom 页面 |
| **卡片背景** | `#fff` | 各类卡片 |
| **文字主色** | `#333` | 标题、正文 |
| **文字次色** | `#666` | 副标题、描述 |
| **文字辅助** | `#999` | 时间、经验、提示文字 |
| **边框色** | `#e8e8e8` | 分隔线、卡片边框 |
| **Header 高度** | `64px` | 固定顶部导航 |
| **内容最大宽** | `1200px` | 居中容器 |
| **圆角** | `12px` / `16px` | 卡片 / 大区块 |
| **阴影** | `0 2px 8px rgba(0,0,0,0.06)` / `0 8px 24px rgba(0,0,0,0.12)` | 普通 / 强调 |
| **响应式断点** | `768px` | 唯一的媒体查询断点 |

### 布局模式

```css
/* 页面容器（固定 Header 补偿） */
.page {
  min-height: calc(100vh - 64px);
  padding-top: 64px;
}

/* 居中内容区 */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 24px;
}

/* 卡片通用样式 */
.card {
  background: #fff;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

/* Flex 居中布局 */
.flex-center {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* Grid 自适应网格 */
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 24px;
}
```

### 响应式设计

项目使用 `max-width: 768px` 作为唯一的移动端断点：

```css
@media (max-width: 768px) {
  .hero {
    flex-direction: column;    /* 纵向排列 */
    padding: 40px 24px;         /* 减小内边距 */
  }

  .hero h1 {
    font-size: 32px;            /* 减小字号 */
  }

  .statistics {
    grid-template-columns: 1fr; /* 单列布局 */
  }

  .rooms-grid {
    grid-template-columns: 1fr; /* 单列布局 */
  }
}
```

**模式**: 桌面端优先（Desktop First），通过 `max-width` 媒体查询适配移动端。

---

## Ant Design Vue 使用规范

### 全局注册

```typescript
// main.ts - 全量注册（非按需引入）
import Antd from 'ant-design-vue';
import 'ant-design-vue/dist/reset.css';
app.use(Antd);
```

### 图标按需导入

```typescript
// 图标不从 ant-design-vue 导入，而是从专用包按需导入
import {
  UserOutlined,      // 用户图标
  LogoutOutlined,    // 登出图标
  PlusOutlined,      // 添加图标
  CheckCircleOutlined, // 完成图标
  TeamOutlined,      // 团队图标
  CopyOutlined,      // 复制图标
  EditOutlined,      // 编辑图标
  CheckOutlined,     // 勾选图标
  ReloadOutlined,    // 刷新图标
  ClockCircleOutlined, // 时钟图标
  FileTextOutlined,  // 文件图标
} from '@ant-design/icons-vue';
```

### 消息提示

```typescript
import { message } from 'ant-design-vue';

// 成功提示
message.success('验证成功,欢迎回来!');
message.success('问题提交成功');
message.success('已标记为已解答');

// 错误提示
message.error('请选择医生');
message.error('请输入问题');
message.error('请先登录');
```

**模式**: 直接调用 `message.success/error()`，不创建实例引用。

---

## 反馈处理模式

### 模拟异步操作

项目所有数据操作均为同步，但使用 `setTimeout` 模拟异步以展示加载状态：

```typescript
const submitQuestion = () => {
  // 1. 前端校验
  if (!questionForm.doctorId) {
    message.error('请选择医生');
    return;
  }

  // 2. 设置加载状态
  submitting.value = true;

  // 3. 模拟异步（500ms）
  setTimeout(() => {
    // 4. 执行业务逻辑
    store.addQuestion({ ... });
    message.success('问题提交成功');

    // 5. 关闭弹窗 + 重置状态
    closeSubmitModal();
    submitting.value = false;
  }, 500);
};
```

### 表单校验

```typescript
// Ant Design 表单校验规则（声明式）
const authRules = {
  name: [{ required: true, message: '请输入姓名' }],
  birthday: [{ required: true, message: '请选择生日' }],
};

// 命令式校验（弹窗内的表单）
if (!questionForm.doctorId) {
  message.error('请选择医生');
  return;
}
if (!questionForm.question.trim()) {
  message.error('请输入问题');
  return;
}
```

---

## 已知代码异味

以下是实际代码中存在但应在新代码中避免的模式：

| 问题 | 位置 | 说明 | 建议改进 |
|------|------|------|----------|
| **重复函数** | `Consultation.vue:301` + `DoctorRoom.vue:177` | `formatTime` 函数两处完全相同 | 提取到 `src/utils/format.ts` |
| **路径别名缺失** | 全部 import 语句 | 使用 `'../store'` 相对路径 | 配置 `@/` 路径别名 |
| **无路由懒加载** | `router/index.ts` | 所有页面静态 import | 改为 `() => import()` 动态导入 |
| **无路由守卫** | `DoctorRoom.vue:155` | 权限检查在 `onMounted` 中 | 使用 `router.beforeEach()` |
| **inline style** | `Consultation.vue:144` | `style="font-size: 12px; color: #999;"` | 移入 scoped CSS |
| **非空断言** | `Consultation.vue:110`, `DoctorRoom.vue:84` | `question.answerTime!` | 添加空值检查 |

---

## 代码审查清单

新增或修改代码时，检查以下项目：

- [ ] Vue SFC 顺序：`<template>` → `<script setup>` → `<style scoped>`
- [ ] import 顺序：vue → vue-router → ant-design-vue → 第三方库 → icons-vue → store
- [ ] TypeScript 严格模式通过（`npm run build` 无类型错误）
- [ ] 无未使用的变量或导入（`noUnusedLocals`, `noUnusedParameters`）
- [ ] CSS 类名使用 kebab-case
- [ ] 设计令牌使用实际值（如渐变色 `#667eea → #764ba2`）
- [ ] 响应式数据：表单用 `reactive`，状态用 `ref`
- [ ] 消息提示使用 `message.success/error()`
- [ ] 模态框有 `confirmLoading` + `setTimeout` 模拟异步
- [ ] 响应式适配使用 `@media (max-width: 768px)`

---

*最后更新: 2026-04-22*
*由 Context Builder 工具集生成*
