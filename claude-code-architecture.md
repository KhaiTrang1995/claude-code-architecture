# 🏗️ Kiến trúc Claude Code — AI Agent Architecture

> **Claude Code** là một hệ thống AI Agent phức tạp được thiết kế để tự động hóa các tác vụ lập trình.  
> Hệ thống vận hành dựa trên vòng lặp **Perception → Action → Observation** và được module hóa thành 6 layer rõ ràng.
>
> *Claude Code is a complex AI Agent system designed to automate programming tasks.*  
> *The system operates on a **Perception → Action → Observation** loop and is modularized into 6 distinct layers.*

---

## Tổng quan Kiến trúc · *Architecture Overview*

```mermaid
graph TB
    subgraph INPUT["① Input & Permission Layer"]
        direction LR
        IL[" Input Layer<br/>CLI · IDE · CI/CD"]
        SM[" Session Manager<br/>Resume · Fork · Persist"]
        PG[" Permission Gate<br/>YAML Rules (3 levels)"]
        IL --> SM --> PG
    end

    subgraph KNOWLEDGE["② Knowledge Layer"]
        direction LR
        CC[" Context Compressor<br/>3-tier · 92% threshold"]
        MS[" Memory Store<br/>Cross-session persistence"]
        SR[" Skill Registry<br/>On-demand injection"]
        TG[" Task Graph<br/>Priority & Dependencies"]
    end

    subgraph AGENT["③ Master Agent Loop"]
        direction LR
        P[" Perceive"]
        A[" Act"]
        O[" Observe"]
        P --> A --> O --> P
    end

    subgraph INTEGRATION["④ Integration & Execution Layer"]
        direction LR
        MCP[" MCP Runtime<br/>Auto-discover tools"]
        TD[" Tool Dispatch<br/>bash · read · write · grep"]
        SRT[" Streaming Runtime<br/>Parallel execution"]
        PC[" Prompt Cache<br/>Stable prefix reuse → 10% cost"]
    end

    subgraph MULTI["⑤ Multi-Agent Layer"]
        direction LR
        SS[" Subagent Spawner<br/>Isolated context"]
        MB[" Teammate Mailboxes<br/>Redis pub/sub"]
        WI[" Worktree Isolator<br/>Branch per task"]
    end

    subgraph OUTPUT["⑥ Observability & Output Layer"]
        direction LR
        EB[" Event Bus<br/>Lifecycle hooks"]
        BE[" Background Executor<br/>Daemon threads"]
        TR[" Task Result<br/>Verified output"]
    end

    INPUT -->|"filtered request"| KNOWLEDGE
    KNOWLEDGE -->|"enriched context"| AGENT
    AGENT -->|"tool calls"| INTEGRATION
    AGENT -->|"spawn subagents"| MULTI
    INTEGRATION -->|"results"| AGENT
    MULTI -->|"results"| AGENT
    AGENT -->|"final output"| OUTPUT
    OUTPUT -->|"feedback loop"| KNOWLEDGE

    style INPUT fill:#1e293b,stroke:#3b82f6,color:#e2e8f0
    style KNOWLEDGE fill:#1e293b,stroke:#8b5cf6,color:#e2e8f0
    style AGENT fill:#1e293b,stroke:#f59e0b,color:#e2e8f0
    style INTEGRATION fill:#1e293b,stroke:#10b981,color:#e2e8f0
    style MULTI fill:#1e293b,stroke:#ef4444,color:#e2e8f0
    style OUTPUT fill:#1e293b,stroke:#06b6d4,color:#e2e8f0
```

---

## ① Lớp Đầu vào & Kiểm soát · *Input & Permission Layer*

### Input Layer

Tiếp nhận yêu cầu từ nhiều nguồn:  
*Accepts requests from multiple sources:*

