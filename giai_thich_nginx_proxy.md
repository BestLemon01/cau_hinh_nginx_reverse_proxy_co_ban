# CẤU HÌNH NGINX REVERSE PROXY CƠ BẢN

## 1. Ý nghĩa của chỉ thị `proxy_pass`
- `proxy_pass http://localhost:3000;` chỉ định cho Nginx đóng vai trò **Reverse Proxy**: khi nhận được HTTP request từ client ở cổng 80, Nginx không tự phục vụ file tĩnh mà chuyển tiếp (forward) toàn bộ request đó tới ứng dụng nội bộ đang lắng nghe ở `localhost:3000` (React Frontend), sau đó nhận response trả về cho client.
- Người dùng chỉ cần truy cập `http://domain.com` (mặc định Port 80) thay vì phải gõ `:3000`, giúp che giấu kiến trúc cổng nội bộ và thuận tiện cho việc nâng cấp, bảo mật.

## 2. Xử lý tình huống bẫy: Khi ứng dụng React ở port 3000 bị sập
- Nếu ứng dụng React ở `localhost:3000` bị sập (stop/crash), Nginx vẫn chạy bình thường ở Port 80 nhưng không thể thiết lập kết nối TCP tới upstream `127.0.0.1:3000`.
- Khi đó, Nginx sẽ trả về cho client mã lỗi **`502 Bad Gateway`**.
