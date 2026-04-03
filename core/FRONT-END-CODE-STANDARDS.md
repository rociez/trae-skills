# 前端代码规范


## 1. 项目结构

### 1.1 核心目录结构
```
project/             # Nuxt 3 应用目录
├── components/      # 全局组件
├── composables/     # 可组合函数
├── middleware/      # 中间件
├── plugins/         # 插件
├── stores/          # Pinia 状态管理
├── utils/           # 工具函数
├── pages/           # 页面组件目录
│   └── index.vue    # 首页组件
├── node_modules/    # 依赖包目录
├── .nuxt/           # Nuxt 生成的目录
├── .output/         # 构建输出目录
├── nuxt.config.ts   # Nuxt 配置文件
├── package.json     # 项目配置和依赖
├── pnpm-lock.yaml   # 依赖锁文件
├── tsconfig.json    # TypeScript 配置
├── uno.config.ts    # UnoCSS 配置
└── vuetify.config.ts # Vuetify 配置
```

### 1.2 技术栈

| 类别 | 技术 | 版本 | 用途 |
|------|------|------|------|
| 前端框架 | Nuxt | 3.21.2 | 服务端渲染框架 |
| 前端框架 | Vue | 3.5.30 | 前端视图库 |
| 构建工具 | Vite | 7.3.1 | 模块打包工具 |
| 状态管理 | Pinia | 3.0.4 | 状态管理库 |
| UI组件库 | Vuetify | 3.12.3 | Material Design 风格组件库 |
| UI组件库 | Element Plus | 2.13.6 | 现代 UI 组件库 |
| CSS框架 | UnoCSS | 66.6.7 | 原子化 CSS 框架 |
| 类型系统 | TypeScript | 6.0.2 | 类型检查 |
| 工具库 | Lodash | 4.17.23 | 实用工具函数 |
| 日期处理 | Dayjs | 1.11.20 | 日期时间处理 |
| 图标库 | Remixicon | 4.9.1 | 图标库 |
| 构建工具 | Nitro | 2.13.2 | Nuxt 的服务器引擎 |

## 2. Nuxt3 开发规范

### 2.1 目录命名规范

- **pages 目录**：使用 kebab-case 命名页面文件，对应路由路径
- **components 目录**：使用 PascalCase 命名组件文件
- **composables 目录**：使用 camelCase 命名可组合函数文件
- **stores 目录**：使用 camelCase 命名 store 文件

### 2.2 组件开发规范

#### 2.2.1 组件命名
- **组件名称**：使用 PascalCase 命名法，如 `UserProfile.vue`
- **组件文件**：与组件名称保持一致
- **Props 命名**：使用 camelCase 命名法，如 `userName`
- **事件命名**：使用 kebab-case 命名法，如 `update:userName`

#### 2.2.2 Props 定义
- **类型定义**：使用 TypeScript 类型定义 Props
- **默认值**：为可选 Props 添加默认值
- **验证**：对 Props 进行必要的验证

```vue
<script setup lang="ts">
defineProps<{
  userId: string;
  userName?: string;
  age: number;
}>();
</script>
```

#### 2.2.3 事件处理
- **事件定义**：使用 `defineEmits` 定义组件事件
- **事件命名**：使用 kebab-case 命名法
- **事件传递**：合理传递事件和参数

```vue
<script setup lang="ts">
const emit = defineEmits<{
  'update:userName': [name: string];
  'delete-user': [id: string];
}>();
</script>
```

### 2.3 页面路由与导航规范

#### 2.3.1 页面文件命名
- **页面文件**：使用 kebab-case 命名法，如 `user-profile.vue`
- **嵌套路由**：使用目录结构表示嵌套关系，如 `user/[id].vue`
- **动态路由**：使用 `[param]` 表示动态参数，如 `[id].vue`

#### 2.3.2 导航方式
- **声明式导航**：使用 `<NuxtLink>` 组件进行导航
- **编程式导航**：使用 `useRouter()` 进行编程式导航
- **路由参数**：使用 `useRoute()` 获取路由参数

```vue
<template>
  <NuxtLink to="/user/123">用户详情</NuxtLink>
</template>

<script setup lang="ts">
const router = useRouter();
const route = useRoute();

const goToUser = (id: string) => {
  router.push(`/user/${id}`);
};

const userId = route.params.id as string;
</script>
```

