# Chính sách Bảo mật · *Security Policy*

## Phiên bản được Hỗ trợ · *Supported Versions*

| Phiên bản · *Version* | Trạng thái · *Status* |
|---|---|
| 1.x.x (latest) | ✅ Được hỗ trợ · *Supported* |
| < 1.0.0 | ❌ Không hỗ trợ · *Not supported* |

---

## Báo cáo Lỗ hổng Bảo mật · *Reporting a Vulnerability*

Chúng tôi rất coi trọng bảo mật của **claude-code-architecture**. Nếu bạn phát hiện lỗ hổng bảo mật, vui lòng báo cáo theo hướng dẫn bên dưới.  
*We take the security of **claude-code-architecture** seriously. If you believe you have found a security vulnerability, please report it as described below.*

### Xin đừng · *Please Do Not*

- Mở issue công khai trên GitHub cho lỗ hổng bảo mật · *Open a public GitHub issue for security vulnerabilities*
- Tiết lộ lỗ hổng công khai trước khi được xử lý · *Disclose the vulnerability publicly before it has been addressed*

### Xin hãy · *Please Do*

1. **Gửi email · *Email us*** tại security@techspherex.com với:  
   *at security@techspherex.com with:*
   - Mô tả lỗ hổng · *Description of the vulnerability*
   - Các bước tái hiện · *Steps to reproduce*
   - Tác động tiềm ẩn · *Potential impact*
   - Đề xuất sửa (nếu có) · *Suggested fix (if any)*

2. **Cho chúng tôi thời gian** xử lý trước khi công bố · *Allow us time to respond and fix the issue before public disclosure*

3. **Mã hóa thông tin nhạy cảm** bằng PGP key (liên hệ để nhận) · *Encrypt sensitive information using our PGP key (available on request)*

### Quy trình Phản hồi · *What to Expect*

| Giai đoạn · *Stage* | Thời gian · *Timeline* |
|---|---|
| Xác nhận đã nhận · *Acknowledgment* | Trong 48 giờ · *Within 48 hours* |
| Đánh giá ban đầu · *Initial Assessment* | Trong 7 ngày · *Within 7 days* |
| Cập nhật định kỳ · *Regular Updates* | Mỗi 7 ngày · *Every 7 days until resolved* |
| Sửa lỗi · *Fix Timeline* | Lỗi nghiêm trọng trong 30 ngày · *Critical issues within 30 days* |

### Bảo vệ An toàn · *Safe Harbor*

Chúng tôi ủng hộ việc tiết lộ có trách nhiệm. Chúng tôi sẽ không truy cứu pháp lý đối với các nhà nghiên cứu:  
*We support responsible disclosure. We will not pursue legal action against researchers who:*

- Nỗ lực thiện chí để tránh vi phạm quyền riêng tư và phá hủy dữ liệu · *Make a good faith effort to avoid privacy violations and data destruction*
- Chỉ tương tác với tài khoản test của chính họ · *Only interact with test accounts they own or with explicit permission*
- Không khai thác lỗ hổng quá mức cần thiết · *Do not exploit the vulnerability beyond demonstrating it*
- Báo cáo lỗ hổng kịp thời · *Report the vulnerability promptly*
- Giữ bí mật lỗ hổng cho đến khi được sửa · *Keep the vulnerability confidential until it is fixed*

---

## Thực hành Bảo mật Tốt nhất · *Security Best Practices*

### Dành cho Người dùng · *For Users*

#### 1. Cập nhật Phần mềm · *Keep Software Updated*
- Luôn sử dụng phiên bản mới nhất · *Always use the latest version*
- Theo dõi các thông báo bảo mật · *Monitor security advisories*
- Cập nhật dependencies thường xuyên · *Update dependencies regularly*

#### 2. Cấu hình An toàn · *Secure Configuration*
- Sử dụng credentials mạnh và duy nhất · *Use strong, unique credentials*
- Bật rate limiting · *Enable rate limiting*
- Cấu hình quyền truy cập phù hợp · *Configure proper access controls*
- Sử dụng biến môi trường cho secrets · *Use environment variables for secrets*

#### 3. Bảo mật Mạng · *Network Security*
- Sử dụng HTTPS cho tất cả giao tiếp · *Use HTTPS for all communications*
- Cấu hình firewall rules phù hợp · *Implement proper firewall rules*
- Cô lập trong phân đoạn mạng an toàn · *Isolate in secure network segments*

#### 4. Giám sát · *Monitoring*
- Bật audit logging · *Enable audit logging*
- Giám sát hoạt động đáng ngờ · *Monitor for suspicious activity*
- Kiểm tra logs định kỳ · *Review logs regularly*

### Dành cho Nhà phát triển · *For Developers*

#### 1. Bảo mật Code · *Code Security*
- Tuân thủ secure coding practices · *Follow secure coding practices*
- Sử dụng type hints và validation · *Use type hints and validation*
- Sanitize tất cả inputs · *Sanitize all inputs*
- Tránh hardcode secrets · *Avoid hardcoded secrets*

#### 2. Dependencies
- Cập nhật dependencies thường xuyên · *Regularly update dependencies*
- Sử dụng `npm audit` / `safety` kiểm tra lỗ hổng · *Use `npm audit` / `safety` to check for vulnerabilities*
- Pin phiên bản dependency · *Pin dependency versions*

