---
title: "Nền tảng SportShop E-Commerce"
description: "Kiến trúc Ba tầng AWS An toàn và Có khả năng Mở rộng cho Bán lẻ Thể thao Trực tuyến"
date: 2025-11-09
---

# Nền tảng SportShop E-Commerce

## Kiến trúc Ba tầng AWS An toàn và Có khả năng Mở rộng cho Bán lẻ Thể thao Trực tuyến

---

### Tóm tắt Điều hành

Nền tảng thương mại điện tử SportShop được xây dựng nhằm hiện đại hóa và tối ưu hóa hoạt động bán lẻ trực tuyến cho các sản phẩm thể thao. Hệ thống áp dụng kiến trúc ba tầng AWS (Edge → Application → Data) sử dụng ReactJS, Spring Boot và Amazon RDS MySQL. Nền tảng hỗ trợ thao tác CRUD sản phẩm, xác thực người dùng qua Amazon Cognito với OTP (email và SMS), tích hợp thanh toán VNPay (sandbox) và trò chuyện thời gian thực.

Bằng cách tận dụng các dịch vụ AWS được quản lý như Elastic Beanstalk, RDS, S3, CloudFront và Cognito kết hợp với quy trình CI/CD tự động (GitLab → CodePipeline → CodeBuild → Elastic Beanstalk), hệ thống đạt được độ sẵn sàng cao, bảo mật mạnh và chi phí vận hành thấp. Kiến trúc này bảo đảm triển khai liên tục, quản lý đơn giản và khả năng mở rộng trong tương lai.

---

### Phát biểu Vấn đề

**Vấn đề:**

Triển khai thủ công chậm, thiếu nhất quán và không an toàn. Thông tin bí mật thường lưu ở dạng văn bản thuần, dễ rò rỉ. Hệ thống chưa có giám sát tập trung, chưa có rollback tự động và thiếu pipeline CI/CD.

**Giải pháp:**
SportShop tận dụng các dịch vụ **AWS managed services** để tự động hóa hạ tầng, đảm bảo bảo mật và khả năng mở rộng.

Hệ thống được chia thành ba tầng:

- **Edge:** Route 53, CloudFront (với ACM), Cognito, SES, SNS
- **Application:** Elastic Beanstalk và Application Load Balancer
- **Data:** RDS MySQL (Single-AZ), S3, CloudWatch

Các thông tin nhạy cảm (cơ sở dữ liệu, khóa VNPay) được lưu an toàn trong **AWS Parameter Store.**

Quy trình CI/CD sử dụng **GitLab → CodePipeline → CodeBuild** để triển khai liên tục lên Elastic Beanstalk.

**Lợi ích:** Tự động hóa CI/CD giúp rút ngắn mạnh thời gian triển khai, HTTPS toàn tuyến tăng cường bảo mật, và thiết kế được tối ưu chi phí cho thử nghiệm và trình diễn thực tế.

---

### Kiến trúc Giải pháp

Nền tảng SportShop áp dụng **three-tier AWS cloud architecture** (Edge – Application – Data) được thiết kế để đảm bảo bảo mật, khả năng mở rộng và tối ưu chi phí cho hệ thống thương mại điện tử.

Nội dung web tĩnh được phân phối toàn cầu thông qua CloudFront và S3, trong khi backend được triển khai trên Elastic Beanstalk có cân bằng tải và tự động mở rộng. Cơ sở dữ liệu RDS MySQL lưu trữ dữ liệu tin cậy, và Cognito quản lý xác thực người dùng bằng mã OTP. Kiến trúc tuân thủ các nguyên tắc chuẩn AWS về bảo mật, sẵn sàng và hiệu quả vận hành. (Hình sơ đồ kiến trúc minh họa bên dưới.)

![AWS Architecture Diagram](/images/diagram.jpg)

#### Các dịch vụ AWS được sử dụng

