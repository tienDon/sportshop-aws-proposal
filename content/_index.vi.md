---
title: "Nền tảng SportShop E-Commerce"
description: "Kiến trúc Ba tầng AWS An toàn và Có khả năng Mở rộng cho Bán lẻ Thể thao Trực tuyến"
date: 2025-11-09
---

# Nền tảng SportShop E-Commerce

## Kiến trúc Ba tầng AWS An toàn và Có khả năng Mở rộng cho Bán lẻ Thể thao Trực tuyến

---

### Tóm tắt Điều hành

Nền tảng thương mại điện tử SportShop được xây dựng nhằm hiện đại hóa và tối ưu hóa hoạt động bán lẻ trực tuyến cho các sản phẩm thể thao. Hệ thống áp dụng kiến trúc ba tầng AWS (Edge → Application → Data) sử dụng ReactJS, Spring Boot và Amazon RDS MySQL.

Nền tảng hỗ trợ:

- Thao tác CRUD sản phẩm
- Xác thực người dùng qua Amazon Cognito với OTP (email và SMS)
- Tích hợp thanh toán VNPay (sandbox)
- Trò chuyện thời gian thực

Bằng cách tận dụng các dịch vụ AWS được quản lý như Elastic Beanstalk, RDS, S3, CloudFront và Cognito kết hợp với quy trình CI/CD tự động (GitLab → CodePipeline → CodeBuild → Elastic Beanstalk), hệ thống đạt được **độ sẵn sàng cao, bảo mật mạnh và chi phí vận hành thấp**. Kiến trúc này bảo đảm triển khai liên tục, quản lý đơn giản và khả năng mở rộng trong tương lai.

---

### Phát biểu Vấn đề

**Vấn đề:**

- Triển khai thủ công chậm, thiếu nhất quán và không an toàn
- Thông tin bí mật thường lưu ở dạng văn bản thuần, dễ rò rỉ
- Hệ thống chưa có giám sát tập trung, chưa có rollback tự động và thiếu pipeline CI/CD

**Giải pháp:**
SportShop tận dụng các dịch vụ AWS được quản lý để tự động hóa hạ tầng, đảm bảo bảo mật và khả năng mở rộng.

**Các tầng Kiến trúc:**

- **Edge:** Route 53, CloudFront (với ACM), Cognito, SES, SNS
- **Application:** Elastic Beanstalk và Application Load Balancer
- **Data:** RDS MySQL (Single-AZ), S3, CloudWatch

**Bảo mật:** Các thông tin nhạy cảm (cơ sở dữ liệu, khóa VNPay) được lưu an toàn trong AWS Parameter Store.

**CI/CD:** GitLab → CodePipeline → CodeBuild để triển khai liên tục lên Elastic Beanstalk.

**Lợi ích:** Tự động hóa CI/CD giúp rút ngắn mạnh thời gian triển khai, HTTPS toàn tuyến tăng cường bảo mật, và thiết kế được tối ưu chi phí cho thử nghiệm và trình diễn thực tế.

---

### Kiến trúc Giải pháp

Nền tảng SportShop áp dụng **kiến trúc đám mây ba tầng của AWS** (Edge – Application – Data) được thiết kế để đảm bảo bảo mật, khả năng mở rộng và tối ưu chi phí cho hệ thống thương mại điện tử.

**Thành phần Chính:**

- **Nội dung web tĩnh** được phân phối toàn cầu thông qua CloudFront và S3
- **Logic backend** được triển khai trên Elastic Beanstalk có cân bằng tải và tự động mở rộng
- **RDS MySQL** lưu trữ dữ liệu tin cậy
- **Cognito** quản lý xác thực người dùng bằng mã OTP

#### Các dịch vụ AWS được sử dụng

| Dịch vụ                       | Mục đích                               |
| ----------------------------- | -------------------------------------- |
| Amazon Route 53               | Quản lý DNS và định tuyến lưu lượng    |
| Amazon CloudFront             | Phân phối nội dung toàn cầu với HTTPS  |
| AWS Certificate Manager (ACM) | Quản lý chứng chỉ SSL/TLS              |
| Amazon Cognito                | Xác thực và quản lý token người dùng   |
| Amazon SES/SNS                | Gửi OTP qua email và SMS               |
| AWS Elastic Beanstalk         | Lưu trữ backend Spring Boot            |
| Application Load Balancer     | Phân phối lưu lượng HTTPS              |
| AWS Parameter Store           | Lưu trữ thông tin nhạy cảm được mã hóa |
| Amazon RDS MySQL              | Lưu trữ dữ liệu ứng dụng               |
| Amazon S3                     | Lưu file tĩnh và sao lưu               |
| Amazon CloudWatch             | Giám sát và cảnh báo                   |

