Dự án: Triển khai IPv6 trên AWS với Cấu hình Dual-stack
1. Giới thiệu
Dự án này trình bày cách triển khai một web server đơn giản trên nền tảng AWS, sử dụng cấu hình mạng dual-stack để hỗ trợ cả địa chỉ IPv4 và IPv6. Mục tiêu là tạo ra một trang web có thể truy cập công khai thông qua cả hai giao thức mạng.

2. Kiến trúc giải pháp
VPC (Virtual Private Cloud): Một VPC được tạo với hỗ trợ dải địa chỉ IPv6.

Subnet: Một subnet công khai được tạo, cũng hỗ trợ dải địa chỉ IPv6.

Internet Gateway: Được sử dụng để cho phép lưu lượng truy cập từ Internet đến VPC của bạn.

Route Table: Bảng định tuyến được cấu hình để cho phép lưu lượng truy cập IPv4 (0.0.0.0/0) và IPv6 (::/0) đi ra ngoài qua Internet Gateway.

EC2 Instance: Một instance Amazon Linux được khởi chạy trong subnet, được gán cả địa chỉ IPv4 và IPv6 công khai.

Security Group: Nhóm bảo mật được cấu hình để cho phép các kết nối SSH (cổng 22) và HTTP (cổng 80) từ mọi địa chỉ IP (0.0.0.0/0 cho IPv4 và ::/0 cho IPv6).

Web Server: Web server Apache được cài đặt và chạy trên instance.

3. Các bước triển khai
3.1. Cấu hình mạng trên AWS
Cấu hình VPC và Subnet: Đã tạo VPC và một subnet công khai, liên kết một dải CIDR IPv6 với cả hai.

Cấu hình Route Table: Đã thêm một route IPv6 với đích ::/0 và target là Internet Gateway.

3.2. Khởi chạy và cấu hình EC2 Instance
Khởi chạy Instance: Một instance EC2 đã được khởi chạy với cấu hình dual-stack, nhận được địa chỉ IPv4 là 44.201.70.35 và địa chỉ IPv6 là 2600:1ff0:4ff6:a:8e00:c02b:389d:d654:3e97.

Cấu hình Security Group: Đã thêm các Inbound Rules để cho phép SSH và HTTP cho cả hai giao thức.

Cài đặt Web Server: Kết nối đến instance qua SSH và chạy các lệnh sau để cài đặt Apache:

Bash

sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
4. Kết quả và kiểm tra
Sau khi hoàn thành các bước trên, trang web của bạn có thể truy cập được thông qua:

Địa chỉ IPv4: http://44.201.70.35/

Địa chỉ IPv6: http://[2600:1ff0:4ff6:a:8e00:c02b:389d:d654:3e97]/

Chúng tôi đã xác nhận rằng địa chỉ IPv6 là hợp lệ và được liên kết với AWS tại Virginia.

5. Đẩy code lên GitHub
Tải file index.html về máy tính cục bộ.

Khởi tạo repository Git cục bộ và liên kết với repository GitHub.

Thực hiện các lệnh sau để đẩy code lên GitHub:

Bash

git init
git add .
git commit -m "Initial commit for IPv6 web project"
git remote add origin https://github.com/MinhQuan2043/workshop-ipv6.git
git push -u origin main