### 2.4 变量命名
- **普通变量**：使用 camelCase 命名法，如 `userName`
- **常量**：使用 UPPER_SNAKE_CASE 命名法，如 `MAX_LENGTH`
- **函数**：使用 camelCase 命名法，如 `getUserInfo`
- **类**：使用 PascalCase 命名法，如 `UserService`
- **接口**：使用 PascalCase 命名法，如 `UserInterface`
- **类型**：使用 PascalCase 命名法，如 `UserType`

### 2.5 状态管理规范（Pinia）

#### 2.5.1 Store 结构
- **Store 文件**：使用 camelCase 命名法，如 `userStore.ts`
- **Store 定义**：使用 `defineStore` 定义 store
- **State 定义**：使用 TypeScript 类型定义 state

```typescript
// stores/userStore.ts
import { defineStore } from 'pinia';

export const useUserStore = defineStore('user', {
  state: () => ({
    userInfo: null as UserInfo | null,
    isLoggedIn: false,
  }),
  getters: {
    getUserInfo: (state) => state.userInfo,
  },
  actions: {
    setUserInfo(info: UserInfo) {
      this.userInfo = info;
      this.isLoggedIn = true;
    },
  },
});

interface UserInfo {
  id: string;
  name: string;
  email: string;
}
```

#### 2.5.2 Store 使用
- **导入 Store**：使用 `import { useUserStore } from '@/stores/userStore';`
- **使用 Store**：在组件中通过 `const userStore = useUserStore();` 调用
- **状态更新**：通过 store 的 actions 更新状态，避免直接修改 state




### 2.6 API 请求处理规范

#### 2.6.1 API 目录结构
- **API 文件**：在 `server/api` 目录下创建 API 路由
- **API 命名**：使用 kebab-case 命名 API 文件，如 `user-profile.ts`

#### 2.6.2 客户端请求
- **使用 $fetch**：使用 Nuxt 内置的 `$fetch` 方法进行请求
- **请求封装**：创建统一的 API 封装函数
- **错误处理**：统一处理 API 请求错误

```typescript
// composables/useApi.ts
export function useApi() {
  const fetchApi = async <T>(url: string, options?: RequestInit): Promise<T> => {
    try {
      return await $fetch<T>(url, {
        ...options,
        headers: {
          'Content-Type': 'application/json',
          ...options?.headers,
        },
      });
    } catch (error) {
      console.error('API 请求错误:', error);
      throw error;
    }
  };

  return {
    fetchApi,
  };
}
```

## 3. 样式开发规范

### 3.1 Scoped CSS 使用
- **Scoped 样式**：使用 `scoped` 属性隔离组件样式
- **深度选择器**：使用 `:deep()` 进行深度选择
- **全局样式**：在 `app.vue` 中定义全局样式

```vue
<style scoped>
.container {
  padding: 20px;
}

:deep(.v-btn) {
  margin-right: 10px;
}
</style>
```

### 3.2 UnoCSS 使用
- **原子化类**：优先使用 UnoCSS 的原子化类
- **自定义工具类**：在 `uno.config.ts` 中定义自定义工具类
- **响应式设计**：使用 UnoCSS 的响应式前缀

## 4. TypeScript 类型定义规范

### 4.1 类型定义文件
- **类型文件**：在 `types` 目录下创建类型定义文件
- **类型命名**：使用 PascalCase 命名类型
- **接口定义**：使用 `interface` 定义对象类型
- **类型导出**：使用 `export` 导出类型

```typescript
// types/index.ts
export interface UserInfo {
  id: string;
  name: string;
  email: string;
}

export type ApiResponse<T> = {
  code: number;
  message: string;
  data: T;
};
```

### 4.2 类型使用
- **类型导入**：使用 `import type { UserInfo } from '@/types';`
- **类型注解**：为变量、函数参数和返回值添加类型注解
- **泛型使用**：合理使用泛型提高代码复用性

## 5. 错误处理与日志规范

### 5.1 错误处理
- **全局错误处理**：在 `app/plugins` 中配置全局错误处理
- **页面错误处理**：使用 `error.vue` 页面处理 404 和其他错误
- **API 错误处理**：统一处理 API 请求错误

