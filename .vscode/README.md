# wewe-rss VSCode 配置说明

本目录包含 wewe-rss 子模块的 VSCode 配置，已自动检测并配置所有语言工具链。

## 检测到的技术栈

### Node.js / TypeScript (主要语言)

- **工具链**: pnpm (全项目统一)
- **配置文件**: `package.json`, `pnpm-workspace.yaml`, `pnpm-lock.yaml`, `tsconfig.json`
- **Node.js 版本**: 20.9.0+
- **pnpm 版本**: 8.6.1+
- **项目类型**: pnpm monorepo (NestJS 后端 + Vite 前端)
- **Formatter**: Prettier
- **Linter**: ESLint
- **子项目**:
  - `apps/server`: NestJS 后端应用 (TypeScript)
  - `apps/web`: Vite + React 前端应用 (TypeScript + React)

### Python (wewe-rss-dingtalk 子项目)

- **工具链**: pip (主要) + conda (备选)
- **配置文件**: `wewe-rss-dingtalk/requirements.txt`
- **Python 版本**: 3.8+
- **Linter**: flake8
- **Formatter**: black
- **类型检查**: mypy
- **项目类型**: DingTalk 机器人集成

## 快速开始

### Node.js 环境设置

#### 安装依赖 (使用 pnpm)

```bash
# 全局安装 pnpm (如果未安装)
npm install -g pnpm

# 或使用 corepack (Node.js 16.13+)
corepack enable pnpm

# 安装项目依赖
pnpm install

# 或仅安装特定工作区
pnpm --filter server install
pnpm --filter web install
```

#### 开发

```bash
# 启动所有应用 (并行)
pnpm dev

# 或启动特定应用
pnpm --filter server dev
pnpm --filter web dev

# 构建
pnpm build:server
pnpm build:web

# 代码格式化
pnpm fmt

# 检查格式
pnpm fmt.check

# Linting
pnpm --filter server lint
pnpm --filter web lint

# 测试
pnpm --filter server test
pnpm --filter server test:watch
pnpm --filter server test:cov

# 数据库迁移
pnpm --filter server migrate

# Prisma Studio
pnpm --filter server studio
```

### Python 环境设置 (wewe-rss-dingtalk)

#### 方案 1: 使用 pip (主要 - 推荐)

```bash
# 进入子项目目录
cd wewe-rss-dingtalk

# 创建虚拟环境
python -m venv .venv

# 激活虚拟环境
source .venv/bin/activate  # Linux/macOS
# 或
.venv\Scripts\activate  # Windows

# 安装依赖
pip install -r requirements.txt

# 运行
python main.py
```

#### 方案 2: 使用 conda (备选)

```bash
# 创建 conda 环境
conda create -n wewe-rss-dingtalk python=3.10

# 激活环境
conda activate wewe-rss-dingtalk

# 安装依赖
pip install -r wewe-rss-dingtalk/requirements.txt

# 运行
python wewe-rss-dingtalk/main.py
```

**切换方式**: 编辑 `settings.json`

- 使用 pip: 保持当前配置 (默认)
- 使用 conda: 注释掉 pip 配置，取消注释 conda 配置部分

## 配置说明

### 自动格式化

- **TypeScript/JavaScript**: 保存时自动使用 Prettier 格式化
- **Python**: 保存时自动使用 black 格式化
- **导入排序**: TypeScript 自动组织导入，Python 自动组织导入

### 自动 Linting

- **TypeScript/JavaScript**: ESLint 自动检查和修复
- **Python**: flake8 自动检查
- **CSS/SCSS**: Stylelint 自动检查

### 排除的目录

以下目录已配置为隐藏，以减少编辑器负担:

- `node_modules/`, `.next/`, `dist/`, `build/`
- `.turbo/`, `.cache/`, `coverage/`
- `.venv/`, `venv/`, `__pycache__/`
- `.eslintcache/`, `.stylelintcache/`

### 工具链切换

#### Python: pip ↔ conda

编辑 `settings.json` 中的 `python.defaultInterpreterPath`:

##### 使用 pip (默认)

```json
"python.defaultInterpreterPath": "${workspaceFolder}/wewe-rss-dingtalk/.venv/bin/python",
```

##### 使用 conda

```json
"python.defaultInterpreterPath": "${workspaceFolder}/wewe-rss-dingtalk/env/bin/python",
"conda.condaPath": "conda",
"conda.envPath": "${workspaceFolder}/wewe-rss-dingtalk/env",
```

## 必需的 VSCode 扩展

### Node.js / TypeScript (必需)

- **ESLint** (dbaeumer.vscode-eslint) - 代码检查
- **Prettier** (esbenp.prettier-vscode) - 代码格式化

### Python (可选，用于 wewe-rss-dingtalk)

- **Python** (ms-python.python) - 官方 Python 扩展，包含 Pylance 语言服务器

### 推荐

- **Prisma** (Prisma.prisma) - Prisma ORM 支持
- **Tailwind CSS IntelliSense** (bradlc.vscode-tailwindcss) - Tailwind CSS 支持
- **PostCSS Language Support** (csstools.postcss) - PostCSS 支持
- **Git Graph** (mhutchie.git-graph) - Git 可视化
- **YAML** (redhat.vscode-yaml) - YAML 语法支持
- **Even Better TOML** (tamasfe.even-better-toml) - TOML 语法支持
- **Markdown All in One** (yzhang.markdown-all-in-one) - Markdown 支持