| Nguồn · *Source* | Mô tả · *Description* | Ví dụ · *Example* |
|-------|--------|-------|
| **CLI** | Giao diện dòng lệnh · *Command line interface* | `claude "fix bug #42"` |
| **IDE** | Tích hợp VSCode, JetBrains · *VSCode, JetBrains integration* | Claude panel trong editor · *Claude panel in editor* |
| **CI/CD** | Pipeline tự động · *Automated pipeline* | GitHub Actions trigger |

### Session Manager

Quản lý trạng thái phiên làm việc với 3 chế độ:  
*Manages session state with 3 modes:*

```mermaid
stateDiagram-v2
    [*] --> Active: New Session
    Active --> Persisted: Save State
    Persisted --> Active: Resume
    Active --> Forked: Fork Context
    Forked --> Active: Continue
    Active --> [*]: End Session
```

- **Resume** — Tiếp tục phiên trước đó với đầy đủ ngữ cảnh · *Continue previous session with full context*
- **Fork** — Rẽ nhánh từ một điểm trong phiên để thử hướng khác · *Branch from a session point to try a different approach*
- **Persist** — Lưu trữ trạng thái để sử dụng sau · *Save state for later use*

### Permission Gate

Chốt chặn an ninh sử dụng **YAML rules 3 cấp độ**:  
*Security checkpoint using **3-level YAML rules**:*

```mermaid
flowchart LR
    REQ["Request"] --> EVAL{"Evaluate Rules"}
    EVAL -->|Level 1| DENY[" Deny<br/>Block immediately"]
    EVAL -->|Level 2| APPROVE[" Approve<br/>Require confirmation"]
    EVAL -->|Level 3| ALLOW[" Allow<br/>Execute freely"]
```

```yaml
# .claude/settings.json — Permission rules
permissions:
  deny:
    - "rm -rf /"
    - "DROP TABLE"
  approve:
    - "npm publish"
    - "git push --force"
  allow:
    - "git status"
    - "npm test"
```

> **Nguyên tắc:** Permission Gate "quan sát mọi thứ" trước khi cho phép vào vòng lặp chính.  
> ***Principle:** The Permission Gate "observes everything" before allowing entry into the main loop.*

---

## ② Lớp Kiến thức · *Knowledge Layer*

Bộ não cung cấp dữ liệu cho Agent, tối ưu cho việc xử lý ngữ cảnh lớn.  
*The brain that supplies data to the Agent, optimized for handling large contexts.*

### Context Compressor

Cơ chế nén ngữ cảnh **3 lớp** với ngưỡng kích hoạt **92%**:  
*3-layer context compression mechanism with **92%** activation threshold:*

```mermaid
flowchart TB
    RAW["📄 Raw Context<br/>100% tokens"] --> L1
    
    subgraph COMPRESS["Context Compressor"]
        L1["Layer 1: Summarize<br/>Old messages → summaries"] --> L2
        L2["Layer 2: Prune<br/>Remove low-relevance data"] --> L3
        L3["Layer 3: Truncate<br/>Hard token limit enforcement"]
    end
    
    L3 --> OUT[" Compressed Context<br/>~40% tokens"]
    L3 --> FILE[" .agent_memory.md<br/>Persisted to disk"]
    
    THRESHOLD{{"⚡ Trigger: token usage ≥ 92%"}} -.-> COMPRESS
```

### Memory Store & Skill Registry

```mermaid
flowchart LR
    subgraph MEMORY["Memory Store"]
        LTM["Long-term Memory<br/>Cross-session facts"]
        STM["Short-term Memory<br/>Current session context"]
        EP["Episodic Memory<br/>Past task outcomes"]
    end
    
    subgraph SKILLS["Skill Registry"]
        S1["deploy/SKILL.md"]
        S2["review/SKILL.md"]
        S3["custom/SKILL.md"]
    end
    
    PROMPT["Active Prompt"] <-->|"Read/Write"| MEMORY
    SKILLS -->|"On-demand inject"| PROMPT
```

