# Quy Trình Kiểm Tra Trước Khi Push Code

## Checklist Tổng Quan

- [ ] Code đã được format theo đúng convention
- [ ] Tất cả tests đã pass
- [ ] Không có conflicts với branch chính
- [ ] Commit messages tuân thủ format
- [ ] Code đã được review (nếu cần)
- [ ] Documentation đã được cập nhật

## 1. Kiểm Tra Code Style

```bash
# Kiểm tra code style
composer check-style

# Nếu có lỗi, chạy lệnh fix tự động
composer fix-style
```

### Các Lỗi Thường Gặp
- Sai format theo PSR-12
- Thiếu hoặc thừa khoảng trắng
- Sai convention đặt tên
- Import không được sử dụng
- Code phức tạp hoặc khó maintain

## 2. Chạy Tests

```bash
# Chạy toàn bộ tests
php artisan test

# Chạy tests với coverage report
php artisan test --coverage

# Chạy tests cho một file cụ thể
php artisan test tests/Feature/ExampleTest.php
```

### Yêu Cầu Coverage
- Unit Tests: >= 80%
- Feature Tests: >= 70%
- Integration Tests: >= 60%

## 3. Kiểm Tra Git Status

```bash
# Xem status hiện tại
git status

# Update branch chính
git checkout main
git pull origin main

# Rebase branch của bạn
git checkout your-branch
git rebase main

# Kiểm tra conflicts
git diff
```

### Xử Lý Conflicts
1. Giải quyết conflicts trong code
2. Đảm bảo ứng dụng vẫn chạy sau khi resolve
3. Chạy lại tests sau khi resolve
4. Add và commit các thay đổi

## 4. Chuẩn Bị Commit

### Format Commit Message
```
type(scope): description

[optional body]

[optional footer]
```

### Types:
- `feat`: Tính năng mới
- `fix`: Sửa lỗi
- `docs`: Thay đổi documentation
- `style`: Format, thiếu dấu chấm phẩy, etc.
- `refactor`: Refactor code
- `test`: Thêm tests
- `chore`: Cập nhật công cụ, libraries, etc.

### Examples:
```
feat(auth): implement JWT authentication

- Add JWT package
- Configure JWT middleware
- Add login and refresh endpoints

Closes #123
```

## 5. Code Review

### Self Review
- [ ] Code có dễ đọc và maintain?
- [ ] Có comments cho code phức tạp?
- [ ] Có xử lý đầy đủ các edge cases?
- [ ] Performance có được tối ưu?
- [ ] Security có được đảm bảo?

### Peer Review (nếu cần)
1. Tạo pull request
2. Assign reviewers
3. Respond to feedback
4. Update code theo suggestions
5. Request re-review

## 6. Documentation

### Cập Nhật Docs
- README.md
- API documentation
- Database changes
- Environment variables mới
- Dependencies mới
- Breaking changes

### Generate API Docs
```bash
# Nếu sử dụng Laravel Swagger
php artisan l5-swagger:generate
```

## 7. Final Checks

### Performance
- [ ] N+1 queries đã được xử lý
- [ ] Indexes được tạo cho queries phức tạp
- [ ] Caching được implement khi cần thiết

### Security
- [ ] Input validation đầy đủ
- [ ] SQL injection prevention
- [ ] XSS prevention
- [ ] CSRF protection
- [ ] Proper authentication/authorization

### Best Practices
- [ ] SOLID principles
- [ ] DRY (Don't Repeat Yourself)
- [ ] KISS (Keep It Simple, Stupid)
- [ ] YAGNI (You Aren't Gonna Need It)

## 8. Push Code

```bash
# Kiểm tra một lần cuối
git status
git diff

# Push code
git push origin your-branch
```

## Xử Lý Khi Có Lỗi

### Nếu Tests Fail
1. Check log để xác định nguyên nhân
2. Fix lỗi
3. Chạy lại toàn bộ test suite
4. Commit fixes với message rõ ràng

### Nếu Code Style Fail
1. Chạy `composer fix-style`
2. Review các thay đổi
3. Commit changes
4. Chạy lại check

### Nếu Build Fail
1. Check CI/CD logs
2. Fix issues
3. Push fixes
4. Đợi build mới hoàn thành

## Resources

### Documentation
- [Laravel Documentation](https://laravel.com/docs)
- [PSR-12 Standard](https://www.php-fig.org/psr/psr-12/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)

### Tools
- PHP_CodeSniffer
- PHP Mess Detector
- PHPStan/Larastan
- Laravel Pint

### IDE Plugins
- PHP Intelephense (VS Code)
- PHP CS Fixer
- PHP Debug
- Laravel IDE Helper

## Support

Nếu bạn gặp vấn đề:
1. Check documentation
2. Hỏi team lead
3. Tạo issue trong project board
4. Update documentation nếu cần thiết