### 5.2 日志规范
- **开发环境**：使用 `console.log` 进行调试
- **生产环境**：使用结构化日志，避免控制台输出敏感信息
- **错误日志**：记录关键错误信息，便于排查问题

## 6. 性能优化指南

### 6.1 代码分割
- **页面懒加载**：Nuxt 自动对页面进行代码分割
- **组件懒加载**：使用 `defineAsyncComponent` 懒加载组件
- **动态导入**：使用 `import()` 动态导入模块

### 6.2 资源优化
- **图片优化**：使用适当的图片格式和尺寸
- **静态资源**：将静态资源放在 `public` 目录
- **CDN 配置**：合理配置 CDN 加速静态资源

### 6.3 渲染优化
- **服务端渲染**：充分利用 Nuxt 的服务端渲染能力
- **缓存策略**：合理配置缓存策略，减少重复渲染
- **预渲染**：对静态页面使用预渲染

## 7. 测试规范

### 7.1 测试文件结构
- **测试文件**：在 `tests` 目录下创建测试文件
- **测试命名**：使用 `.test.ts` 或 `.spec.ts` 后缀
- **测试组织**：按功能模块组织测试文件

### 7.2 测试类型
- **单元测试**：测试单个组件或函数
- **集成测试**：测试组件之间的交互
- **E2E 测试**：测试完整的用户流程

### 7.3 测试框架
- **单元测试**：使用 Vitest
- **E2E 测试**：使用 Cypress 或 Playwright
- **测试覆盖率**：目标覆盖率不低于 80%

## 8. Git 提交规范

### 8.1 提交信息格式
```
<type>(<scope>): <subject>

<body>

<footer>
```

### 8.2 提交类型
- **feat**：新功能
- **fix**：修复 bug
- **docs**：文档更新
- **style**：代码风格调整
- **refactor**：代码重构
- **test**：测试相关
- **chore**：构建、依赖等其他修改

### 8.3 分支管理
- **main**：主分支，用于发布生产版本
- **develop**：开发分支，用于集成功能
- **feature**：功能分支，用于开发新功能
- **bugfix**：修复分支，用于修复 bug

## 9. 测试规范

### 9.1 测试类型
- **单元测试**：测试单个组件或函数
- **集成测试**：测试组件之间的交互
- **E2E 测试**：测试完整的用户流程

### 9.2 测试框架
- **单元测试**：使用 Vitest 或 Jest
- **E2E 测试**：使用 Cypress 或 Playwright

### 9.3 测试覆盖率
- **代码覆盖率**：目标覆盖率不低于 80%
- **分支覆盖率**：目标覆盖率不低于 70%

### 9.4 测试编写规范
- **测试命名**：使用描述性的测试名称
- **测试用例**：每个测试用例只测试一个功能点
- **测试数据**：使用合理的测试数据
- **测试环境**：确保测试环境的一致性

## 10. 可执行性和可落地性

### 10.1 工具支持
- **ESLint**：使用 ESLint 进行代码风格检查
- **Prettier**：使用 Prettier 进行代码格式化
- **Husky**：使用 Husky 进行 Git 提交前的检查
- **Commitizen**：使用 Commitizen 规范 Git 提交信息

### 10.2 开发流程
- **代码审查**：进行代码审查，确保代码质量
- **持续集成**：使用 CI/CD 进行持续集成和部署
- **自动化测试**：在 CI/CD 中运行自动化测试

### 10.3 团队协作
- **代码规范培训**：对团队成员进行代码规范培训
- **规范执行监督**：定期检查代码规范的执行情况
- **规范更新**：根据项目需求和技术发展，及时更新代码规范

### 10.4 文档管理
- **规范文档**：维护详细的代码规范文档
- **API 文档**：使用 JSDoc 生成 API 文档
- **使用说明**：编写组件和工具的使用说明



## 12. 总结

本代码规范基于项目的技术栈和需求制定，旨在提高代码质量、团队协作效率和项目可维护性。规范涵盖了命名、代码风格、组件设计、错误处理、性能优化、Git 提交和测试等各个方面，具有可执行性和可落地性。

团队成员应该严格遵守本规范，确保代码的一致性和质量。同时，规范也应该根据项目的发展和技术的进步进行适时的更新和调整。