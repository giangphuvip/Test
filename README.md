# Hướng Dẫn Thiết Lập RDP Windows Trên GitHub Actions Qua Tailscale

Dự án này sử dụng GitHub Actions để tự động khởi tạo một máy ảo Windows Server 2022, cấu hình Remote Desktop Protocol (RDP) và kết nối nó vào mạng Tailscale của bạn.

## 📋 Yêu Cầu Chuẩn Bị
1. Tài khoản [GitHub](https://github.com/).
2. Tài khoản [Tailscale](https://tailscale.com/).
3. Ứng dụng **Remote Desktop Connection** (có sẵn trên Windows) hoặc ứng dụng RDP tương đương trên macOS/Linux/Điện thoại.

## ⚙️ Các Bước Thiết Lập

### Bước 1: Lấy Auth Key từ Tailscale
1. Đăng nhập vào trang quản trị [Tailscale Admin Console](https://login.tailscale.com/admin/machines).
2. Chuyển đến mục **Settings** > **Keys**.
3. Bấm **Generate auth key...** 
4. Trong cửa sổ tạo khóa, **đánh dấu tích vào "Ephemeral"** (điều này giúp máy ảo tự động xóa khỏi danh sách mạng lưới của bạn khi GitHub Actions kết thúc).
5. Nhấn **Generate key** và sao chép đoạn mã bắt đầu bằng `tskey-...`.

### Bước 2: Thêm Secret Vào Kho Lưu Trữ (Repository)
1. Mở kho lưu trữ GitHub của bạn.
2. Đi tới **Settings** > **Secrets and variables** > **Actions**.
3. Nhấn **New repository secret**.
4. Khai báo thông tin sau:
   - **Name:** `TAILSCALE_AUTH_KEY`
   - **Secret:** Dán đoạn mã Auth key bạn vừa copy ở Bước 1 vào đây.
5. Nhấn **Add secret**.

### Bước 3: Khởi Chạy Workflow
1. Chuyển sang tab **Actions** trên GitHub repository.
2. Bấm vào workflow **RDP** (hoặc tên workflow tương ứng) ở menu bên trái.
3. Nhấn **Run workflow** > **Run workflow** để bắt đầu tiến trình.
4. Đợi khoảng 1-2 phút cho đến khi workflow chạy tới bước **Maintain Connection**. Nhấp vào bước đó để xem log.

## 🚀 Cách Kết Nối
Khi workflow đang chạy và hiển thị trạng thái `RDP Active`, hãy xem log ở bước **Maintain Connection** để lấy địa chỉ IP của máy ảo (Tailscale IP).

Mở phần mềm Remote Desktop Connection và nhập các thông tin sau:
- **Computer / PC name:** `Địa chỉ IP Tailscale` (ví dụ: 100.x.x.x)
- **Username:** `kozodj`
- **Password:** `giangaphu2010@`

## ⚠️ Lưu Ý Quan Trọng
* **Giới Hạn Thời Gian:** Các runner do GitHub lưu trữ (GitHub-hosted runners) có giới hạn thời gian chạy tối đa là **6 tiếng (360 phút)** cho mỗi luồng công việc. Sau 6 tiếng, phiên làm việc sẽ tự động bị ngắt và toàn bộ dữ liệu chưa lưu trên máy ảo sẽ bị mất.
* **Chính Sách Của GitHub:** Việc sử dụng GitHub Actions chỉ để làm máy chủ cá nhân ảo (VPS) hoặc chạy các tác vụ không liên quan đến phát triển/kiểm thử phần mềm có thể vi phạm Điều khoản dịch vụ của GitHub. Tài khoản của bạn có thể bị khóa nếu lạm dụng hệ thống lưu trữ máy tính này trong thời gian dài liên tục.