#### 3. Testing
- Bao gồm security tests · *Include security tests*
- Test authentication và authorization · *Test authentication and authorization*
- Test input validation · *Test input validation*
- Sử dụng static analysis tools · *Use static analysis tools*

#### 4. Code Review
- Yêu cầu review cho tất cả thay đổi · *Require reviews for all changes*
- Review bảo mật cho code nhạy cảm · *Security-focused reviews for sensitive code*
- Quét bảo mật tự động trong CI/CD · *Automated security scanning in CI/CD*

---

## Các Cân nhắc Bảo mật · *Known Security Considerations*

### Xác thực Đầu vào · *Input Validation*

Tất cả input được xác thực để ngăn chặn · *All user inputs are validated to prevent*:
- Command injection
- Path traversal
- SQL injection (nếu áp dụng · *if applicable*)
- XSS attacks

### Giới hạn Tốc độ · *Rate Limiting*

Rate limiting được áp dụng để ngăn chặn · *Rate limiting is enforced to prevent*:
- Tấn công từ chối dịch vụ · *Denial of service attacks*
- Brute force attacks
- Cạn kiệt tài nguyên · *Resource exhaustion*

### Xác thực & Ủy quyền · *Authentication & Authorization*

- API keys phải được lưu trữ an toàn · *API keys should be stored securely*
- Credentials không bao giờ được log · *Credentials should never be logged*
- Áp dụng nguyên tắc quyền tối thiểu · *Use principle of least privilege*

### Quản lý Secrets · *Secrets Management*

- Không bao giờ commit secrets vào version control · *Never commit secrets to version control*
- Sử dụng biến môi trường hoặc secret managers · *Use environment variables or secret managers*
- Xoay vòng credentials thường xuyên · *Rotate credentials regularly*

---

## Tính năng Bảo mật · *Security Features*

### Bảo mật Tích hợp · *Built-in Security*

| Tính năng · *Feature* | Mô tả · *Description* |
|---|---|
| Input validation | Xác thực và sanitize đầu vào · *Input validation and sanitization* |
| Rate limiting | Giới hạn tần suất yêu cầu · *Request rate limiting* |
| Audit logging | Ghi log kiểm toán · *Audit trail logging* |
| Secure defaults | Cấu hình mặc định an toàn · *Secure default configuration* |
| Least privilege | Thực thi với quyền tối thiểu · *Least privilege execution* |

### Security Headers (khi triển khai · *when deploying*)

- Bật HTTPS · *Enable HTTPS*
- Thiết lập security headers · *Set security headers*
- Cấu hình CORS đúng cách · *Implement CORS properly*
- Sử dụng secure cookies · *Use secure cookies*

---

## Bảo mật Dependencies · *Dependency Security*

Chúng tôi sử dụng các công cụ tự động để giám sát dependencies:  
*We use automated tools to monitor dependencies:*

| Công cụ · *Tool* | Mục đích · *Purpose* |
|---|---|
| **Dependabot** | Cập nhật dependency tự động · *Automated dependency updates* |
| **npm audit** | Quét lỗ hổng npm packages · *npm package vulnerability scanning* |
| **Safety** | Quét lỗ hổng Python packages · *Python package vulnerability scanning* |
| **Bandit** | Security linting cho Python · *Security linting for Python* |

---

## Nhật ký Kiểm toán Bảo mật · *Security Audit Log*

Các sự kiện liên quan đến bảo mật được ghi log:  
*Security-relevant events are logged:*

- Các lần thử xác thực · *Authentication attempts*
- Lỗi ủy quyền · *Authorization failures*
- Thay đổi cấu hình · *Configuration changes*
- Hoạt động đáng ngờ · *Suspicious activities*
- Truy cập tài nguyên · *Resource access*

---

## Tuân thủ · *Compliance*

Dự án tuân thủ · *This project follows*:

- Hướng dẫn OWASP Top 10 · *OWASP Top 10 guidelines*
- Giảm thiểu CWE Top 25 · *CWE Top 25 mitigation*
- Vòng đời phát triển an toàn · *Secure development lifecycle*
- Đánh giá bảo mật định kỳ · *Regular security assessments*

---

## Liên hệ · *Contact*

| Kênh · *Channel* | Chi tiết · *Details* |
|---|---|
| **Email bảo mật · *Security Email*** | security@techspherex.com |
| **Vấn đề chung · *General Issues*** | GitHub Issues |
| **PGP Key** | Liên hệ để nhận · *Available on request* |

---

## Ghi nhận · *Acknowledgments*

Chúng tôi cảm ơn các nhà nghiên cứu bảo mật đã tiết lộ có trách nhiệm.  
*We thank the following security researchers for responsible disclosure.*

<!-- Danh sách sẽ được cập nhật · List will be updated -->

---

## Cập nhật · *Updates*

Chính sách bảo mật này được xem xét và cập nhật hàng quý.  
*This security policy is reviewed and updated quarterly.*

**Cập nhật lần cuối · *Last update***: 2026-05-12

---

## Tài liệu Bổ sung · *Additional Resources*

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25](https://cwe.mitre.org/top25/archive/2023/2023_top25_list.html)
- [Claude Code Security — Permission Gate](../claude-code-architecture.md#①-lớp-đầu-vào--kiểm-soát--input--permission-layer)
- [Hướng dẫn Đóng góp · *Contributing Guide*](CONTRIBUTING.md)