---

### Triển khai Kỹ thuật

**Công nghệ sử dụng:**

- **Frontend:** ReactJS (S3 + CloudFront, HTTPS bằng ACM)
- **Backend:** Spring Boot (Java 17) trên Elastic Beanstalk
- **CSDL:** RDS MySQL (Single-AZ)
- **CI/CD:** GitLab → CodePipeline → CodeBuild
- **Bảo mật:** ACM, IAM Roles, Parameter Store (KMS), CloudWatch Alerts

**Các giai đoạn Triển khai:**

1. **Thiết kế Kiến trúc:** Xác định subnet, routing, security group và IAM
2. **Tối ưu Chi phí:** Sử dụng Free Tier với RDS Single-AZ và một NAT Gateway
3. **Triển khai:** Triển khai backend trên Elastic Beanstalk và frontend trên S3 + CloudFront
4. **Giám sát & CI/CD:** Kết nối GitLab với CodePipeline và bật CloudWatch alerts

---

### Lộ trình & Ngân sách

**Lộ trình Dự án:**

- **Tháng 0:** Xem xét hệ thống và chuẩn bị kiến trúc AWS
- **Tháng 1:** Thiết lập VPC, ALB, RDS, S3, CloudFront, Route 53 và pipeline CI/CD
- **Tháng 2:** Tích hợp Cognito, SES, SNS và VNPay sandbox; triển khai frontend và backend
- **Tháng 3:** Kiểm thử, giám sát hiệu năng và triển khai chính thức

**Ước tính Ngân sách:** **$106.42/tháng** (~$1,292/năm + $15 phí một lần)

**Thành phần Chi phí Chính:**

- NAT Gateway: $43.36/tháng (chi phí lớn nhất)
- ALB: $19.57/tháng
- RDS MySQL: $21.84/tháng
- EC2 (Elastic Beanstalk): $11.68/tháng
- Các dịch vụ khác: <$10/tháng tổng cộng

---

### Đánh giá Rủi ro & Giảm thiểu

| Rủi ro         | Ảnh hưởng  | Xác suất   | Giảm thiểu                    |
| -------------- | ---------- | ---------- | ----------------------------- |
| Sự cố Instance | Trung bình | Thấp       | Tự phục hồi Elastic Beanstalk |
| Downtime CSDL  | Cao        | Thấp       | Sao lưu tự động RDS           |
| Lỗi Pipeline   | Trung bình | Trung bình | Giám sát CloudWatch + SNS     |
| Vượt Chi phí   | Trung bình | Trung bình | Cảnh báo AWS Budget           |

**Kế hoạch Dự phòng:**

- Triển khai lại bằng phiên bản trước đó trong CodePipeline
- Khôi phục từ snapshot RDS gần nhất
- Tạm tắt NAT Gateway để kiểm soát chi phí

---

### Kết quả Mong đợi

**Cải tiến Kỹ thuật:**

- ✅ CI/CD tự động giúp giảm thời gian triển khai từ hàng giờ xuống vài phút
- ✅ HTTPS toàn tuyến bằng ACM tăng cường bảo mật dữ liệu và độ tin cậy người dùng
- ✅ Giám sát tập trung qua CloudWatch giúp phát hiện sự cố sớm
- ✅ Kiến trúc hỗ trợ mở rộng tự động và kiểm soát chi phí hiệu quả

**Giá trị Dài hạn:**

- 🎯 Cung cấp mô hình AWS mẫu có thể tái sử dụng cho các hệ thống thương mại điện tử tương lai
- 🎯 Tạo nền tảng chi phí thấp, an toàn, dễ mở rộng cho startup và dự án học thuật
- 🎯 Giúp sinh viên tích lũy kinh nghiệm thực tế với công cụ AWS và thiết kế ứng dụng phân tán

---

_Đề xuất này cung cấp lộ trình hoàn chỉnh để triển khai một nền tảng thương mại điện tử hiện đại, an toàn và có khả năng mở rộng sử dụng các nguyên tắc chuẩn AWS và giải pháp hiệu quả về chi phí._