- **Amazon Route 53**: Quản lý DNS và định tuyến lưu lượng đến CloudFront.
- **Amazon CloudFront**: Phân phối giao diện ReactJS và nội dung tĩnh qua HTTPS toàn cầu.
- **AWS Certificate Manager (ACM)**: Cấp và quản lý chứng chỉ SSL/TLS cho CloudFront để mã hóa HTTPS toàn tuyến.
- **Amazon Cognito**: Quản lý xác thực, đăng ký và token người dùng; gửi OTP qua SES và SNS.
- **Amazon SES**: Gửi OTP và email xác minh tài khoản.
- **Amazon SNS**: Gửi thông báo SMS và cảnh báo hệ thống.
- **AWS Elastic Beanstalk**: Lưu trữ backend Spring Boot (REST + WebSocket) với khả năng tự mở rộng và quản lý môi trường.
- **Application Load Balancer (ALB)**: Định tuyến HTTPS (cổng 443) từ CloudFront đến các instance EB.
- **AWS Parameter Store**: Lưu trữ thông tin nhạy cảm (DB, khóa VNPay) được mã hóa bằng KMS.
- **NAT Gateway**: Cho phép EB truy cập Internet an toàn (gọi VNPay API, tải thư viện).
- **Amazon RDS MySQL (Single-AZ)**: Lưu người dùng, sản phẩm, đơn hàng và tin nhắn chat.
- **Amazon S3**: Lưu build frontend, hình ảnh và bản sao lưu (mã hóa, có version).
- **VPC Gateway Endpoint cho S3**: Cho phép EB truy cập S3 nội bộ mà không cần Internet công cộng.
- **Amazon CloudWatch**: Thu thập log và metric từ EB, ALB, RDS để giám sát và cảnh báo.
- **Amazon SNS**: Gửi cảnh báo CloudWatch qua email hoặc SMS.
- **AWS CodePipeline và CodeBuild**: Tự động hóa CI/CD, tích hợp với GitLab để triển khai liên tục lên Elastic Beanstalk.

**Thiết kế thành phần:**

- **Tầng Edge (Phân phối & Xác thực):** Route 53 quản lý tên miền và định tuyến đến CloudFront; CloudFront cache và phân phối giao diện ReactJS qua HTTPS (ACM). Cognito xử lý đăng nhập người dùng, SES và SNS gửi mã OTP qua email và SMS.

- **Tầng Ứng dụng (Logic và API):** Elastic Beanstalk chạy backend Spring Boot, ALB phân phối lưu lượng HTTPS giữa các instance. Hệ thống tự mở rộng, truy cập VNPay và Cognito JWKS an toàn qua NAT Gateway. Các bí mật và cấu hình ứng dụng được lấy từ Parameter Store.

- **Tầng Dữ liệu (Lưu trữ và Giám sát):** RDS MySQL quản lý dữ liệu quan hệ. S3 lưu giao diện, hình ảnh và sao lưu, truy cập nội bộ qua VPC Gateway Endpoint. CloudWatch giám sát EB, ALB, RDS và gửi cảnh báo qua SNS để xử lý kịp thời.

---

### Triển khai Kỹ thuật

**Công nghệ sử dụng:**

- **Frontend:** ReactJS (S3 + CloudFront, HTTPS bằng ACM)
- **Backend:** Spring Boot (Java 17) trên Elastic Beanstalk
- **CSDL:** RDS MySQL (Single-AZ)
- **CI/CD:** GitLab → CodePipeline → CodeBuild
- **Bảo mật:** ACM, IAM Roles, Parameter Store (KMS), CloudWatch Alerts

**Quy trình triển khai gồm bốn giai đoạn chính:**

1. **Thiết kế Kiến trúc:** Xác định subnet, routing, security group và IAM.
2. **Tối ưu Chi phí:** Sử dụng Free Tier với RDS Single-AZ và một NAT Gateway để giảm chi phí.
3. **Triển khai:** Triển khai backend trên Elastic Beanstalk và frontend trên S3 + CloudFront. Cấu hình ACM, Route 53, Cognito, SES và SNS.
4. **Giám sát & CI/CD:** Kết nối GitLab với CodePipeline và CodeBuild để build tự động, bật CloudWatch + SNS để giám sát hoạt động.

---

### Lộ trình & Ngân sách

**Lộ trình Dự án:**

- **Trước triển khai**
  - **Tháng 0:** Xem xét hệ thống và chuẩn bị kiến trúc AWS
- **Triển khai**
  - **Tháng 1:** Thiết lập VPC, ALB, RDS, S3, CloudFront, Route 53 và pipeline CI/CD.
  - **Tháng 2:** Tích hợp Cognito, SES, SNS và VNPay sandbox; triển khai frontend và backend.
  - **Tháng 3:** Thực hiện kiểm thử, giám sát hiệu năng và triển khai chính thức.
    Sau triển khai: Tiếp tục tối ưu, giám sát và nghiên cứu khả năng mở rộng.

---

### Ước tính Ngân sách

Bạn có thể tìm thấy ước tính ngân sách trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?utm_source=chatgpt.com&id=1195281cec33775de9efa2b10f02e4927fa72063) hoặc tải xuống ở đây [Budget Estimation PDF](/files/SportShop-Budget-Estimation.pdf).

