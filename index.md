\# Workshop: Triển khai và Chuyển đổi IPv6 trên AWS

\---

\## 1. Giới thiệu về IPv6

\### 1.1 IPv6 là gì và tại sao cần chuyển đổi?

* IPv6 (Internet Protocol version 6) là thế hệ tiếp theo của giao thức Internet, được thiết kế để thay thế IPv4.
* Lý do chính cho sự chuyển đổi là do \*\*sự cạn kiệt địa chỉ IPv4\*\*. IPv4 chỉ có khoảng 4.3 tỷ địa chỉ, trong khi IPv6 cung cấp một số lượng địa chỉ khổng lồ: $3.4 \times 10^{38}$.

\### 1.2 So sánh IPv4 và IPv6

* \*\*Định dạng địa chỉ\*\*: IPv4 sử dụng 32-bit (ví dụ: `192.168.1.1`), còn IPv6 sử dụng 128-bit (ví dụ: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`).
* \*\*Bảo mật\*\*: IPsec là tùy chọn trên IPv4, nhưng là \*\*bắt buộc\*\* và được tích hợp sẵn trong giao thức IPv6, cung cấp mã hóa và xác thực đầu cuối.
* \*\*Cấu hình\*\*: IPv6 hỗ trợ \*\*SLAAC (Stateless Address Autoconfiguration)\*\*, cho phép các thiết bị tự cấu hình địa chỉ mà không cần máy chủ DHCPv6.

\---

\## 2. Chiến lược Triển khai và Di chuyển IPv6

\### 2.1 Các phương pháp di chuyển

* \*\*Dual-stack\*\*: Triển khai cả IPv4 và IPv6 cùng lúc. Đây là phương pháp phổ biến nhất trên đám mây.
* \*\*Tunneling\*\*: Đóng gói các gói tin IPv6 bên trong các gói tin IPv4 để truyền qua mạng IPv4.
* \*\*NAT64/DNS64\*\*: Cho phép các máy chủ chỉ hỗ trợ IPv6 giao tiếp với các máy chủ chỉ hỗ trợ IPv4.

\### 2.2 Chiến lược Dual-stack trên AWS

Chúng ta sẽ sử dụng chiến lược Dual-stack trên AWS để đảm bảo tính tương thích với các hệ thống cũ trong khi vẫn tận dụng được lợi ích của IPv6.

\---

\## 3. Thực hành trên AWS: Triển khai Dual-stack

Đây là phần hướng dẫn từng bước. Bạn sẽ thực hiện theo các bước này trên tài khoản AWS của mình.

\### 3.1 Bước 1: Tạo VPC và Subnet

* Vào giao diện quản lý AWS, tìm kiếm \*\*VPC\*\*.
* Nhấn \*\*Create VPC\*\*.
* \*\*Cấu hình CIDR IPv4\*\*: Chọn một dải CIDR như `10.0.0.0/16`.
* \*\*Cấu hình CIDR IPv6\*\*: AWS sẽ tự động cung cấp một dải địa chỉ IPv6 ($/56$) cho VPC của bạn.
* Tạo 2 Subnet trong VPC này, mỗi Subnet sẽ được tự động gán một dải địa chỉ IPv6 ($/64$).

\### 3.2 Bước 2: Cập nhật Bảng Định tuyến (Route Table)

* Tạo một \*\*Internet Gateway (IGW)\*\* và gắn nó vào VPC của bạn.
* Cập nhật bảng định tuyến chính của VPC:
* Thêm một route mới cho IPv4: `0.0.0.0/0` trỏ tới IGW.
* Thêm một route mới cho IPv6: `::/0` trỏ tới IGW.

\### 3.3 Bước 3: Cấu hình Security Group và EC2

* Tạo một Security Group cho phép truy cập HTTP và SSH từ cả IPv4 và IPv6.
* Khởi chạy một \*\*EC2 Instance\*\* (ví dụ: Amazon Linux 2 AMI) trong Subnet đã tạo.
* Đảm bảo `Auto-assign public IPv4 address` và `Auto-assign IPv6 address` đều được bật.

\---

\## 4. Kiểm tra Kết nối

* Sử dụng trình duyệt hoặc lệnh `curl -6 http://[địa chỉ IPv6 của EC2]` để kiểm tra truy cập.

\---
