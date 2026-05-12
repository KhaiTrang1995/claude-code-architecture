# 🏗️ Claude Code Architecture & Structure

> **Phân tích kiến trúc hệ thống Claude Code** — tài liệu kỹ thuật + thư viện scaffold sẵn sàng cho production.  
> *Architectural analysis of the Claude Code system — technical documentation + production-ready scaffold library.*

[![npm version](https://img.shields.io/npm/v/claude-code-structure.svg)](https://www.npmjs.com/package/claude-code-structure)
[![PyPI version](https://img.shields.io/pypi/v/claude-code-structure.svg)](https://pypi.org/project/claude-code-structure/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

<img width="1456" height="940" alt="unnamed (1)" src="https://github.com/user-attachments/assets/733f1525-bb79-454c-8d68-3a8ba6ff60ca" />

## 📌 Dự án này gồm gì? · *What's in this project?*

| Thành phần · *Component* | Mô tả · *Description* |
|---|---|
| **[Architecture Doc](claude-code-architecture.md)** | Phân tích kiến trúc 6 layer chi tiết (song ngữ VI/EN) · *Detailed 6-layer architecture analysis (bilingual VI/EN)* |
| **[Visual Architecture](architecture-visual.html)** | Trang HTML trực quan hóa kiến trúc, responsive & animated · *Interactive HTML architecture visualization* |
| **[claude-code-structure](structủe-readme.md)** | Thư viện npm/pip tạo cây thư mục chuẩn cho Claude Code · *npm/pip library to scaffold Claude Code directory structure* |

---

## 🧠 Kiến trúc Claude Code — Tổng quan · *Architecture Overview*

Claude Code là một **AI Agent** vận hành trên vòng lặp **Perception → Action → Observation**, được chia thành 6 layer:  
*Claude Code is an **AI Agent** operating on a **Perception → Action → Observation** loop, organized into 6 layers:*

```
┌─────────────────────────────────────────────────────────┐
│  ① Input & Permission    Tiếp nhận & kiểm soát quyền   │
│  ② Knowledge Layer       Bộ não ngữ cảnh & bộ nhớ      │
│  ③ Master Agent Loop     Vòng lặp Perceive→Act→Observe │
│  ④ Integration Layer     MCP Runtime & Tool Dispatch    │
│  ⑤ Multi-Agent Layer     Subagent & Worktree Isolation  │
│  ⑥ Observability Layer   Event Bus & Feedback Loop      │
└─────────────────────────────────────────────────────────┘
```

### Luồng hoạt động · *How it works*

```
User Request → Session Manager → Permission Gate
  → Knowledge Layer (compress + enrich context)
    → Agent Loop (perceive → act → observe)
      → Tool Dispatch / Spawn Subagents
        → Verify & Output → Update Memory → Return Result
```

> 📖 Xem chi tiết: [claude-code-architecture.md](claude-code-architecture.md) · *Full details in the architecture doc*  
> 🎨 Xem trực quan: mở [architecture-visual.html](architecture-visual.html) trong browser · *Open the visual HTML in your browser*

---

## ⚡ Quick Start — Tạo cây thư mục Claude Code · *Scaffold a Claude Code project*

Thư viện [`claude-code-structure`](https://www.npmjs.com/package/claude-code-structure) giúp bạn tạo nhanh cấu trúc dự án chuẩn cho Claude Code chỉ với 1 lệnh:  
*The [`claude-code-structure`](https://www.npmjs.com/package/claude-code-structure) library lets you scaffold a production-ready Claude Code project in one command:*

### npm / Node.js
```bash
npx claude-code-structure            # tạo tại thư mục hiện tại · scaffold in current dir
npx claude-code-structure ./my-app   # tạo tại thư mục chỉ định · scaffold in specific dir
```

### pip / Python
```bash
pip install claude-code-structure
claude-init                          # tạo tại thư mục hiện tại · scaffold in current dir
claude-init ./my-app                 # tạo tại thư mục chỉ định · scaffold in specific dir
```

### Kết quả · *Generated output*

```
your-project/
├── CLAUDE.md                 ← Load mỗi phiên · Loaded every session
├── CLAUDE.local.md           ← Override cá nhân (gitignored) · Local overrides
├── .mcp.json                 ← Tích hợp MCP · MCP integrations
└── .claude/
    ├── settings.json         ← Quyền & cấu hình · Permissions & config
    ├── rules/                ← Quy tắc code · Code style rules
    │   ├── code-style.md
    │   ├── testing.md
    │   └── api-conventions.md
    ├── commands/             ← Lệnh slash · Slash commands
    │   ├── review.md
    │   └── fix-issue.md
    ├── skills/               ← Kỹ năng auto-inject · Auto-injected skills
    │   └── deploy/
    ├── agents/               ← Agent con · Sub-agents
    │   ├── code-reviewer.md
    │   └── security-auditor.md
    └── hooks/                ← Script sự kiện · Event scripts
        └── validate-bash.sh
```

---

## 🗺️ Mapping: Kiến trúc ↔ Cây thư mục · *Architecture ↔ Directory Structure*

Mỗi layer trong kiến trúc tương ứng với các file/thư mục cụ thể:  
*Each architecture layer maps to specific files/directories:*

| Layer | Files | Vai trò · *Role* |
|---|---|---|
| ① Input & Permission | `.claude/settings.json` | Quyền truy cập, model selection · *Permissions, hooks* |
| ② Knowledge | `CLAUDE.md`, `.claude/rules/`, `.claude/skills/` | Ngữ cảnh, quy tắc, kỹ năng · *Context, rules, skills* |
| ③ Agent Loop | Claude Code runtime (built-in) | Vòng lặp AI chính · *Core orchestration* |
| ④ Integration | `.mcp.json`, `.claude/hooks/` | MCP servers, event scripts |
| ⑤ Multi-Agent | `.claude/agents/*.md` | Định nghĩa agent con · *Subagent definitions* |
| ⑥ Observability | `.claude/hooks/`, Event scripts | Giám sát & feedback · *Monitoring & feedback* |

---

## 🔧 Cách áp dụng vào dự án · *How to apply to your project*

### Bước 1: Scaffold · *Step 1: Scaffold*

```bash
cd your-project
npx claude-code-structure
```

### Bước 2: Tùy chỉnh CLAUDE.md · *Step 2: Customize CLAUDE.md*

Cập nhật tech stack, package manager, và build commands cho dự án của bạn:  
*Update the tech stack, package manager, and build commands for your project:*

```markdown
# CLAUDE.md
## Tech Stack
- Language: TypeScript / Python
- Framework: Next.js / FastAPI
- Package Manager: pnpm / uv
- Build: pnpm build / make build
- Test: pnpm test / pytest
```

### Bước 3: Cấu hình Rules · *Step 3: Configure Rules*

Chỉnh sửa các file trong `.claude/rules/` theo convention của team:  
*Edit files in `.claude/rules/` to match your team's conventions:*

- **`code-style.md`** — Naming, indentation, TypeScript/Python conventions
- **`testing.md`** — Coverage targets (≥80%), testing tools, best practices
- **`api-conventions.md`** — REST design, status codes, error format

### Bước 4: Kết nối MCP · *Step 4: Connect MCP*

Thêm token và cấu hình vào `.mcp.json`:  
*Add tokens and configuration to `.mcp.json`:*

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "${GITHUB_TOKEN}" }
    }
  }
}
```

### Bước 5: Thiết lập Agents · *Step 5: Set up Agents*

Tạo agent con chuyên biệt trong `.claude/agents/`:  
*Create specialized sub-agents in `.claude/agents/`:*

- **`code-reviewer.md`** — Agent review code tự động · *Automated code review agent*
- **`security-auditor.md`** — Agent kiểm tra bảo mật OWASP · *OWASP security audit agent*

---

## 📂 Cấu trúc Repository · *Repository Structure*

```
claude-code-architecture/
├── README.md                        ← Bạn đang đây · You are here
├── claude-code-architecture.md      ← Kiến trúc 6 layer chi tiết · Detailed architecture
├── architecture-visual.html         ← Trực quan hóa HTML · Visual HTML page
├── architecture.md                  ← Hướng dẫn build & publish · Build guide
├── structủe-readme.md               ← README cho thư viện npm/pip · Library README
└── CLAUDE.md                        ← Quy tắc hành vi AI · AI behavioral guidelines
```

---

## 🔗 Tài liệu tham khảo · *References*

- [Claude Code Official Docs](https://docs.anthropic.com/claude/docs/claude-code) — Tài liệu chính thức · *Official documentation*
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) — Giao thức tích hợp công cụ · *Tool integration protocol*
- [claude-code-structure on npm](https://www.npmjs.com/package/claude-code-structure) — Package npm
- [claude-code-structure on PyPI](https://pypi.org/project/claude-code-structure/) — Package Python

---

## 🤝 Contributing

1. Fork repository
2. Tạo nhánh feature: `git checkout -b feat/my-feature`  
   *Create feature branch*
3. Commit theo [Conventional Commits](https://www.conventionalcommits.org/)
4. Push và mở Pull Request  
   *Push and open a Pull Request*

---

## 📄 License

[MIT](LICENSE) © TechSphereX TA