- **Memory Store** — Lưu trữ xuyên suốt phiên, ghi vào `CLAUDE.md` và `.agent_memory.md` · *Cross-session storage, persisted to `CLAUDE.md` and `.agent_memory.md`*
- **Skill Registry** — Tiêm (inject) kỹ năng vào prompt khi phát hiện context phù hợp · *Injects skills into prompts when matching context is detected*

### Task Graph

```mermaid
flowchart TB
    T1["Task A<br/>Priority: HIGH"] --> T3["Task C<br/>Priority: MEDIUM"]
    T2["Task B<br/>Priority: HIGH"] --> T3
    T3 --> T4["Task D<br/>Priority: LOW"]
    T1 --> T5["Task E<br/>Priority: MEDIUM"]
    
    style T1 fill:#dc2626,color:#fff
    style T2 fill:#dc2626,color:#fff
    style T3 fill:#f59e0b,color:#000
    style T5 fill:#f59e0b,color:#000
    style T4 fill:#22c55e,color:#000
```

> Sắp xếp thứ tự thực thi dựa trên **priority** và **dependency graph** — đảm bảo không task nào chạy trước khi dependencies hoàn tất.  
> *Execution order is based on **priority** and **dependency graph** — ensuring no task runs before its dependencies are completed.*

---

## ③ Vòng lặp Trung tâm · *Master Agent Loop*

Đây là **trái tim** của hệ thống — vòng lặp liên tục **Perceive → Act → Observe**:  
*This is the **heart** of the system — a continuous **Perceive → Act → Observe** loop:*

```mermaid
flowchart TB
    START((" Start")) --> PERCEIVE

    PERCEIVE[" PERCEIVE<br/>─────────────<br/>• Read context from Knowledge Layer<br/>• Analyze current state<br/>• Identify what needs to be done"]
    
    PERCEIVE --> DECIDE{" Decision Point"}
    
    DECIDE -->|"Direct tool call"| ACT_TOOL[" ACT: Tool Call<br/>─────────────<br/>• bash, read, write, grep<br/>• Via Tool Dispatch"]
    
    DECIDE -->|"Complex task"| ACT_SPAWN[" ACT: Spawn Subagent<br/>─────────────<br/>• Create child agent<br/>• Isolated context<br/>• Delegated subtask"]
    
    DECIDE -->|"Task complete"| DONE((" Done"))
    
    ACT_TOOL --> OBSERVE[" OBSERVE<br/>─────────────<br/>• Collect tool output<br/>• Evaluate success/failure<br/>• Update context"]
    
    ACT_SPAWN --> OBSERVE
    
    OBSERVE --> PERCEIVE
```

### Đặc điểm chính · *Key Characteristics*

| Đặc điểm · *Feature* | Mô tả · *Description* |
|-----------|--------|
| **Self-correcting** | Tự phát hiện lỗi và thử lại với cách tiếp cận khác · *Auto-detects errors and retries with a different approach* |
| **Context-aware** | Luôn cập nhật ngữ cảnh mới nhất từ Knowledge Layer · *Always updated with the latest context from the Knowledge Layer* |
| **Goal-driven** | Theo dõi mục tiêu cuối cùng, không lạc hướng · *Tracks the final goal, stays on course* |
| **Tool-agnostic** | Có thể gọi bất kỳ tool nào đã đăng ký · *Can invoke any registered tool* |

---

## ④ Lớp Tích hợp & Thực thi · *Integration & Execution Layer*

### MCP Runtime (Model Context Protocol)

```mermaid
flowchart LR
    MCP["🔌 MCP Runtime"] -->|"Auto-discover"| FS[" Filesystem Server"]
    MCP -->|"Auto-discover"| GIT[" Git Server"]
    MCP -->|"Auto-discover"| CUSTOM[" Custom Server"]
    MCP -->|"Auto-discover"| DB[" Database Server"]
    
    FS --> TOOLS["Available Tools"]
    GIT --> TOOLS
    CUSTOM --> TOOLS
    DB --> TOOLS
```

