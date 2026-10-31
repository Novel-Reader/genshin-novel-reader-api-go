# Genshin Novel Reader Go Server

基于 Gin 框架构建的 Genshin 小说阅读器后端服务器，支持最新前端版本。

## 快速开始

### 环境要求

- Go 1.16+
- MySQL 5.7+
- Redis 5.0+

### 本地运行

```bash
# 在 genshin-novel-reader-scripts 项目中统一初始化仓库、配置和依赖
bash ../genshin-novel-reader-scripts/scripts/init.sh

# 启动共享数据库服务
cd ../genshin-novel-reader-scripts
docker compose up -d

# 返回 Go API 仓库
cd ../genshin-novel-reader-api-go

# 启动服务
go run main.go
```

## API接口

### 用户管理

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/api/user?email=xxx` | 根据邮箱查询用户 |
| POST | `/api/user` | 创建新用户 |
| PUT | `/api/user` | 更新用户信息 |
| DELETE | `/api/user` | 删除用户 |

### 系统接口

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | `/api/ping` | 健康检查 |
| GET | `/` | 欢迎页面 |

## 项目结构

```
├── config/                 # 配置文件
│   ├── config.go          # 配置加载逻辑
│   └── config.json        # 由 genshin-novel-reader-scripts 生成，不提交
├── db/                    # 数据库操作
│   └── db.go             # 数据库连接和查询
├── middleware/            # 中间件
│   └── middleware.go     # 自定义中间件
├── routes/               # 路由定义
│   ├── index/           # 首页路由
│   ├── ping/            # 健康检查路由
│   ├── user/            # 用户管理路由
│   └── routes.go        # 路由注册
├── templates/           # 模板文件
│   └── welcome.html    # 欢迎页面
├── go.mod              # Go模块依赖
├── go.sum              # 依赖校验
└── main.go             # 应用入口
```

## 配置说明

数据库和 Redis 连接配置由工作区中的 `genshin-novel-reader-scripts/.env` 统一维护，并生成到 `config/config.json`。请勿手动提交该生成文件。

## 数据库初始化

数据库容器和版本化 SQL 迁移由 `genshin-novel-reader-scripts` 项目统一维护。请按该项目 README 中的说明启动数据库并执行迁移。

## 开发指南

### 添加新路由

1. 在 `routes/` 目录下创建新的路由包
2. 实现路由处理函数
3. 在 `routes/routes.go` 中注册路由

### 数据库操作

使用 `db.QueryDB()` 函数执行SQL查询：

```go
results, err := db.QueryDB("SELECT * FROM user WHERE email = ?", email)
```

## 故障排除

### 日志查看

```bash
# 查看实时日志
tail -f novel-reader-go-server.log

# 查看共享数据库日志
docker compose -f ../genshin-novel-reader-scripts/docker-compose.yml logs -f db
```

### 常见问题

1. 端口占用: 在 `genshin-novel-reader-scripts/.env` 中修改对应端口后重新生成配置
2. 数据库连接失败: 检查MySQL服务状态和连接配置
3. 依赖缺失: 运行 `go mod tidy` 重新下载依赖

## 许可证

MIT License