**Chi phí cơ sở hạ tầng Dịch vụ AWS**

- **Amazon RDS for MySQL**: $21.84/tháng (db.t2.micro, 20 GB storage)
- **Amazon EC2 (Elastic Beanstalk)**: $11.68/tháng (t3.micro, 20 GB EBS)
- **S3 Standard**: $0.26/tháng (10 GB, typical requests)
- **Data Transfer (Internet)**: $0.60/tháng (1 GB inbound, 5 GB outbound)
- **Amazon CloudFront**: $0.88/tháng (10 GB, 10k HTTPS requests)
- **Amazon Route 53**: $0.90/tháng (1 hosted zone)
- **AWS Certificate Manager (ACM)**: $0.00/tháng (free public cert; $15 one-time)
- **Amazon Cognito**: $0.00/tháng (≈500 MAU)
- **Amazon Simple Email Service (SES)**: $0.10/tháng (≈1,000 emails)
- **Amazon Simple Notification Service (SNS – topics)**: $0.00/tháng
- **Amazon CloudWatch (metrics & APIs)**: $3.02/tháng (≈10 metrics, basic APIs)
- **Network Address Translation (NAT Gateway)**: $43.36/tháng (1 gateway, 720 h)
- **NAT Data Transfer**: $1.30/tháng (≈10 GB outbound, 5 GB intra-region)
- **Application Load Balancer (ALB)**: $19.57/tháng (HTTPS routing, ~0.2 LCU)
- **AWS CodePipeline**: $0.00/tháng (1 pipeline, Free Tier)
- **AWS CodeBuild**: $0.50/tháng (≈10 builds × 10 min, general1.small)
- **AWS Systems Manager Parameter Store**: $0.00/tháng (≈10 standard parameters)
- **Amazon CloudWatch (logs)**: $1.41/tháng (≈2 GB ingested, 1-month retention)
- **AWS Key Management Service (KMS)**: $1.00/tháng (1 CMK, optional)

## **Tổng: $106.42/tháng, $1,292.04/12 tháng, $15.00 trả trước.**

### Đánh giá Rủi ro & Giảm thiểu

- **Ma trận rủi ro**

  - **Sự cố mạng hoặc instance**: Ảnh hưởng trung bình, xác suất thấp
  - **Downtime hoặc mất dữ liệu cơ sở dữ liệu**: Ảnh hưởng cao, xác suất thấp
  - **Lỗi pipeline hoặc build**: Ảnh hưởng trung bình, xác suất trung bình
  - **Tăng chi phí do NAT Gateway**: Ảnh hưởng trung bình, xác suất trung bình

- **Chiến lược giảm thiểu**

  - **Mạng/Instance**: Dùng Elastic Beanstalk rolling update và health check để tự phục hồi
  - **Cơ sở dữ liệu**: Bật sao lưu tự động và snapshot RDS để khôi phục nhanh khi xảy ra sự cố
  - **Pipeline**: Giám sát CodePipeline và CodeBuild qua CloudWatch, gửi cảnh báo qua SNS
  - **Chi phí**: Cấu hình cảnh báo AWS Budget và áp dụng lifecycle rule cho S3 để giảm chi phí lưu trữ

- **Kế hoạch dự phòng**
  - **Nếu Elastic Beanstalk gặp lỗi**: Triển khai lại bằng phiên bản ứng dụng trước đó trong CodePipeline
  - **Nếu RDS ngừng hoạt động**: Khôi phục từ snapshot gần nhất
  - **Khi chi phí NAT tăng**: Tạm tắt NAT Gateway và dùng truy cập outbound thủ công trong môi trường test

---

### Kết quả Mong đợi

**Cải tiến Kỹ thuật:**

- CI/CD tự động giúp giảm thời gian triển khai từ hàng giờ xuống vài phút.
- HTTPS toàn tuyến bằng ACM tăng cường bảo mật dữ liệu và độ tin cậy người dùng.
- Giám sát tập trung qua CloudWatch giúp phát hiện sự cố sớm.
- Kiến trúc hỗ trợ mở rộng tự động và kiểm soát chi phí hiệu quả.

**Giá trị Dài hạn:**

- Cung cấp mô hình AWS mẫu có thể tái sử dụng cho các hệ thống thương mại điện tử tương lai.
- Tạo nền tảng chi phí thấp, an toàn, dễ mở rộng cho startup và dự án học thuật.
- Giúp sinh viên tích lũy kinh nghiệm thực tế với công cụ AWS và thiết kế ứng dụng phân tán.
