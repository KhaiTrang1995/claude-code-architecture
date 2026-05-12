# Hướng dẫn Đóng góp · *Contributing Guide*

Cảm ơn bạn đã quan tâm đến dự án **claude-code-architecture** của TechSphereX Studio! Tài liệu này hướng dẫn cách đóng góp hiệu quả.  
*Thank you for your interest in contributing to the **claude-code-architecture** project by TechSphereX Studio! This document provides guidelines for effective contributions.*

---

## Quy tắc Ứng xử · *Code of Conduct*

Dự án tuân thủ Quy tắc Ứng xử mà tất cả người đóng góp cần tuân theo. Vui lòng đọc [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) trước khi đóng góp.  
*This project adheres to a Code of Conduct that all contributors are expected to follow. Please read [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) before contributing.*

---

## Bắt đầu · *Getting Started*

### 1. Fork & Clone repository

```bash
git clone https://github.com/KhaiTrang1995/claude-code-architecture.git
cd claude-code-architecture
```

### 2. Cài đặt dependencies · *Install dependencies*

```bash
# Cho npm package · For npm package
cd packages/npm
npm install

# Cho Python package · For Python package
cd packages/python
pip install -e ".[dev]"
```

### 3. Cài đặt pre-commit hooks · *Install pre-commit hooks*

```bash
pre-commit install
```

### 4. Chạy tests · *Run tests*

```bash
# npm
npm test

# Python
pytest
```

---

## Quy trình Phát triển · *Development Workflow*

### 1. Tạo nhánh · *Create a Branch*

```bash
git checkout -b feature/your-feature-name
# hoặc · or
git checkout -b fix/your-bug-fix
```

Quy ước đặt tên nhánh · *Branch naming conventions*:

| Tiền tố · *Prefix* | Mục đích · *Purpose* | Ví dụ · *Example* |
|---|---|---|
| `feature/` | Tính năng mới · *New feature* | `feature/add-hook-template` |
| `fix/` | Sửa lỗi · *Bug fix* | `fix/yaml-parsing-error` |
| `docs/` | Tài liệu · *Documentation* | `docs/update-architecture` |
| `refactor/` | Tái cấu trúc · *Refactoring* | `refactor/simplify-init` |

### 2. Thực hiện thay đổi · *Make Changes*

- Đảm bảo code tuân thủ style guide hiện có · *Ensure code follows existing style guide*
- Viết tests cho tính năng mới · *Write tests for new features*
- Cập nhật tài liệu nếu cần · *Update documentation if needed*

### 3. Commit Changes

Chúng tôi sử dụng [Conventional Commits](https://www.conventionalcommits.org/):  
*We follow [Conventional Commits](https://www.conventionalcommits.org/):*

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Các loại commit · *Commit types*:**

| Type | Mô tả · *Description* |
|---|---|
| `feat` | Tính năng mới · *New feature* |
| `fix` | Sửa lỗi · *Bug fix* |
| `docs` | Thay đổi tài liệu · *Documentation changes* |
| `style` | Thay đổi định dạng code · *Code style changes (formatting, etc.)* |
| `refactor` | Tái cấu trúc code · *Code refactoring* |
| `test` | Thêm/sửa tests · *Test additions or changes* |
| `chore` | Thay đổi build/tool · *Build process or auxiliary tool changes* |

**Ví dụ · *Example*:**
```
feat(template): Add security-auditor agent template

- Add .claude/agents/security-auditor.md
- Include OWASP Top 10 scanning rules
- Add comprehensive documentation

Closes #123
```

### 4. Push và tạo Pull Request · *Push and Create Pull Request*

```bash
git push origin your-branch-name
```

Sau đó tạo Pull Request trên GitHub.  
*Then create a Pull Request on GitHub.*

---

## Hướng dẫn Pull Request · *Pull Request Guidelines*

### Checklist trước khi gửi · *Before Submitting*

- [ ] Code tuân thủ style guide · *Code follows project style guidelines*
- [ ] Tất cả tests pass · *All tests pass*
- [ ] Đã thêm tests cho tính năng mới · *New tests added for new functionality*
- [ ] Tài liệu đã cập nhật · *Documentation updated*
- [ ] Commit messages đúng convention · *Commit messages follow convention*
- [ ] Không có merge conflicts với main · *No merge conflicts with main branch*
- [ ] Đã test trên cả npm và Python (nếu liên quan) · *Tested on both npm and Python (if applicable)*

### Mô tả Pull Request · *Pull Request Description*

Bao gồm các thông tin sau · *Include the following*:

- **Cái gì · *What***: Mô tả ngắn gọn thay đổi · *Brief description of changes*
- **Tại sao · *Why***: Lý do thay đổi · *Reason for changes*
- **Cách · *How***: Chi tiết kỹ thuật · *Technical details of implementation*
- **Testing**: Cách đã test · *How changes were tested*
- **Screenshots**: Nếu có thay đổi UI · *If applicable*

---

## Tài liệu · *Documentation*

### Cấu trúc tài liệu · *Documentation Structure*

```
claude-code-architecture/
├── README.md                        ← Tổng quan dự án · Project overview
├── claude-code-architecture.md      ← Kiến trúc chi tiết · Detailed architecture
├── architecture-visual.html         ← Trực quan hóa · Visual HTML
├── architecture.md                  ← Hướng dẫn build · Build guide
├── structủe-readme.md               ← README thư viện · Library README
└── .github/
    ├── CONTRIBUTING.md              ← Bạn đang đây · You are here
    ├── SECURITY.md                  ← Chính sách bảo mật · Security policy
    └── CODE_OF_CONDUCT.md           ← Quy tắc ứng xử · Code of conduct
```

### Quy tắc viết tài liệu · *Documentation Rules*

- Tất cả tài liệu phải **song ngữ Việt/Anh** · *All documentation must be **bilingual Vietnamese/English***
- Sử dụng Mermaid cho diagram · *Use Mermaid for diagrams*
- Giữ các heading nhất quán · *Keep headings consistent*

---

## Nhận Trợ giúp · *Getting Help*

- **Câu hỏi · *Questions***: Mở GitHub Discussion · *Open a GitHub Discussion*
- **Lỗi · *Bugs***: Mở GitHub Issue · *Open a GitHub Issue*
- **Bảo mật · *Security***: Email security@techspherex.com

---

## Ghi nhận Đóng góp · *Recognition*

Người đóng góp sẽ được ghi nhận tại · *Contributors will be recognized in*:

- Trang GitHub contributors · *GitHub contributors page*
- CHANGELOG.md
- Release notes

---

## Giấy phép · *License*

Khi đóng góp, bạn đồng ý rằng đóng góp của bạn sẽ được cấp phép theo cùng giấy phép với dự án (MIT License).  
*By contributing, you agree that your contributions will be licensed under the same license as the project (MIT License).*

---

Cảm ơn bạn đã đóng góp! 🎉  
*Thank you for contributing!* 🎉