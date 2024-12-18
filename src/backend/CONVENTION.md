# Laravel Coding Convention

## Table of Contents
1. [General PHP Rules](#general-php-rules)
2. [Laravel Specific Rules](#laravel-specific-rules)
3. [Directory Structure](#directory-structure)
4. [Naming Conventions](#naming-conventions)
5. [Database](#database)
6. [API Development](#api-development)
7. [Security](#security)
8. [Code Style Checking](#code-style-checking)

## General PHP Rules

### Code Style
- Follow PSR-12 coding standard
- Use soft tabs (4 spaces) for indentation
- Maximum line length: 120 characters
- Add single space after control structure keywords
```php
// Good
if ($condition) {
    // code
}

// Bad
if($condition){
    // code
}
```

### Type Declarations
- Use type declarations where possible
```php
public function calculateTotal(float $price, int $quantity): float
{
    return $price * $quantity;
}
```

### Naming
- Use camelCase for variables and methods
- Use PascalCase for classes
- Use UPPER_CASE for constants
```php
class UserController
{
    private const MAX_ATTEMPTS = 5;
    
    private string $firstName;
    
    public function getUserProfile(): array
    {
        // code
    }
}
```

## Laravel Specific Rules

### Controllers
- Use singular PascalCase
- Suffix with 'Controller'
- Keep controllers focused and lean
```php
// Good
class UserController extends Controller
{
    public function index()
    {
        return User::paginate();
    }
    
    public function store(StoreUserRequest $request)
    {
        // validation is handled in StoreUserRequest
        return User::create($request->validated());
    }
}
```

### Models
- Use singular PascalCase
- Define relationships
- Use model observers for complex operations
```php
class User extends Model
{
    protected $fillable = [
        'name',
        'email',
        'password',
    ];
    
    protected $hidden = [
        'password',
        'remember_token',
    ];
    
    public function posts(): HasMany
    {
        return $this->hasMany(Post::class);
    }
}
```

### Services
- Use for complex business logic
- Keep controllers thin, services fat
```php
class PaymentService
{
    public function processPayment(Order $order): bool
    {
        // Complex payment logic here
    }
}
```

## Directory Structure

```
app/
├── Console/
│   └── Commands/                # Custom artisan commands
├── Exceptions/                  # Custom exception handlers
├── Http/
│   ├── Controllers/            # Request handlers
│   │   ├── Api/               # API controllers
│   │   └── Web/               # Web controllers
│   ├── Middleware/            # Request middleware
│   └── Requests/              # Form requests
├── Models/                     # Eloquent models
├── Services/                   # Business logic services
├── Repositories/              # Data access layer
└── Providers/                 # Service providers

database/
├── factories/                 # Model factories
├── migrations/               # Database migrations
└── seeders/                 # Database seeders

tests/
├── Feature/                 # Feature tests
└── Unit/                   # Unit tests
```

## Naming Conventions

### Files and Directories
- Controllers: `UserController.php`
- Models: `User.php`
- Migrations: `2024_01_01_000000_create_users_table.php`
- Views: `user/index.blade.php`

### Database
- Tables: plural, snake_case (`users`, `blog_posts`)
- Pivot tables: singular model names in alphabetical order (`role_user`)
- Columns: snake_case, no table name prefix (`first_name`)
- Foreign keys: singular model name with _id suffix (`user_id`)

## API Development

### Response Format
```php
return response()->json([
    'success' => true,
    'data' => $data,
    'message' => 'Operation successful',
    'errors' => []
], $statusCode);
```

### API Versioning
- Use URL versioning: `/api/v1/users`
- Create separate controllers for each version

### Request Validation
- Use Form Request classes for validation
```php
class StoreUserRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:8'
        ];
    }
}
```

## Security

### Authentication
- Use Laravel Sanctum for API authentication
- Use Laravel Breeze/Jetstream for web authentication
- Never store sensitive data in .env files in production

### Authorization
- Use Policies for authorization logic
- Use Gates for simple authorization
```php
class PostPolicy
{
    public function update(User $user, Post $post): bool
    {
        return $user->id === $post->user_id;
    }
}
```

### Data Protection
- Always validate input data
- Use prepared statements (Laravel does this automatically)
- Escape output data
- Use HTTPS in production
- Set proper CORS headers

## Code Style Checking

### Available Tools

1. **PHP_CodeSniffer (PHPCS)**
   - Kiểm tra code theo chuẩn PSR-12
   - Tự động sửa lỗi với PHPCBF
   ```bash
   # Kiểm tra code style
   ./vendor/bin/phpcs --standard=PSR12 app tests
   
   # Tự động sửa lỗi
   ./vendor/bin/phpcbf --standard=PSR12 app tests
   ```

2. **PHP Mess Detector (PHPMD)**
   - Phát hiện code smells và vấn đề tiềm ẩn
   - Kiểm tra: complexity, unused code, naming, etc.
   ```bash
   ./vendor/bin/phpmd app,tests text cleancode,codesize,controversial,design,naming,unusedcode
   ```

3. **PHPStan/Larastan**
   - Phân tích static code
   - Phát hiện lỗi tiềm ẩn và type-related issues
   ```bash
   ./vendor/bin/phpstan analyse
   ```

4. **Laravel Pint**
   - Official Laravel code style fixer
   - Dựa trên PHP-CS-Fixer
   ```bash
   ./vendor/bin/pint
   ```

### Composer Scripts

Chúng tôi đã cấu hình sẵn các composer scripts để dễ dàng kiểm tra code:

```bash
# Kiểm tra toàn bộ code style
composer check-style

# Tự động sửa code style
composer fix-style
```

### IDE Integration

1. **PHPStorm**
   - Vào `Settings > PHP > Quality Tools`
   - Cấu hình đường dẫn tới các công cụ trong thư mục vendor
   - Enable "Run on Save" để tự động format khi lưu file

2. **Visual Studio Code**
   - Cài đặt extensions:
     - PHP Intelephense
     - PHP CS Fixer
     - PHP Debug
   - Thêm settings vào `.vscode/settings.json`:
   ```json
   {
       "php.suggest.basic": false,
       "[php]": {
           "editor.formatOnSave": true,
           "editor.defaultFormatter": "bmewburn.vscode-intelephense-client"
       }
   }
   ```

### Git Integration

1. **Pre-commit Hook**
   - Tự động kiểm tra code trước khi commit
   - File: `.git/hooks/pre-commit`
   ```bash
   #!/bin/sh
   
   echo "Running code style check..."
   composer check-style
   
   if [ $? -ne 0 ]; then
       echo "Code style check failed. Please fix the errors before committing."
       exit 1
   fi
   ```

2. **CI/CD Pipeline**
   ```yaml
   # .github/workflows/code-style.yml
   name: Code Style
   
   on: [push, pull_request]
   
   jobs:
     phpcs:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - name: Setup PHP
           uses: shivammathur/setup-php@v2
         - name: Install Dependencies
           run: composer install
         - name: Check Code Style
           run: composer check-style
   ```

### Customizing Rules

1. **PHPStan/Larastan (`phpstan.neon`)**
   ```yaml
   parameters:
       level: 5
       paths:
           - app
           - tests
       checkMissingIterableValueType: false
   ```

2. **Laravel Pint (`pint.json`)**
   ```json
   {
       "preset": "psr12",
       "rules": {
           "array_syntax": {
               "syntax": "short"
           },
           "ordered_imports": {
               "sort_algorithm": "alpha"
           },
           "no_unused_imports": true
       }
   }
   ```

3. **PHPMD (`phpmd.xml`)**
   ```xml
   <?xml version="1.0"?>
   <ruleset name="Custom PHPMD Rules">
       <rule ref="rulesets/cleancode.xml">
           <exclude name="StaticAccess"/>
       </rule>
       <rule ref="rulesets/codesize.xml"/>
       <rule ref="rulesets/naming.xml">
           <exclude name="ShortVariable"/>
       </rule>
   </ruleset>
   ```

### Best Practices

1. **Kiểm tra code thường xuyên**
   - Chạy `composer check-style` trước khi commit
   - Cấu hình IDE để tự động format khi save
   - Review code style trong pull requests

2. **Xử lý lỗi**
   - Ưu tiên sửa lỗi theo mức độ nghiêm trọng
   - Tạo issue cho các lỗi cần thảo luận
   - Document các exceptions khi cần thiết

3. **Team Workflow**
   - Thống nhất coding standards trong team
   - Code review tập trung vào logic thay vì style
   - Sử dụng automated tools để giảm manual review

4. **Monitoring & Reporting**
   - Theo dõi số lượng lỗi qua thời gian
   - Tạo báo cáo code quality định kỳ
   - Cập nhật rules dựa trên feedback

## Testing

### Unit Tests
- Test individual components
- Use descriptive test method names
```php
public function test_user_can_be_created_with_valid_data(): void
{
    // test code
}
```

### Feature Tests
- Test complete features
- Test API endpoints
```php
public function test_api_can_create_user(): void
{
    $response = $this->postJson('/api/users', [
        'name' => 'John Doe',
        'email' => 'john@example.com'
    ]);
    
    $response->assertStatus(201);
}
```

## Git Workflow

### Branches
- main/master: production code
- develop: development code
- feature/*: new features
- bugfix/*: bug fixes
- hotfix/*: urgent production fixes

### Commit Messages
Format:
```
type(scope): description

[optional body]

[optional footer]
```

Types:
- feat: new feature
- fix: bug fix
- docs: documentation changes
- style: formatting, missing semi colons, etc
- refactor: code change that neither fixes a bug nor adds a feature
- test: adding tests
- chore: maintain

Example:
```
feat(auth): implement JWT authentication

- Add JWT package
- Configure JWT middleware
- Add login and refresh endpoints

Closes #123
```

## Code Review Guidelines

### What to Look For
1. Code Style
   - Follows PSR-12
   - Proper indentation
   - Meaningful variable names

2. Security
   - Input validation
   - SQL injection prevention
   - XSS prevention
   - CSRF protection

3. Performance
   - N+1 query prevention
   - Proper indexing
   - Cache usage where appropriate

4. Testing
   - Unit tests present
   - Feature tests for API endpoints
   - Test coverage adequate

### Review Process
1. Read the description and requirements
2. Check out the branch locally
3. Run tests
4. Review code changes
5. Provide constructive feedback
6. Approve or request changes
