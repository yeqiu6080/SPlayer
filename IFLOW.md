# SPlayer 项目上下文

## 项目概述

SPlayer 是一个简约的音乐播放器，基于 Electron + Vue 3 + TypeScript + Naïve UI 开发。项目支持网页端和桌面客户端，目前主要适配 Windows 平台，其他平台可自行解决兼容性后构建。

### 技术栈

- **前端框架**: Vue 3 (Composition API)
- **编程语言**: TypeScript
- **UI 组件库**: Naïve UI
- **桌面框架**: Electron 38.2.2
- **构建工具**: Vite 7.3.0 + electron-vite 5.0.0
- **状态管理**: Pinia 3.0.4 (带持久化插件)
- **路由**: Vue Router 4.6.3
- **包管理器**: pnpm (推荐)
- **Node.js 版本**: >= 20
- **npm 版本**: >= 10

### 核心依赖

- `@neteasecloudmusicapienhanced/api`: 网易云音乐 API
- `@applemusic-like-lyrics/*`: Apple Music 风格歌词
- `@material/material-color-utilities`: Material Design 配色工具
- `@pixi/*`: 音乐频谱可视化
- `electron-store`: Electron 数据持久化
- `axios`: HTTP 请求
- `localforage`: 本地存储
- `music-metadata`: 音乐元数据解析

### 原生模块

- `discord-rpc-for-splayer`: Discord Rich Presence 集成
- `smtc-for-splayer`: Windows SMTC (系统媒体传输控件) 集成

## 项目结构

```
SPlayer/
├── electron/              # Electron 主进程和预加载脚本
│   ├── main/             # 主进程代码
│   │   ├── ipc/          # IPC 通信处理
│   │   ├── services/     # 服务层
│   │   ├── windows/      # 窗口管理
│   │   ├── tray/         # 系统托盘
│   │   └── utils/        # 工具函数
│   ├── preload/          # 预加载脚本
│   └── server/           # 内置 API 服务器
├── native/               # Rust 原生模块
│   ├── discord-rpc-for-splayer/
│   └── smtc-for-splayer/
├── public/               # 静态资源
├── src/                  # 前端源码
│   ├── api/              # API 接口封装
│   ├── components/       # Vue 组件
│   ├── composables/      # 组合式函数
│   ├── constants/        # 常量定义
│   ├── core/             # 核心功能
│   │   ├── player/       # 播放器核心
│   │   └── resource/     # 资源管理
│   ├── layout/           # 布局组件
│   ├── router/           # 路由配置
│   ├── stores/           # Pinia 状态管理
│   ├── style/            # 全局样式
│   ├── types/            # TypeScript 类型定义
│   ├── utils/            # 工具函数
│   └── views/            # 页面视图
├── docs/                 # VitePress 文档
└── scripts/              # 构建脚本
```

## 构建和运行

### 环境要求

- Node.js >= 20
- npm >= 10
- pnpm (推荐安装: `npm install pnpm -g`)

### 开发命令

```bash
# 安装依赖
pnpm install

# 构建原生模块
pnpm build:native

# 启动开发模式
pnpm dev

# 代码格式化
pnpm format

# 代码检查和自动修复
pnpm lint

# TypeScript 类型检查
pnpm typecheck

# 仅检查 Node.js 类型
pnpm typecheck:node

# 仅检查 Web 类型
pnpm typecheck:web
```

### 构建命令

```bash
# 构建所有平台（当前系统架构）
pnpm build

# 构建 Web 版
pnpm build:web

# 构建不打包版本
pnpm build:unpack

# 构建 Windows 版
pnpm build:win

# 构建 macOS 版
pnpm build:mac

# 构建 Linux 版
pnpm build:linux

# 构建特定架构（例如同时构建 x64 和 arm64）
pnpm build:win -- --x64 --arm64
```

### 文档构建

```bash
# 启动文档开发服务器
pnpm docs:dev

# 构建文档
pnpm docs:build

# 预览文档
pnpm docs:preview
```

## 环境配置

项目使用 `.env` 文件配置环境变量。复制 `.env.example` 为 `.env` 并修改配置：

```env
# WEB 端口
VITE_WEB_PORT=14558

# API 端口
VITE_SERVER_PORT=25884

# API 地址 - 结尾不要加 /
VITE_API_URL=/api/netease
```

## 开发约定

### 代码风格

- 使用 **Prettier** 进行代码格式化
- 使用 **ESLint** 进行代码检查
- TypeScript 严格模式
- 单引号: `false` (使用双引号)
- 尾随逗号: `all`
- 缩进: 2 空格
- 分号: `true`
- 行宽: 100 字符

