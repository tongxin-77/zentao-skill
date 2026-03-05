# ZenTao Skill

ZenTao project management system integration with RESTful API support for 18.x+ versions.

## 项目介绍

本工具为 ZenTao（禅道）项目管理系统开源版18.5提供完整的集成支持，通过其官方 `easysoft/go-zentao` SDK 与 ZenTao 的 RESTful API 交互，支持所有主要的 ZenTao 模块和操作。

## 配置说明

在技能根目录中创建一个 `config.json` 文件，包含您的 ZenTao 服务器配置：

```json
{
  "url": "http://your-zentao-url",
  "username": "your-username",
  "password": "your-password"
}
```

## 使用示例

本技能在 `go-zentao/example/` 目录中包含可直接运行的示例程序。

### 任务管理示例 (example/tasks/)

用于管理和查询任务的综合示例，支持灵活的筛选选项。

**查询参数：**
| 参数 | 描述 | 示例 |
|------|------|------|
| `-incomplete` | 只显示未完成的任务 (wait/doing) | `cd go-zentao/example/tasks && go run main.go -incomplete` |
| `-status <value>` | 按状态筛选 | `cd go-zentao/example/tasks && go run main.go -status doing` |
| `-priority <n>` | 按优先级筛选 (1-4) | `cd go-zentao/example/tasks && go run main.go -priority 3` |
| `-project <id>` | 按项目 ID 筛选 | `cd go-zentao/example/tasks && go run main.go -project 20` |
| `-execution <id>` | 按执行 ID 筛选 | `cd go-zentao/example/tasks && go run main.go -execution 24` |
| `-h` / `-help` | 显示帮助 | `cd go-zentao/example/tasks && go run main.go -h` |

**创建任务参数：**
| 参数 | 描述 | 是否必需 |
|------|------|----------|
| `-create <executionID>` | 在指定执行中创建任务 | 是 |
| `-name <string>` | 任务名称 | 是 |
| `-type <string>` | 任务类型 (devel/test/doc/...) | 否 (默认: devel) |
| `-pri <1-4>` | 优先级 | 否 (默认: 3) |
| `-estimate <hours>` | 预估小时数 | 否 |
| `-assignedTo <user>` | 分配给用户 | 否 (默认: 当前用户) |
| `-comment <text>` | 备注 | 否 |

**任务操作参数：**
| 操作 | 命令 |
|------|------|
| 开始任务 | `-start <taskID> [-comment "text"]` |
| 完成任务 | `-finish <taskID> [-consumed 8] [-comment "text"]` |
| 暂停任务 | `-pause <taskID> [-comment "text"]` |
| 继续任务 | `-continue <taskID> [-comment "text"]` |
| 关闭任务 | `-close <taskID> [-comment "text"]` |

**使用示例：**
```bash
cd go-zentao/example/tasks

# 查询任务
go run main.go                          # 显示所有任务
go run main.go -incomplete             # 只显示未完成的任务
go run main.go -status doing           # 按状态筛选
go run main.go -priority 2             # 按优先级筛选

# 创建任务
go run main.go -create 24 -name "实现登录功能" -pri 2 -estimate 8

# 任务操作
go run main.go -start 123 -comment "开始工作"
go run main.go -finish 123 -consumed 8 -comment "任务完成"
go run main.go -pause 123
go run main.go -continue 123
go run main.go -close 123
```

### Bug 管理示例 (example/bugs/)

用于查询和确认分配给当前用户的 Bug 的示例。

**功能：**
- 获取分配给当前用户的所有 Bug
- 按产品组织
- 显示 Bug 详细信息（ID、标题、状态、严重程度、优先级）
- 使用更新的元数据确认 Bug

**查询使用：**
```bash
cd go-zentao/example/bugs
go run main.go
```

**确认 Bug 参数：**
| 参数 | 描述 | 是否必需 |
|------|------|----------|
| `-confirm <bugID>` | 确认指定的 Bug | 是 |
| `-pri <1-4>` | 优先级 | 否 (默认: 3) |
| `-type <string>` | Bug 类型 (codeerror/designdefect/config/...) | 否 (默认: codeerror) |
| `-status <string>` | 状态 (active/resolved/closed) | 否 (默认: active) |
| `-deadline <YYYY-MM-DD>` | 截止日期 | 否 |
| `-assignedTo <user>` | 分配给用户 | 否 (默认: 当前用户) |
| `-comment <text>` | 备注 | 否 |

**确认 Bug 使用：**
```bash
cd go-zentao/example/bugs
go run main.go -confirm 456 -pri 1 -type codeerror -status active -assignedTo tx
```

## API 文档链接

- ZenTao 官方 API 文档：https://www.zentao.net/book/api.html
- go-zentao SDK GitHub 仓库：https://github.com/easysoft/go-zentao

## 注意事项

- 始终确保您的 ZenTao 用户具有适当的权限
- 推荐使用基于 Token 的认证而不是基于会话的认证
- 使用特定 API 前请检查 ZenTao 版本兼容性
- 某些功能可能在开源版本中不可用（API 文档中有说明）
