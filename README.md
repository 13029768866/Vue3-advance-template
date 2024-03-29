# Vue3 Advance Template

## 一、特性

- ⚡️ [Vite](https://cn.vitejs.dev/) - 构建工具
- 🖖 [Vue 3](https://cn.vuejs.org/) - 渐进式框架
- 🚦 [Vue Router](https://router.vuejs.org/zh) - 路由管理器
- 📦 [Pinia](https://pinia.vuejs.org/zh) - 状态管理系统（轻量级）
- 🔗 [Axios](https://axios-http.com/zh/) - 一个基于 promise 的网络请求库，可以用于浏览器和 node.js
- 🧰 [Husky](https://typicode.github.io/husky/#/) + [Lint-Staged](https://github.com/okonet/lint-staged) - Git Hook 工具
- 🛡️ [EditorConfig](http://editorconfig.org/) + [ESLint](http://eslint.cn/) + [Prettier](https://prettier.cn/) + [Stylelint](https://stylelint.cn/) - 代码规范
- 🔨 [Commitizen](https://cz-git.qbb.sh/zh) + [Commitlint](https://commitlint.js.org/) - 提交规范

## 二、PNPM (包管理工具）

- [pnpm](https://pnpm.io/)

- 快：`pnpm`是同类工具速度的将近 2 倍
- 高效： `node_modules`中所有文件均链接自单一存储位置
- 支持单体仓库： `monorepo`，单个源码仓库中包含多个软件包支持
- 权限严格： 创建的`node_modules`默认并非扁平结构，因此代码无法对任意软件包进行访问

## 三、Vite (构建工具)

- [Vite](https://cn.vitejs.dev/)

- 极速的服务启动：使用原生 ESM 文件，无需打包操作！（Webpack 整个项目代码打包在一起，然后才能启动服务）
- `HMR`(热重载)轻量快速：无论应用大小，都可以极快的模块热更新
- 丰富的功能：对`TypeScript`、`JSX`、`CSS`等支持开箱即用
- 优化的构建：可选择 **多页应用** 和 **库** 模式的预配置（本质 Rollup 构建）
- 通用的插件：开发和构建之间共享 `Rollup-superset` 插件接口
- 完全类型化的 API：灵活的 API 和完整 TypeScript

## 四、项目目录

## 五、TS 配置

### 1、安装依赖

- `typescript`
- `@vue/tsconfig`
- `vue-tsc`

```
pnpm i typescript @vue/tsconfig vue-tsc -D
```

### 2、配置 tsconfig.json

```json
{
  "compilerOptions": {
    "paths": {
      // 别名路径
      "@/*": ["./src/*"]
    },
    "target": "ESNext", // 转化目标语法
    "module": "ESNext", // 转化格式
    "moduleResolution": "Node", // 解析规则
    "strict": true, // 严格模式
    "jsx": "preserve", // 不允许ts变异jsx语法
    "resolveJsonModule": true, // 允许导入json文件
    "esModuleInterop": true, // 可以通过import的方式导入CommonJS模块
    "lib": ["ESNext", "DOM"], // 编译过程中需要引入的库文件
    "skipLibCheck": true, // 忽略声明文件（xxx.d.ts）类型检查
    "useDefineForClassFields": true, // class不需要初始化字段
    "noEmit": true // 开启不生成输出文件,只进行代码校验
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "exclude": ["node_modules", "tests/server/**/*.ts", "dist", "**/*.js"],
  "references": [{ "path": "./tsconfig.node.json" }] // 用于指定项目共享代码的引用关系
}
```

### 3、配置 tsconfig.node.josn

```json
{
  "extends": "@vue/tsconfig/tsconfig.node.json",
  "include": ["vite.config.*", "vitest.config.*", "cypress.config.*", "playwright.config.*"],
  "compilerOptions": {
    "composite": true,
    "types": ["node"]
  }
}
```

### 4、依赖包声明文件

- `@types/node`

## 六、`EsLint` 配置

### 1、安装依赖

- `eslint`
- `eslint-plugin-vue`
- `vue-eslint-parser`
- `@typescript-eslint/eslint-plugin`
- `@typescript-eslint/parser`

```bash
pnpm i eslint eslint-plugin-vue vue-eslint-parser @typescript-eslint/eslint-plugin @typescript-eslint/parser -D
```

### 2、配置继承规则和自定义规则

详情请看`.eslintrc.js`

### 3、配置.eslintignore

```json
node_modules
*.sh
*.md
*.woff
*.ttf
*.css
*.jpg
*.jpeg
*.png
*.gif
*.d.ts
.vscode
.idea
dist
/public
/docs
.husky
.local
/bin
Dockerfile
```

### 4、添加脚本

```json
"lint:eslint": "eslint --cache --max-warnings 0  \"{src,mock}/**/*.{vue,ts,tsx}\" --fix",
```

- `--cache`: 启用缓存，用于加速下一次 lint。
- `--max-warnings 0`: 将警告的数量限制为 0，即不允许有任何警告，只有错误才会被报告。

## 七、`Prettier ` 配置

### 1、安装依赖

- `prettier`
- `eslint-config-prettier`: 关闭 ESLint 中与 Prettier 中会发生冲突的规则
- `eslint-plugin-prettier`: 将 Prettier 的规则设置到 ESLint 的规则中

```shell
pnpm i prettier eslint-config-prettier eslint-plugin-prettier -D
```

### 2、配置 prettier.config.js

```js
module.exports = {
  printWidth: 100, // 每行100个字符
  tabWidth: 2, // tab间隔
  useTabs: false, // 是否使用tab缩进而不是空格
  semi: true, // 语句结尾是否打印分号
  singleQuote: true, // 是否使用单引号
  quoteProps: 'as-needed', // 对象属性被引用修改时 as-needed(需要时候添加)，consistent(一个引用全部添加)，preserve(对象属性中使用)
  jsxSingleQuote: false, // jsx使用单引号
  trailingComma: 'es5', // 多行末尾逗号，es5(对象，数组es5中有效结尾逗号)，none（没有）,all(尽可能使用,现代浏览器需要babel转译支持node8，ES7)
  bracketSpacing: true, // 对象空格{ foo: bar }
  jsxBracketSameLine: false, // 开始标签结尾‘>’是否换行，false换行
  arrowParens: 'always', // 箭头函数参数是否带括号; always: (x) => x , avoid: x => x
  rangeStart: 0, // 文件格式化起始部分
  requirePragma: false, // 是否格式化文件顶部多行注释
  insertPragma: false, // 配合requirePragma给多行注释添加换行符
  proseWrap: 'never', // 换行规则; always(超过printWidth换行)，never（不换行，可以使用编译器换行规则），preserve（按照原样返回）
  htmlWhitespaceSensitivity: 'strict', // html内空格敏感度; strict(敏感)，ignore（忽视）, css(css现实属性默认值)
  vueIndentScriptAndStyle: true, // 是否缩进vue文件中的,<script>和<style>标签
  endOfLine: 'auto', // 结束行，linux默认‘lf’,配合.editorconfig解决windows结束行“crlf”报错问题
};
```

### 3、配置.prettierignore

```json
/dist/*
.local
.output.js
/node_modules/**

**/*.svg
**/*.sh

/public/*
```

### 4、添加脚本

```json
"lint:prettier": "prettier --write  \"src/**/*.{js,json,tsx,css,less,scss,vue,html,md}\"",
```

## 八、`Stylelint` 配置

### 1、安装依赖

- `postcss`: css 工具
- `postcss-less`/`postcss-sass`: 识别 less/sass 语法
- `postcss-html`: 识别 html/vue 中的`<style></style>`标签中的样式
- `stylelint `: css 格式 lint 工具`:
- `stylelint-config-standard`: `Stylelint`的标准可共享配置规则
- `stylelint-config-prettier`: 关闭所有不必要或可能与`Prettier`冲突的规则
- `stylelint-config-recommended`: `Stylelint`官方提供的一个推荐配置
- `stylelint-config-recommended-vue`: lint`.vue`文件的样式配置
- `stylelint-order`: 指定样式书写的顺序，在`.stylelintrc.js`中`order/properties-order`指定顺序

```json
pnpm i stylelint postcss postcss-html stylelint-config-standard stylelint-config-prettier stylelint-config-recommended stylelint-config-recommended-vue stylelint-order -D
```

### 2、配置`.stylelintrc.js`

详情请看`.stylelintrc.js`

### 3、添加脚本

```json
 "lint:stylelint": "stylelint --cache --fix \"**/*.{vue,less,postcss,css,scss}\" --cache --cache-location node_modules/.cache/stylelint/",
```

## 九、`Git` 提交信息校验

### 1、安装依赖

- `husky`
- `lint-staged`
- `@commitlint/cli`
- `@commitlint/config-conventional`
- `cz-git`

```bash
pnpm i husky lint-staged @commitlint/cli @commitlint/config-conventional cz-git -D
```

### 2、husky 钩子配置

详情查看`.husky`下面`pre-commit`、`commit-msg`

### 3、配置`lint-staged`

```json
"lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "{!(package)*.json,*.code-snippets,.!(browserslist)*rc}": [
      "prettier --write--parser json"
    ],
    "package.json": [
      "prettier --write"
    ],
    "*.vue": [
      "eslint --fix",
      "prettier --write",
      "stylelint --fix"
    ],
    "*.{scss,less,styl,html}": [
      "stylelint --fix",
      "prettier --write"
    ],
    "*.md": [
      "prettier --write"
    ]
  }
```

### 4、配置`commitlint.config.js`

详情见`commitlint.config.js`

### 5、配置脚本指令

```json
"config": {
    "commitizen": {
      "path": "node_modules/cz-git"
    }
  }
```

## 十、快乐小插件

### 1、`unplugin-auto-import` 自动导入 api

1. 配置

   ```js
     AutoImport({
         imports: ['vue', 'vue-router'],
         // eslintrc: { enabled: true },
       }),
   ```

2. 解决 eslint 报错

   通过 `eslintrc: { enabled: true }` 配置生成 `.eslintrc-auto-import.json`,在 `eslintrc` 中继承

3. 解决 ts 报错

`tsconfig` 继承 `auto-imports.d.ts`

### 2、`unplugin-vue-components` 按需加载 UI 框架组件

1. 配置

   ```js
   AutoImport({
       resolvers: [ElementPlusResolver()],
   }),
       Components({
       resolvers: [ElementPlusResolver()],
   }),
   ```

## 十一、UI 框架换肤方法

### 1、静态方法

1. 通过替换 css 变量实现，全局添加自定类型，类型下面替换变量(全局包裹容器上定义 class 类名)

   ```css
   .theme-class {
     --el-fill-color-blank: yellow;
   }
   ```