### TypeScript 配置

- 项目使用 TypeScript 项目引用
- 主配置: `tsconfig.json`
- Node.js 配置: `tsconfig.node.json`
- Web 配置: `tsconfig.web.json`
- 运行类型检查: `pnpm typecheck`

### 组件开发

- 使用 **Vue 3 Composition API**
- 自动导入 Vue 和 VueUse 的组合式函数
- 自动导入 Naïve UI 组件
- 自定义指令: `v-debounce`、`v-throttle`、`v-visible`

### 状态管理

- 使用 **Pinia** 进行状态管理
- 使用 `pinia-plugin-persistedstate` 进行持久化
- stores 按功能模块划分

### 路由

- 使用 **Vue Router 4**
- 页面视图位于 `src/views/`

### IPC 通信

- 主进程 IPC 处理: `electron/main/ipc/`
- 预加载脚本: `electron/preload/index.ts`
- 前端 IPC 初始化: `src/utils/initIpc.ts`

## 主要功能

- 扫码/手机号登录网易云音乐
- 每日签到（每日签到和云贝签到）
- 桌面歌词（支持 Apple Music 风格）
- 本地播放器模式
- 封面主题色自适应
- Light/Dark/Auto 模式切换
- 本地歌曲管理和编辑
- 无版权歌曲播放（客户端独占）
- 歌曲下载（支持 Hi-Res）
- 歌单管理和收藏
- 每日推荐和私人 FM
- 云盘音乐管理
- 逐字歌词和翻译
- MV 和视频播放
- 音乐频谱显示
- 音乐渐入渐出
- PWA 支持
- 评论区
- Last.fm Scrobble
- Discord Rich Presence
- Windows SMTC 集成

## 构建输出

- **Electron 应用**: 输出到 `dist/` 目录
- **Web 应用**: 输出到 `out/renderer/` 目录
- **原生模块**: 输出到 `target/` 目录

## 部署方式

### Docker 部署

```bash
# 本地构建
docker build -t splayer .
docker run -d --name SPlayer -p 25884:25884 splayer

# 使用 Docker Compose
docker-compose up -d

# 在线部署
docker pull imsyy/splayer:latest
docker run -d --name SPlayer -p 25884:25884 imsyy/splayer:latest
```

### Vercel 部署

1. 部署 NeteaseCloudMusicApi 并获取 API 地址
2. Fork 本仓库
3. 创建 `.env` 文件并配置 `VITE_API_URL`
4. 将 `Output Directory` 设置为 `out/renderer`
5. 点击 Deploy

### 服务器部署

```bash
git clone https://github.com/imsyy/SPlayer.git
pnpm install
pnpm build
# 将站点运行目录设置为 out/renderer
```

## 注意事项

1. **许可协议**: 项目使用 AGPL-3.0 许可，修改和分发必须基于 AGPL-3.0 并提供源代码
2. **仅限学习**: 仅供个人学习研究使用，禁止用于商业及非法用途
3. **移动端**: 仅做基础适配，不保证功能全部可用
4. **原生模块**: 开发前需要先构建原生模块 (`pnpm build:native`)
5. **端口配置**: 确保 `VITE_WEB_PORT` 和 `VITE_SERVER_PORT` 不被占用
6. **API 依赖**: 项目依赖 NeteaseCloudMusicApi 运行

## 常见问题

### 构建失败

- 确保已安装 Node.js >= 20
- 确保已安装 pnpm
- 先运行 `pnpm build:native` 构建原生模块
- 检查端口是否被占用

### 类型错误

- 运行 `pnpm typecheck` 检查类型错误
- 确保 `auto-imports.d.ts` 和 `components.d.ts` 已生成

### 原生模块问题

- Windows: 确保 Rust 工具链已安装
- macOS ARM: 可能需要特殊处理，参考文档中的 `troubleshooting/macos-arm-api.md`
- Linux: 可能需要解决沙盒问题，参考 `troubleshooting/ubuntu-sandbox.md`

## 相关文档

- [API 文档](https://splayer.imsyy.top/api.html)
- [贡献指南](docs/contributing.md)
- [下载页面](docs/download.md)
- [使用指南](docs/guide.md)
- [API 说明](docs/api.md)
- [Socket 说明](docs/socket.md)
- [原生模块说明](docs/native.md)

## 许可证

GNU Affero General Public License (AGPL-3.0)