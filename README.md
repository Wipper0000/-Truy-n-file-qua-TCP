
<h2 align="center">
    <a href="https://dainam.edu.vn/vi/khoa-cong-nghe-thong-tin">
    🎓 Faculty of Information Technology (DaiNam University)
    </a>
</h2>
<h2 align="center">
   TRUYỀN FILE QUA GIAO THỨC TCP
</h2>
<div align="center">
    <p align="center">
        <img src="docs/aiotlab_logo.png" alt="AIoTLab Logo" width="170"/>
        <img src="docs/fitdnu_logo.png" alt="AIoTLab Logo" width="180"/>
        <img src="docs/dnu_logo.png" alt="DaiNam University Logo" width="200"/>
    </p>

[![AIoTLab](https://img.shields.io/badge/AIoTLab-green?style=for-the-badge)](https://www.facebook.com/DNUAIoTLab)
[![Faculty of Information Technology](https://img.shields.io/badge/Faculty%20of%20Information%20Technology-blue?style=for-the-badge)](https://dainam.edu.vn/vi/khoa-cong-nghe-thong-tin)
[![DaiNam University](https://img.shields.io/badge/DaiNam%20University-orange?style=for-the-badge)](https://dainam.edu.vn)

</div>

## 📖 1. Giới thiệu
Trong thời đại công nghệ thông tin phát triển mạnh mẽ, nhu cầu trao đổi và chia sẻ dữ liệu giữa các máy tính ngày càng phổ biến. Một trong những phương thức quan trọng và được sử dụng rộng rãi là truyền file qua giao thức TCP (Transmission Control Protocol).

TCP là giao thức hướng kết nối, đảm bảo tính tin cậy, toàn vẹn và đúng thứ tự dữ liệu trong quá trình truyền. Nhờ đó, việc truyền file qua TCP có thể áp dụng cho nhiều hệ thống thực tế như: dịch vụ FTP, ứng dụng chat, hệ thống lưu trữ dữ liệu phân tán, hay các phần mềm hỗ trợ tải xuống.

🔑 **Các chức năng chính của hệ thống**:

-Kết nối Client – Server qua TCP Socket.

-Truyền và nhận file giữa các client.

-Hiển thị tiến trình truyền và nhận file .

-Lưu file tự động vào thư mục chỉ định.

## 🛠️ 2. Công nghệ sử dụng

**Ngôn ngữ lập trình**: Java

**Giao thức**: TCP,TCP Socket

**Công nghệ giao diện**: Java Swing

**Thư viện**:

-java.net 

-java.io 

**Công cụ phát triển**: Eclipse IDE

**Hệ điều hành**: Windows 10

**JDK**: Java SE


## 🚀 3. Hình ảnh các chức năng


<p align="center">
  <img src="docs/Server.png" alt="Ảnh 1" width="500"/>
</p>
<p align="center">
  <em>Hình 1: Giao diện Server  </em>
</p>


<p align="center">
  <img src="docs/ClientA.png" alt="" width="500"/>
</p>
<p align="center">
  <em>Hình 2: Giao diện Client A  </em>
</p><p align="center">


  <img src="docs/ClientB.png" alt="" width="500"/>
</p>
<p align="center">
  <em>Hình 3: Giao diện Client B  </em>
</p>

<p align="center">
  <img src="docs/Connect.png" alt="" width="500"/>
</p>
<p align="center">
  <em>Hình 4: Giao diện khi Server đã chạy và các Client đã kết nối  </em>
</p>

<p align="center">
  <img src="docs/ChoseFile.png" alt="" width="500"/>
</p>
<p align="center">
  <em>Hình 5: Giao diện Khi chọn File  </em>
</p>

<p align="center">
  <img src="docs/Received.png" alt="" width="500"/>
</p>

## 📝 4. Các bước cài đặt

#### Bước 1: Chuẩn bị môi trường
1. **Kiểm tra Java**: Mở terminal/command prompt và chạy:
   ```bash
   java -version
   javac -version
   ```
   Đảm bảo cả hai lệnh đều hiển thị phiên bản Java 8 trở lên.

2. **Tải mã nguồn**: Sao chép thư mục `Truyen-file-qua-tcp` chứa các file:
   - `Server.java`
   - `Client.java`

#### Bước 2: Biên dịch mã nguồn
1. **Mở terminal** và điều hướng đến thư mục chứa mã nguồn
2. **Biên dịch các file Java**:
   ```bash
   javac Truyen-file-qua-tcp/*.java
   ```
   Hoặc biên dịch từng file riêng lẻ:
   ```bash
   javac Truyen-file-qua-tcp/Server.java
   javac Truyen-file-qua-tcp/Client.java
   ```

3. **Kiểm tra kết quả**: Nếu biên dịch thành công, sẽ tạo ra các file `.class` tương ứng.

#### Bước 3: Chạy ứng dụng

**Khởi động Server:**
```bash
java Truyen-file-qua-tcp.Server
```
- Server sẽ khởi động trên ip (127.0.0.1) và port (4000) mặc định 
- Giao diện server sẽ hiển thị, sẵn sàng nhận kết nối từ client

**Khởi động Client:**
```bash
java Truyen-file-qua-tcp.Client
```
- Nhập địa chỉ trên 2 Client để kết nối với Server
- Client sẽ kết nối đến Server và log của Server sẽ hiện kết nối của từng Client

### 🚀 Sử dụng ứng dụng

1. **Kết nối**: Client kết nối đến Server sau khi nhập địa chỉ và nhấn kết nối
2. **Chọn file**: Mở và chọn file để gửi
3. **Gửi File**: Sau khi đã chọn file ta nhấn "Gửi File"
4. **Nhận File**: Khi đã nhận file client còn lại sẽ nhận được file và thông báo đã nhận được
5. **Lưu File**: Client sau khi nhận file sẽ lưu vào thu mục đã chỉ định sẵn
6. **Ngắt kết nối**: Đóng cửa sổ client hoặc nhấn Ctrl+C để ngắt kết nối

© 2025 AIoTLab, Faculty of Information Technology, DaiNam University. All rights reserved.

---
