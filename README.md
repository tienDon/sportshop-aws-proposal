# SportShop E-Commerce Proposal - Hugo Site

🌐 **Live Website**: https://tienDon.github.io/sportshop-aws-proposal/

Đây là website Hugo để hiển thị proposal nền tảng SportShop E-Commerce với hỗ trợ đa ngôn ngữ (Tiếng Anh và Tiếng Việt).

## 🚀 Tính năng

- ✅ **Đa ngôn ngữ**: Hỗ trợ tiếng Anh và tiếng Việt với nút chuyển đổi dễ sử dụng
- ✅ **Responsive Design**: Tối ưu cho desktop, tablet và mobile
- ✅ **Print-Optimized**: CSS được tối ưu cho việc in hoặc xuất PDF
- ✅ **Clean Layout**: Navbar với logo "Proposal" và nút chuyển ngôn ngữ
- ✅ **One-Page Proposal**: Tất cả nội dung được hiển thị trên một trang dễ đọc

## 📁 Cấu trúc Project

```
hugo/
├── content/
│   ├── _index.en.md        # Trang chủ tiếng Anh
│   ├── _index.vi.md        # Trang chủ tiếng Việt
│   ├── proposal.en.md      # Proposal tiếng Anh
│   └── proposal.vi.md      # Proposal tiếng Việt
├── layouts/
│   ├── _default/
│   │   ├── baseof.html     # Layout chính
│   │   └── single.html     # Layout cho trang đơn
│   └── partials/
│       ├── head.html       # Meta tags và CSS
│       ├── header.html     # Navbar với language switcher
│       └── footer.html     # Footer
├── assets/css/
│   └── custom.css          # CSS tùy chỉnh cho print và responsive
├── themes/PaperMod/        # Hugo theme PaperMod
└── hugo.toml              # Cấu hình Hugo với multilingual
```

## 🛠️ Cài đặt & Chạy

### Yêu cầu

- Hugo Extended version (>= 0.120.0)
- Git

### Cài đặt dependencies

```powershell
# Di chuyển đến thư mục project
cd "d:\aws-learning\hugo"

# Cài đặt Git submodules (theme)
git submodule update --init --recursive
```

### Chạy Development Server

```powershell
# Chạy Hugo server
hugo server --bind 0.0.0.0 --port 1313

# Hoặc với buildDrafts nếu có content draft
hugo server --buildDrafts --bind 0.0.0.0
```

Website sẽ chạy tại: http://localhost:1313

### Build cho Production

```powershell
# Build static files
hugo --minify

# Files sẽ được tạo trong thư mục public/
```

## 🌐 Sử dụng

### Chuyển đổi ngôn ngữ

- Trang mặc định: Tiếng Anh (http://localhost:1313/)
- Trang tiếng Việt: http://localhost:1313/vi/
- Nút chuyển ngôn ngữ ở góc phải navbar

### Xem Proposal

- Tiếng Anh: http://localhost:1313/proposal/
- Tiếng Việt: http://localhost:1313/vi/proposal/

## 📄 In và Xuất PDF

### Cách 1: Print từ Browser

1. Mở trang proposal cần in
2. Nhấn `Ctrl+P` (Windows) hoặc `Cmd+P` (Mac)
3. Chọn cài đặt:
   - **Destination**: Save as PDF
   - **Layout**: Portrait
   - **Paper size**: A4
   - **Margins**: Minimum
   - **Options**: Bỏ tích "Headers and footers", "Background graphics"
4. Nhấn "Save" hoặc "Print"

### Cách 2: Browser Developer Tools

```javascript
// Mở Developer Console (F12) và chạy lệnh sau để tối ưu print:
document.body.classList.add("print-mode");
window.print();
```

### Cách 3: Sử dụng Hugo Build

```powershell
# Build và serve static files
hugo --minify
cd public
python -m http.server 8000

# Sau đó truy cập http://localhost:8000 để in
```

### Tips cho Print PDF tốt nhất

- Sử dụng trang proposal trực tiếp (không qua homepage)
- Đảm bảo navbar bị ẩn khi print (đã được CSS xử lý)
- Font size được tối ưu tự động cho A4
- Margins được set tự động
- Page breaks được xử lý tự động

## 🎨 Tùy chỉnh

### Thêm CSS tùy chỉnh

Chỉnh sửa file: `assets/css/custom.css`

### Thay đổi nội dung

- **English content**: Chỉnh sửa `content/proposal.en.md`
- **Vietnamese content**: Chỉnh sửa `content/proposal.vi.md`

### Cấu hình site

Chỉnh sửa file: `hugo.toml`

```toml
# Thay đổi title, description, v.v.
[languages.en]
  title = "Your Custom Title"

[languages.vi]
  title = "Tiêu đề Tùy chỉnh"
```

### Thêm ngôn ngữ mới

1. Thêm language config trong `hugo.toml`:

```toml
[languages.fr]
  languageName = "Français"
  weight = 3
  title = "SportShop E-Commerce Platform"
```

2. Tạo file content: `content/proposal.fr.md`
3. Cập nhật navbar trong `layouts/partials/header.html`

## 🔧 Troubleshooting

### Hugo Server không chạy

```powershell
# Kiểm tra Hugo version
hugo version

# Nếu chưa cài, download từ: https://github.com/gohugoio/hugo/releases
# Đảm bảo sử dụng Extended version
```

### Git submodule errors

```powershell
# Reset submodules
git submodule deinit --all -f
git submodule update --init --recursive

# Hoặc clone lại theme manually
rm -rf themes/PaperMod
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

### CSS không load

```powershell
# Clear Hugo cache
hugo --cleanDestinationDir
```

### Language switching không hoạt động

Kiểm tra:

1. File content có đúng suffix (.en.md, .vi.md)
2. Cấu hình language trong hugo.toml
3. URL path đúng format

## 📞 Hỗ trợ

Nếu gặp vấn đề:

1. Kiểm tra Hugo version (cần Extended >= 0.120.0)
2. Đảm bảo tất cả files có quyền read/write
3. Kiểm tra terminal output để debug lỗi
4. Xem Hugo documentation: https://gohugo.io/documentation/

## 📈 Phát triển tiếp

Các tính năng có thể thêm:

- 🔍 Search functionality
- 📊 Analytics integration
- 🎨 Theme customization
- 📱 PWA support
- 🌙 Dark/Light mode toggle
- 📧 Contact form
- 📁 Download PDF button
