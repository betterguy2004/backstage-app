---
description: Cập nhật thông tin tổng quan dự án trong project-info.md
---

# Workflow: Cập nhật Project Information

Workflow này hướng dẫn cách cập nhật và duy trì thông tin tổng quan của dự án trong file `project-info.md`.

## Khi nào cần cập nhật

Cập nhật `project-info.md` khi:
- Thêm/sửa/xóa component chính trong hệ thống
- Thay đổi workflow hoặc luồng xử lý
- Cập nhật kiến trúc hoặc sơ đồ hệ thống
- Thay đổi công nghệ/stack
- Thêm tính năng mới quan trọng
- Cập nhật mục tiêu hoặc giá trị đạt được

## Cấu trúc file project-info.md

File phải bao gồm các phần sau:

1. **Tổng quan Workflow Dự án**: Mô tả ngắn gọn về dự án và nguồn tham khảo
2. **Mục tiêu hệ thống**: Liệt kê các mục tiêu chính
3. **Kiến trúc tổng thể**: Sơ đồ ASCII art về luồng hệ thống
4. **Workflow chi tiết**: Mô tả từng bước trong quy trình
5. **Các thành phần chính**: Danh sách components và plugins
6. **Giá trị đạt được**: Lợi ích và giá trị của hệ thống
7. **Luồng tổng hợp**: Tóm tắt ngắn gọn
8. **Ứng dụng thực tế**: Use cases

## Steps để cập nhật

### 1. Xem nội dung hiện tại
```bash
cat project-info.md
```

### 2. Backup file hiện tại (optional nhưng khuyến nghị)
// turbo
```bash
cp project-info.md project-info.md.backup
```

### 3. Chỉnh sửa file
- Mở file `project-info.md` trong editor
- Cập nhật các phần cần thiết
- Đảm bảo format markdown đúng chuẩn
- Kiểm tra các sơ đồ ASCII art vẫn hiển thị chính xác

### 4. Review changes
```bash
git diff project-info.md
```

### 5. Commit changes
```bash
git add project-info.md
git commit -m "docs: update project overview information"
```

### 6. Push lên remote repository
```bash
git push origin main
```

## Best Practices

1. **Ngắn gọn nhưng đầy đủ**: Cung cấp đủ thông tin nhưng không quá chi tiết
2. **Sử dụng sơ đồ**: ASCII art hoặc mermaid diagrams giúp dễ hiểu
3. **Cập nhật thường xuyên**: Đừng để thông tin lỗi thời
4. **Tiếng Việt rõ ràng**: Sử dụng thuật ngữ chuyên ngành phù hợp
5. **Liên kết nguồn**: Link đến tài liệu chi tiết hơn nếu cần
6. **Version control**: Luôn commit changes để theo dõi lịch sử

## Template mẫu cho sections mới

### Thêm component mới:
```markdown
### [Tên Component]
- Feature 1
- Feature 2
- Feature 3
```

### Thêm workflow step:
```markdown
### Bước X: [Tên bước]

[Mô tả chi tiết]

- Action 1
- Action 2
- Output expected
```

## Checklist trước khi commit

- [ ] Kiểm tra tất cả links có hoạt động
- [ ] Sơ đồ ASCII art hiển thị đúng
- [ ] Không có lỗi chính tả
- [ ] Thông tin kỹ thuật chính xác
- [ ] Format markdown đúng chuẩn
- [ ] Đã review bởi ít nhất một người khác (nếu có thể)