Cấu hình trong `.mcp.json`: · *Configuration in `.mcp.json`:*
```json
{
  "mcpServers": {
    "github": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-github"] },
    "filesystem": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-filesystem", "./"] }
  }
}
```

### Tool Dispatch

Mỗi công cụ có handler riêng biệt:  
*Each tool has a dedicated handler:*

| Tool | Handler | Mô tả · *Description* |
|------|---------|--------|
| `bash` | `BashHandler` | Thực thi shell commands · *Execute shell commands* |
| `read` | `FileReadHandler` | Đọc nội dung file · *Read file contents* |
| `write` | `FileWriteHandler` | Ghi/tạo file · *Write/create files* |
| `grep` | `SearchHandler` | Tìm kiếm trong codebase · *Search the codebase* |
| `edit` | `EditHandler` | Chỉnh sửa file có sẵn · *Edit existing files* |

### Streaming Runtime & Prompt Cache

```mermaid
flowchart LR
    subgraph STREAM["Streaming Runtime"]
        direction TB
        T1["Tool Call 1"] 
        T2["Tool Call 2"]
        T3["Tool Call 3"]
        T1 & T2 & T3 -->|"Parallel"| MERGE["Merge Results"]
    end
    
    subgraph CACHE["Prompt Cache"]
        direction TB
        PREFIX["Stable Prefix<br/>System prompt + rules<br/>━━━━━━━━━━━━━━━━<br/> CACHED (free)"]
        DYNAMIC["Dynamic Suffix<br/>Current conversation<br/>━━━━━━━━━━━━━━━━<br/> Billed (~10%)"]
    end
    
    STREAM --> OUTPUT["Final Output"]
    CACHE --> OUTPUT
```

> **Prompt Cache** giúp giảm chi phí xuống **~10%** bằng cách tái sử dụng stable prefix (system prompt, rules, skills) — chỉ phần dynamic suffix mới tốn token.  
> ***Prompt Cache** reduces cost to **~10%** by reusing the stable prefix (system prompt, rules, skills) — only the dynamic suffix consumes tokens.*

---

## ⑤ Lớp Đa Tác tử · *Multi-Agent Layer*

Khi tác vụ phức tạp cần chia nhỏ:  
*When complex tasks need to be divided:*

### Subagent Spawner & Communication · *Giao tiếp giữa các Agent*

```mermaid
sequenceDiagram
    participant MA as  Master Agent
    participant SS as  Spawner
    participant SA1 as  Subagent 1
    participant SA2 as  Subagent 2
    participant MB as  Mailbox (Redis)

    MA->>SS: Spawn(task_1, isolated_context)
    MA->>SS: Spawn(task_2, isolated_context)
    SS->>SA1: Create with isolated context
    SS->>SA2: Create with isolated context
    
    SA1->>MB: publish(progress_update)
    MB->>MA: subscribe(progress_update)
    
    SA2->>MB: publish(need_info)
    MB->>SA1: relay(need_info)
    SA1->>MB: publish(response)
    MB->>SA2: relay(response)
    
    SA1->>MA: Return result_1
    SA2->>MA: Return result_2
    MA->>MA: Merge results
```

### FSM Protocol — Quản lý trạng thái giao tiếp · *Communication State Management*

```mermaid
stateDiagram-v2
    [*] --> IDLE
    IDLE --> REQUEST: Send request
    REQUEST --> WAIT: Awaiting response
    WAIT --> RESPOND: Response received
    RESPOND --> IDLE: Process complete
    WAIT --> TIMEOUT: No response
    TIMEOUT --> REQUEST: Retry
```

### Worktree Isolator

```mermaid
flowchart TB
    MAIN[" main branch"] --> WT1[" worktree/task-1<br/>Subagent 1"]
    MAIN --> WT2[" worktree/task-2<br/>Subagent 2"]
    MAIN --> WT3[" worktree/task-3<br/>Subagent 3"]
    
    WT1 --> LOCK[" Atomic Lock<br/>(Autonomous Board)"]
    WT2 --> LOCK
    WT3 --> LOCK
    
    LOCK --> MERGE[" Merge to main<br/>Zero conflicts guaranteed"]
```

