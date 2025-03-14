# 情感共振系统 (Emotional Resonance System)

这是一个基于Vue3+Vite+Tailwind CSS的情感可视化系统，用于展示和交互不同的情感状态。

## 项目功能

- 情感状态机：可视化和切换不同的情感状态
- 心率监测器：根据情感状态显示模拟心率
- 情感构成：展示不同情感的比例分布
- 情感记忆与日记：记录和查看情感体验
- 情感触发器：自定义情感触发条件
- 情感表达：生成与当前情感相关的表达方式

## 项目结构

```
emotion-monica/
├── src/                  # 源代码目录
│   ├── assets/           # 静态资源
│   ├── components/       # 组件
│   ├── App.vue           # 主应用组件
│   ├── emotion.vue       # 情感可视化组件
│   ├── main.js           # 入口文件
│   └── style.css         # 全局样式
├── public/               # 公共资源
├── index.html            # HTML模板
├── package.json          # 项目依赖
├── tailwind.config.js    # Tailwind配置
├── postcss.config.js     # PostCSS配置
└── vite.config.js        # Vite配置
```

## 安装与运行

1. 安装依赖：
```bash
npm install
```

2. 启动开发服务器：
```bash
npm run dev
```

3. 构建生产版本：
```bash
npm run build
```

## 技术栈

- Vue 3 - 前端框架
- Vite - 构建工具
- Tailwind CSS - 样式框架
- Lucide Vue - 图标库

## 项目进度

- [x] 基础环境配置
- [x] 情感状态机实现
- [x] 心率监测器实现
- [x] 情感构成可视化
- [x] 情感记忆与日记功能
- [x] 情感触发器功能
- [x] 情感表达生成器
- [ ] 数据持久化
- [ ] 用户自定义主题
