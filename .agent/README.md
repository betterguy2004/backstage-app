# .agent Directory

Thư mục này chứa các quy tắc (rules), workflows và cấu hình cho AI Agent và team development.

## Cấu trúc

```
.agent/
├── rules.md                    # Quy tắc chung cho dự án
├── workflows/                  # Các workflows/hướng dẫn thực hiện
│   └── update-project-info.md  # Workflow cập nhật project-info.md
└── README.md                   # File này
```

## Mục đích

### rules.md
Định nghĩa các quy tắc bắt buộc cho team development, bao gồm:
- Quy định về documentation
- Git commit conventions  
- Review process
- Best practices

**Tất cả members phải tuân thủ các rules này.**

### workflows/
Chứa các hướng dẫn chi tiết (step-by-step) để thực hiện các tác vụ phổ biến.

Mỗi workflow file theo format:
```markdown
---
description: Mô tả ngắn gọn
---

# Nội dung workflow
...
```

## Workflows hiện có

### `/update-project-info` 
Hướng dẫn cập nhật file `project-info.md` - tài liệu tổng quan dự án.

**Khi nào sử dụng**:
- Thêm/sửa component trong hệ thống
- Thay đổi workflow hoặc kiến trúc
- Cập nhật stack công nghệ
- Thêm tính năng mới quan trọng

**Cách dùng**: Đọc file `workflows/update-project-info.md` và follow các steps

## Cách sử dụng

### Cho Developers
1. Đọc `rules.md` để hiểu các quy tắc dự án
2. Tham khảo workflows khi cần thực hiện tasks
3. Đề xuất thêm rules/workflows mới nếu cần qua PR

### Cho AI Agents
AI Agent sẽ tự động:
- Tuân thủ các rules trong `rules.md`
- Sử dụng workflows khi được yêu cầu
- Đề xuất cập nhật documentation khi detect changes

### Cho Tech Leads
- Review và approve changes cho files trong `.agent/`
- Đảm bảo rules được enforce
- Cập nhật rules khi cần thiết

## Thêm workflow mới

Để thêm workflow mới:

1. Tạo file trong `workflows/` với tên mô tả rõ ràng (dùng kebab-case)
2. Follow template format với YAML frontmatter
3. Viết bằng tiếng Việt, rõ ràng, có ví dụ
4. Thêm annotation `// turbo` hoặc `// turbo-all` nếu cần auto-run
5. Cập nhật README này để list workflow mới
6. Tạo PR để review

## Best Practices

1. **Keep it simple**: Rules và workflows phải dễ hiểu, dễ follow
2. **Actionable**: Mỗi rule/workflow phải có action items rõ ràng
3. **Living document**: Thường xuyên review và cập nhật
4. **Vietnamese first**: Viết bằng tiếng Việt để team dễ hiểu
5. **Examples**: Luôn có ví dụ cụ thể

## Maintenance

- **Owner**: Tech Lead
- **Review cycle**: Quarterly hoặc khi có major changes
- **Update trigger**: Team feedback, process changes, tool updates

---

**Lưu ý**: Thư mục này là phần quan trọng của project governance. Mọi changes phải qua review process.
