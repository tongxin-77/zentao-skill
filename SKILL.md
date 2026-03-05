---
name: zentao
description: ZenTao project management system integration with RESTful API support for 18.x+ versions
---

## When to use me

Use this skill when you need to:
- Integrate with ZenTao project management system
- Automate project management tasks via API
- Create, query, update, or delete projects, tasks, bugs, stories, etc.
- Manage users, departments, and permissions
- Work with ZenTao 18.x and above versions

## How I work

This skill leverages the official `easysoft/go-zentao` SDK to interact with ZenTao's RESTful API. It provides comprehensive support for all major ZenTao modules and operations.

## Configuration

Create a `config.json` file in the skill root directory with your ZenTao server configuration:

```json
{
  "url": "http://your-zentao-url",
  "username": "your-username",
  "password": "your-password"
}
```

## Example Programs

This skill includes ready-to-run example programs in the `go-zentao/example/` directory.

### Tasks Example (`example/tasks/`)

A comprehensive example for managing and querying tasks with flexible filtering options.

**Features:**
- Get all tasks assigned to current user
- Filter by task status (wait/doing/done/cancel)
- Filter by priority (1-4)
- Filter by specific project or execution
- Quick filter for incomplete tasks
- Create new tasks
- Start, pause, continue, finish, and close tasks

**Query Parameters:**
| Parameter | Description | Example |
|-----------|-------------|---------|
| `-incomplete` | Show only incomplete tasks (wait/doing) | `cd go-zentao/example/tasks && go run main.go -incomplete` |
| `-status <value>` | Filter by status | `cd go-zentao/example/tasks && go run main.go -status doing` |
| `-priority <n>` | Filter by priority (1-4) | `cd go-zentao/example/tasks && go run main.go -priority 3` |
| `-project <id>` | Filter by project ID | `cd go-zentao/example/tasks && go run main.go -project 20` |
| `-execution <id>` | Filter by execution ID | `cd go-zentao/example/tasks && go run main.go -execution 24` |
| `-h` / `-help` | Show help | `cd go-zentao/example/tasks && go run main.go -h` |

**Create Task Parameters:**
| Parameter | Description | Required |
|-----------|-------------|----------|
| `-create <executionID>` | Create task in specified execution | Yes |
| `-name <string>` | Task name | Yes |
| `-type <string>` | Task type (devel/test/doc/...) | No (default: devel) |
| `-pri <1-4>` | Priority | No (default: 3) |
| `-estimate <hours>` | Estimated hours | No |
| `-assignedTo <user>` | Assign to user | No (default: current user) |
| `-comment <text>` | Comment | No |

**Task Action Parameters:**
| Action | Command |
|--------|---------|
| Start task | `-start <taskID> [-comment "text"]` |
| Finish task | `-finish <taskID> [-consumed 8] [-comment "text"]` |
| Pause task | `-pause <taskID> [-comment "text"]` |
| Continue task | `-continue <taskID> [-comment "text"]` |
| Close task | `-close <taskID> [-comment "text"]` |

**Usage Examples:**
```bash
cd go-zentao/example/tasks

# Query tasks
go run main.go                          # Show all tasks
go run main.go -incomplete             # Show only incomplete tasks
go run main.go -status doing           # Filter by status
go run main.go -priority 2             # Filter by priority

# Create task
go run main.go -create 24 -name "Implement login feature" -pri 2 -estimate 8

# Task operations
go run main.go -start 123 -comment "Starting work"
go run main.go -finish 123 -consumed 8 -comment "Task completed"
go run main.go -pause 123
go run main.go -continue 123
go run main.go -close 123
```

### Bugs Example (`example/bugs/`)

Example for querying and confirming bugs assigned to current user.

**Features:**
- Get all bugs assigned to current user
- Organized by product
- Show bug details (ID, title, status, severity, priority)
- Confirm bugs with updated metadata

**Query Usage:**
```bash
cd go-zentao/example/bugs
go run main.go
```

**Confirm Bug Parameters:**
| Parameter | Description | Required |
|-----------|-------------|----------|
| `-confirm <bugID>` | Confirm specified bug | Yes |
| `-pri <1-4>` | Priority | No (default: 3) |
| `-type <string>` | Bug type (codeerror/designdefect/config/...) | No (default: codeerror) |
| `-status <string>` | Status (active/resolved/closed) | No (default: active) |
| `-deadline <YYYY-MM-DD>` | Deadline | No |
| `-assignedTo <user>` | Assign to user | No (default: current user) |
| `-comment <text>` | Comment | No |

**Confirm Bug Usage:**
```bash
cd go-zentao/example/bugs
go run main.go -confirm 456 -pri 1 -type codeerror -status active -assignedTo tx
```

## Notes

- Always ensure your ZenTao user has appropriate permissions
- Token-based authentication is recommended over session-based
- Check ZenTao version compatibility before using specific APIs
- Some features may not be available in open-source edition (indicated in API docs)
