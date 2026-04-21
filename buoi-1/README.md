# Buổi 1 – Câu hỏi ôn tập

## 1. Private IP và Public IP khác nhau như thế nào?

| | Private IP | Public IP |
|---|---|---|
| **Phạm vi** | Chỉ dùng trong mạng nội bộ (LAN) | Dùng trên Internet toàn cầu |
| **Ai cấp phát** | Router/DHCP nội bộ | Nhà cung cấp dịch vụ Internet (ISP) |
| **Có thể truy cập từ Internet không?** | Không (cần NAT) | Có |
| **Ví dụ dải địa chỉ** | 192.168.x.x, 10.x.x.x, 172.16–31.x.x | Bất kỳ IP nào ngoài dải private |
| **Chi phí** | Miễn phí, dùng tùy ý trong LAN | Thường phải thuê/mua từ ISP |

**Tóm lại:** Private IP là địa chỉ "nội bộ" giúp các thiết bị trong cùng mạng giao tiếp với nhau; Public IP là địa chỉ "công khai" giúp mạng nội bộ kết nối ra Internet.

---

## 2. DNS có vai trò gì trong việc truy cập website?

DNS (Domain Name System) đóng vai trò như một **"cuốn danh bạ" của Internet**:

- Khi bạn gõ `https://google.com` vào trình duyệt, máy tính không biết địa chỉ IP của Google.
- Trình duyệt gửi truy vấn tới **DNS Resolver** (thường do ISP hoặc Google/Cloudflare cung cấp).
- DNS Resolver tìm và trả về địa chỉ IP tương ứng (ví dụ `142.250.x.x`).
- Trình duyệt dùng IP đó để kết nối tới máy chủ web và tải nội dung về.

**Không có DNS**, người dùng phải nhớ địa chỉ IP thay vì tên miền dễ nhớ.

---

## 3. Port 80 và Port 443 khác nhau chỗ nào?

| | Port 80 | Port 443 |
|---|---|---|
| **Giao thức** | HTTP (HyperText Transfer Protocol) | HTTPS (HTTP Secure) |
| **Mã hóa** | Không có – dữ liệu truyền dưới dạng văn bản thuần | Có – dữ liệu được mã hóa bằng TLS/SSL |
| **Bảo mật** | Dễ bị nghe lén (Man-in-the-Middle) | An toàn hơn, chống nghe lén |
| **Trình duyệt hiển thị** | "Not Secure" / không có ổ khóa | Ổ khóa xanh (🔒) |

**Tóm lại:** Port 80 là HTTP không mã hóa; Port 443 là HTTPS có mã hóa TLS, an toàn hơn cho người dùng.

---

## 4. Ngrok giải quyết vấn đề gì?

**Vấn đề:** Máy tính cá nhân của bạn nằm sau NAT/Firewall, không có Public IP cố định → người ngoài Internet không thể truy cập trực tiếp vào server đang chạy ở `localhost`.

**Ngrok giải quyết bằng cách:**

1. Tạo một **tunnel** (đường hầm) bảo mật từ máy tính của bạn đến các máy chủ của Ngrok trên Internet.
2. Cấp cho bạn một **URL công khai** (ví dụ `https://abc123.ngrok.io`).
3. Khi có request gửi tới URL đó, Ngrok chuyển tiếp về `localhost` của bạn.

**Ứng dụng thực tế:** Demo ứng dụng local cho khách hàng, test webhook từ dịch vụ bên ngoài (GitHub, Stripe…), phát triển ứng dụng mobile cần HTTPS.

---

## 5. Nginx với vai trò Reverse Proxy hoạt động ra sao?

**Reverse Proxy** là một server trung gian đứng trước các server ứng dụng thực sự (backend).

**Luồng hoạt động:**

```
Client (Browser)
      │
      │  request https://example.com
      ▼
  [ Nginx – Reverse Proxy ]
      │
      │  forward request tới backend
      ▼
  [ App Server: Node.js / Python / Java... ]
      │
      │  response
      ▼
  [ Nginx ]
      │
      │  trả response về cho client
      ▼
Client (Browser)
```

**Lợi ích khi dùng Nginx làm Reverse Proxy:**

- **Load Balancing:** Phân phối traffic đều cho nhiều backend server.
- **SSL Termination:** Nginx xử lý HTTPS, backend chỉ cần HTTP nội bộ.
- **Caching:** Lưu cache response để giảm tải backend.
- **Bảo mật:** Che giấu địa chỉ và cổng thực của server ứng dụng.
- **Gzip/Compression:** Nén dữ liệu trước khi gửi về client.

---

## 6. Tại sao website ngày nay cần HTTPS thay vì HTTP?

1. **Bảo mật dữ liệu:** HTTPS mã hóa toàn bộ dữ liệu trao đổi (thông tin đăng nhập, thẻ ngân hàng…), chống nghe lén và tấn công Man-in-the-Middle.

2. **Xác thực danh tính:** Chứng chỉ TLS/SSL xác nhận website đúng là trang bạn muốn truy cập, chống giả mạo (phishing).

3. **SEO và uy tín:** Google ưu tiên xếp hạng các trang HTTPS. Trình duyệt hiển thị cảnh báo "Not Secure" với trang HTTP.

4. **HTTP/2 và hiệu năng:** Hầu hết trình duyệt chỉ hỗ trợ HTTP/2 (nhanh hơn HTTP/1.1) qua HTTPS.

5. **Yêu cầu của các API hiện đại:** Nhiều tính năng web (Geolocation, Service Worker, camera…) chỉ hoạt động trên HTTPS.

6. **Niềm tin của người dùng:** Người dùng ngày càng nhận thức được tầm quan trọng của ổ khóa xanh trước khi nhập thông tin cá nhân.