## 项目结构

```text
wewe-rss/
├── apps/
│   ├── server/                 # NestJS 后端应用
│   │   ├── src/               # 源代码
│   │   ├── test/              # 测试文件
│   │   ├── prisma/            # Prisma 数据库配置
│   │   ├── package.json
│   │   └── tsconfig.json
│   └── web/                    # Vite + React 前端应用
│       ├── src/               # 源代码
│       ├── index.html
│       ├── package.json
│       ├── tsconfig.json
│       ├── vite.config.ts
│       └── tailwind.config.ts
├── wewe-rss-dingtalk/         # DingTalk 机器人集成
│   ├── main.py
│   ├── requirements.txt
│   └── Dockerfile
├── package.json               # 根 package.json
├── pnpm-workspace.yaml        # pnpm 工作区配置
├── pnpm-lock.yaml            # 依赖锁定文件
├── tsconfig.json             # 根 TypeScript 配置
├── .prettierrc.json          # Prettier 配置
├── docker-compose.yml        # Docker Compose 配置
└── .vscode/                  # VSCode 配置 (团队共享)
```

## 常见问题

### Q: pnpm 命令找不到

A: 确保已全局安装 pnpm: `npm install -g pnpm` 或启用 corepack: `corepack enable pnpm`

### Q: TypeScript 错误提示

A: 确保已运行 `pnpm install` 并且 `typescript.tsdk` 指向正确的路径。

### Q: 格式化不工作

A: 确保已安装 Prettier 和 ESLint 扩展，并在 VSCode 中启用格式化。

### Q: Python 解释器未找到

A: 确保已创建虚拟环境或 conda 环境并激活。检查 `settings.json` 中的 `python.defaultInterpreterPath` 是否正确。

### Q: 如何运行数据库迁移

A: 运行 `pnpm --filter server migrate` 进行数据库迁移。

### Q: 如何使用 Prisma Studio

A: 运行 `pnpm --filter server studio` 打开 Prisma Studio。

### Q: 如何构建 Docker 镜像

A: 使用 `docker-compose build` 或 `docker build -f Dockerfile .`

## 架构说明

wewe-rss 是一个 RSS 聚合和分发系统，包含以下核心组件：

- **Server (NestJS)**: 后端 API 服务，处理 RSS 订阅、解析和存储
- **Web (Vite + React)**: 前端 Web 应用，提供用户界面
- **wewe-rss-dingtalk**: DingTalk 机器人集成，用于推送 RSS 更新到钉钉

所有组件通过 pnpm monorepo 统一管理，支持并行开发和构建。

## 开发工作流

1. **克隆仓库**

   ```bash
   git clone https://github.com/cooderl/wewe-rss.git
   cd wewe-rss
   ```

2. **安装依赖**

   ```bash
   pnpm install
   ```

3. **配置环境**

   ```bash
   cp apps/server/.env.local.example apps/server/.env.local
   cp apps/web/.env.local.example apps/web/.env.local
   ```

4. **启动开发服务器**

   ```bash
   pnpm dev
   ```

5. **访问应用**
   - 后端 API: [http://localhost:3000](http://localhost:3000)
   - 前端应用: [http://localhost:5173](http://localhost:5173)

## 部署

### Docker 部署

```bash
# 构建镜像
docker-compose build

# 启动容器
docker-compose up -d

# 查看日志
docker-compose logs -f
```

### 生产构建

```bash
# 构建后端
pnpm build:server

# 构建前端
pnpm build:web

# 启动生产服务器
pnpm start:server
pnpm start:web
```

## 环境变量

### Server (.env.local)

```bash
DATABASE_URL=postgresql://user:password@localhost:5432/wewe-rss
NODE_ENV=development
```

### Web (.env.local)

```bash
VITE_API_URL=http://localhost:3000
```

## 注意事项

1. **.vscode 文件夹已提交到 Git**，用于团队共享配置
2. **pnpm 工作区**: 所有依赖管理命令必须使用 pnpm，不支持 npm 或 yarn
3. **Node.js 版本**: 推荐 Node.js 20.9.0+ (根据 package.json)
4. **TypeScript 版本**: 5.3.3 (根据 apps/server/package.json)
5. **Python 版本**: 3.8+ (用于 wewe-rss-dingtalk)

## 关键命令速查

```bash
# 开发
pnpm dev                    # 启动所有应用
pnpm --filter server dev    # 启动后端
pnpm --filter web dev       # 启动前端

# 构建
pnpm build:server           # 构建后端
pnpm build:web              # 构建前端

# 代码质量
pnpm fmt                    # 格式化代码
pnpm fmt.check              # 检查格式
pnpm --filter server lint   # Lint 后端
pnpm --filter web lint      # Lint 前端

# 测试
pnpm --filter server test   # 运行测试
pnpm --filter server test:cov  # 生成覆盖率报告

# 数据库
pnpm --filter server migrate    # 数据库迁移
pnpm --filter server studio     # Prisma Studio

# 生产
pnpm start:server           # 启动生产后端
pnpm start:web              # 启动生产前端
```

---

**最后更新**: 2025-11-17
