# 项目结构

## 1. 目录树 (Directory Tree)
```
/
├── components/      # 全局组件
├── composables/     # 可组合函数
├── middleware/      # 中间件
├── plugins/         # 插件
├── stores/          # Pinia 状态管理
├── utils/           # 工具函数
├── pages/           # 页面组件目录
│   └── index.vue    # 首页组件
│   ├── [模块A]/
│   └── [模块B]/
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


## 2. 关键文件说明 (Key Files)
*   **package.json / nuxt.config.ts**: 项目依赖管理文件。
*   .env.development 和 .env.production : 环境变量示例（禁止上传敏感信息）。
*   **README.md**: 项目入口文档，必须包含“如何启动”说明。

## 3. 模块说明 (Module Description)
*   **pages/**: 业务逻辑的核心。
    *   `[模块A]`: [描述]
    *   `[模块B]`: [描述]