> Mỗi subagent làm việc trên **branch riêng** → không xung đột code. **Atomic lock** đảm bảo merge tuần tự, an toàn.  
> *Each subagent works on a **separate branch** → no code conflicts. **Atomic lock** ensures sequential, safe merges.*

---

## ⑥ Lớp Giám sát & Đầu ra · *Observability & Output Layer*

### Event Bus & Background Executor

```mermaid
flowchart LR
    subgraph EVENTS["Event Bus"]
        E1["on:tool_start"]
        E2["on:tool_end"]
        E3["on:error"]
        E4["on:agent_spawn"]
        E5["on:session_end"]
    end
    
    subgraph BG["Background Executor"]
        D1[" Log Writer<br/>Daemon thread"]
        D2[" Metrics Collector<br/>Daemon thread"]
        D3[" State Persister<br/>Daemon thread"]
    end
    
    EVENTS -->|"Non-blocking emit"| BG
    BG -->|"Async write"| STORAGE[" Logs & Metrics"]
```

### Output & Feedback Loop

```mermaid
flowchart LR
    RESULT[" Task Result<br/>Verified output"] --> USER[" User"]
    RESULT --> FEEDBACK[" Feedback Loop"]
    FEEDBACK --> MS[" Memory Store<br/>Learn from actions"]
    MS --> NEXT["Next Session<br/>Improved context"]
```

> Kết quả được **xác minh** trước khi trả về. Đồng thời tự động cập nhật vào Memory Store để hệ thống **"học"** từ các thao tác vừa thực hiện.  
> *Results are **verified** before being returned. Simultaneously, the Memory Store is auto-updated so the system **"learns"** from performed actions.*

---

## Mapping: Kiến trúc ↔ Cấu trúc Project · *Architecture ↔ Project Structure*

| Layer | File/Directory trong scaffold · *Files/Directories in scaffold* |
|-------|-------------------------------|
| **Input & Permission** | `.claude/settings.json` — permissions, hooks |
| **Knowledge Layer** | `CLAUDE.md`, `.claude/rules/*.md`, `.claude/skills/` |
| **Master Agent Loop** | Claude Code runtime (built-in) |
| **Integration & Execution** | `.mcp.json`, `.claude/hooks/` |
| **Multi-Agent Layer** | `.claude/agents/*.md` |
| **Observability & Output** | `.claude/hooks/`, Event-driven scripts |

---

## Tóm tắt Luồng Hoạt động End-to-End · *End-to-End Workflow Summary*

```mermaid
flowchart TB
    USER[" User Request<br/>(CLI / IDE / CI)"] 
    --> SESSION[" Session Manager"]
    --> PERM{" Permission Gate"}
    
    PERM -->|"Denied"| BLOCK[" Blocked"]
    PERM -->|"Approved"| KNOW[" Knowledge Layer<br/>Compress + Enrich"]
    
    KNOW --> LOOP[" Master Agent Loop"]
    
    LOOP -->|"Simple task"| TOOL[" Tool Dispatch"]
    LOOP -->|"Complex task"| SPAWN[" Spawn Subagents"]
    
    TOOL --> OBSERVE[" Observe Result"]
    SPAWN --> OBSERVE
    
    OBSERVE -->|"Not done"| LOOP
    OBSERVE -->|"Complete"| VERIFY[" Verify & Output"]
    
    VERIFY --> MEMORY[" Update Memory"]
    VERIFY --> RESULT[" Return to User"]
    
    MEMORY -.->|"Next session"| KNOW
```

---

> **Tài liệu này** mô tả kiến trúc conceptual của Claude Code dựa trên phân tích hệ thống.  
> *This document describes the conceptual architecture of Claude Code based on system analysis.*  
> Xem thêm · *See also*: [Generated Structure](../README.md#generated-structure) · [Template Files](../template/)
