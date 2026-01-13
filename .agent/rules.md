# Project Rules & Guidelines

## Project Documentation Rules

### Rule 1: Duy trì file project-info.md

**Mục đích**: Đảm bảo thông tin tổng quan về dự án luôn được cập nhật và chính xác.

**Quy định**:

1. **File bắt buộc**: File `project-info.md` ở root directory là tài liệu chính thức về tổng quan dự án
2. **Cập nhật kịp thời**: Mọi thay đổi lớn về kiến trúc, workflow, hoặc components phải được phản ánh trong file này trong vòng 24h
3. **Người chịu trách nhiệm**: 
   - Tech Lead: Review và approve changes
   - Developers: Đề xuất cập nhật khi có thay đổi
   - DevOps: Cập nhật phần infrastructure và deployment

**Nội dung bắt buộc phải có**:

- [ ] Tổng quan về dự án (mục tiêu, scope)
- [ ] Kiến trúc hệ thống (diagrams)
- [ ] Workflow chi tiết (các bước xử lý)
- [ ] Danh sách components/services chính
- [ ] Stack công nghệ sử dụng
- [ ] Giá trị và lợi ích của hệ thống
- [ ] Use cases thực tế

**Format yêu cầu**:
- Markdown format chuẩn
- Sử dụng tiếng Việt
- ASCII diagrams cho các luồng
- Có references/links khi cần thiết

**Workflow cập nhật**: Sử dụng `/update-project-info` workflow

---

### Rule 2: Git Commit Convention cho project-info.md

Khi commit changes cho `project-info.md`, sử dụng format:

```
docs(project-info): <mô tả ngắn gọn>

<mô tả chi tiết nếu cần>
```

**Ví dụ**:
```
docs(project-info): add new ArgoCD sync workflow

- Added detailed steps for ArgoCD automatic sync
- Updated architecture diagram with new component
```

---

### Rule 3: Review Process

**Đối với changes lớn** (thay đổi kiến trúc, workflow chính):
1. Tạo Pull Request
2. Tag Tech Lead để review
3. Cần ít nhất 1 approval
4. Merge và notify team qua Slack/Teams

**Đối với changes nhỏ** (typo, formatting, minor updates):
- Có thể commit trực tiếp lên main branch
- Nhưng vẫn phải follow commit convention

---

### Rule 4: Backup và Version Control

1. **Không xóa thông tin cũ**: Nếu cần cập nhật, hãy comment hoặc move sang section "Archived"
2. **Git history**: Dựa vào git history để track changes
3. **Backup định kỳ**: Tự động backup qua Git, không cần manual backup

---

## Development Workflow Rules

### Rule 5: Backstage Template Development

Khi tạo/sửa Backstage Software Templates:

1. Cập nhật `project-info.md` nếu template thêm workflow mới
2. Document trong TechDocs
3. Test template trước khi merge
4. Update catalog-info.yaml nếu cần

### Rule 6: Infrastructure Changes

Mọi thay đổi về infrastructure qua Crossplane phải:

1. Được document trong `project-info.md` nếu ảnh hưởng đến kiến trúc
2. Follow GitOps workflow (qua ArgoCD)
3. Test trong dev environment trước
4. Có rollback plan

---

## Compliance và Best Practices

### Rule 7: Documentation First

Trước khi implement thay đổi lớn:
1. Update design trong `project-info.md`
2. Review với team
3. Sau đó mới implement

### Rule 8: Single Source of Truth

- `project-info.md` là source of truth cho project overview
- Các tài liệu chi tiết khác (TechDocs, README) có thể reference đến file này
- Tránh duplicate information

---

## Automation Rules

### Rule 9: CI/CD Integration

- GitHub Actions có thể validate format của `project-info.md`
- Có thể tự động generate một số phần từ code (danh sách plugins, versions)
- Notify team khi có updates

### Rule 10: Backstage Integration

- File `project-info.md` có thể được hiển thị trong Backstage TechDocs
- Sử dụng metadata để track ownership
- Link từ Software Catalog đến documentation

---

## Enforcement

**Consequences khi không follow rules**:
- Warning đầu tiên
- Review code sẽ bị block cho đến khi follow rules
- Trong sprint retrospective sẽ review compliance

**Benefits khi follow rules**:
- Team có shared understanding
- Onboarding mới dễ dàng hơn
- Documentation luôn up-to-date
- Tăng quality và maintainability
