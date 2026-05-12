# CLAUDE.md

> File này được Claude Code load tự động mỗi phiên làm việc.  
> *This file is automatically loaded by Claude Code at the start of every session.*

---

## Tổng quan Dự án · *Project Overview*

**claude-code-architecture** — Tài liệu phân tích kiến trúc hệ thống Claude Code + thư viện scaffold `claude-code-structure`.  
*Architecture analysis documentation for the Claude Code system + the `claude-code-structure` scaffold library.*

- **Repo**: `github.com/KhaiTrang1995/claude-code-architecture`
- **Ngôn ngữ · *Languages***: Markdown, HTML, JavaScript, Python
- **Package Manager**: npm (Node.js ≥18), pip (Python ≥3.9)
- **License**: MIT
- **Author**: TechSphereX TA

---

## Cấu trúc Dự án · *Project Structure*

```
claude-code-architecture/
├── README.md                        ← Main README (bilingual VI/EN)
├── CLAUDE.md                        ← File này · This file
├── claude-code-architecture.md      ← Kiến trúc 6 layer · 6-layer architecture
├── architecture-visual.html         ← Trực quan hóa HTML · Visual HTML
├── architecture.md                  ← Hướng dẫn build · Build guide
├── structủe-readme.md               ← README cho thư viện npm/pip · Library README
└── .github/
    ├── CONTRIBUTING.md              ← Hướng dẫn đóng góp · Contributing guide
    ├── SECURITY.md                  ← Chính sách bảo mật · Security policy
    └── CODE_OF_CONDUCT.md           ← Quy tắc ứng xử · Code of conduct
```

---

## Lệnh thường dùng · *Common Commands*

```bash
# Scaffold dự án mới · Scaffold a new project
npx claude-code-structure ./my-project

# Python alternative
pip install claude-code-structure
claude-init ./my-project

# Build npm package
cd packages/npm && npm pack

# Build Python package
cd packages/python && python -m build

# Chạy tests · Run tests
npm test          # npm package
pytest            # Python package
```

---

## Quy tắc Tài liệu · *Documentation Rules*

- Tất cả tài liệu phải **song ngữ Việt/Anh** · *All docs must be **bilingual Vietnamese/English***
- Format: `Tiếng Việt · *English translation*` cho headings
- Format: `Nội dung Việt\n*English content*` cho paragraphs
- Sử dụng Mermaid cho diagrams · *Use Mermaid for diagrams*
- Emoji được dùng cho layer numbers (①②③④⑤⑥) · *Emoji used for layer numbering*

---

## Hướng dẫn Hành vi · *Behavioral Guidelines*

Các quy tắc giảm thiểu lỗi phổ biến khi coding. Ưu tiên cẩn thận hơn tốc độ.  
*Guidelines to reduce common coding mistakes. Biased toward caution over speed.*

### 1. Suy nghĩ Trước khi Code · *Think Before Coding*

**Không giả định. Không giấu sự nhầm lẫn. Nêu rõ đánh đổi.**  
***Don't assume. Don't hide confusion. Surface tradeoffs.***

Trước khi triển khai · *Before implementing*:
- Nêu rõ giả định. Nếu không chắc, hỏi · *State assumptions explicitly. If uncertain, ask*
- Nếu có nhiều cách hiểu, trình bày hết — đừng chọn im lặng · *If multiple interpretations exist, present them — don't pick silently*
- Nếu có cách đơn giản hơn, nói ra. Phản biện khi cần · *If a simpler approach exists, say so. Push back when warranted*
- Nếu không rõ, dừng lại. Chỉ ra chỗ khó hiểu. Hỏi · *If something is unclear, stop. Name what's confusing. Ask*

### 2. Đơn giản Trước · *Simplicity First*

**Code tối thiểu giải quyết vấn đề. Không suy đoán.**  
***Minimum code that solves the problem. Nothing speculative.***

- Không thêm tính năng ngoài yêu cầu · *No features beyond what was asked*
- Không tạo abstraction cho code dùng 1 lần · *No abstractions for single-use code*
- Không thêm "linh hoạt" khi chưa được yêu cầu · *No "flexibility" that wasn't requested*
- Không xử lý lỗi cho tình huống không thể xảy ra · *No error handling for impossible scenarios*
- Viết 200 dòng mà có thể 50 → viết lại · *If you write 200 lines and it could be 50, rewrite it*

Tự hỏi: "Senior engineer có nói code này quá phức tạp không?" Nếu có, đơn giản hóa.  
*Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.*

### 3. Thay đổi Chính xác · *Surgical Changes*

**Chỉ chạm vào những gì cần thiết. Chỉ dọn rác của chính mình.**  
***Touch only what you must. Clean up only your own mess.***

Khi sửa code hiện có · *When editing existing code*:
- Không "cải thiện" code, comment, hay formatting lân cận · *Don't "improve" adjacent code, comments, or formatting*
- Không refactor thứ chưa hỏng · *Don't refactor things that aren't broken*
- Theo style hiện có, dù bạn muốn làm khác · *Match existing style, even if you'd do it differently*
- Nếu thấy dead code không liên quan, nhắc — đừng xóa · *If you notice unrelated dead code, mention it — don't delete it*

Khi thay đổi tạo ra code thừa · *When your changes create orphans*:
- Xóa imports/variables/functions mà THAY ĐỔI CỦA BẠN làm dư · *Remove imports/variables/functions that YOUR changes made unused*
- Không xóa dead code có sẵn trừ khi được yêu cầu · *Don't remove pre-existing dead code unless asked*

Tiêu chí: Mọi dòng thay đổi phải liên quan trực tiếp đến yêu cầu của người dùng.  
*The test: Every changed line should trace directly to the user's request.*

### 4. Thực thi Hướng Mục tiêu · *Goal-Driven Execution*

**Định nghĩa tiêu chí thành công. Lặp cho đến khi xác minh.**  
***Define success criteria. Loop until verified.***

Chuyển đổi tác vụ thành mục tiêu có thể kiểm chứng · *Transform tasks into verifiable goals*:
- "Thêm validation" → "Viết tests cho input sai, rồi làm chúng pass" · *"Add validation" → "Write tests for invalid inputs, then make them pass"*
- "Sửa bug" → "Viết test tái hiện bug, rồi làm pass" · *"Fix the bug" → "Write a test that reproduces it, then make it pass"*
- "Refactor X" → "Đảm bảo tests pass trước và sau" · *"Refactor X" → "Ensure tests pass before and after"*

Với tác vụ nhiều bước, nêu kế hoạch ngắn · *For multi-step tasks, state a brief plan*:
```
1. [Bước · Step] → xác minh · verify: [check]
2. [Bước · Step] → xác minh · verify: [check]
3. [Bước · Step] → xác minh · verify: [check]
```

Tiêu chí rõ giúp tự lặp độc lập. Tiêu chí mờ ("làm cho chạy") cần hỏi liên tục.  
*Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.*

---

**Các quy tắc này đang hoạt động nếu:** diff ít thay đổi thừa, ít viết lại do quá phức tạp, và câu hỏi làm rõ đến trước khi code — không phải sau khi mắc lỗi.  
***These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.*