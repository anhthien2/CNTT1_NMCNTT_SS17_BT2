# Phần 1: Báo cáo phân tích kiến trúc đa Profile
**1. Phân tích nguyên nhân sâu xa văng phiên đăng nhập - Session Conflict**

Khi dùng chung 1 Profile Chrome mặc định, tất cả dữ liệu được lưu chung trong 1 thư mục `User Data\Default`.

-   **Cơ chế Cookies:** Mỗi website CarePulse, PTIT, GitHub đều lưu `session_token` dưới dạng Cookie trong file `Cookies` duy nhất. Chrome chỉ có 1 `Cookie Jar` cho 1 Profile.
-   **Xung đột:** Khi thực tập sinh mở tab mới đăng nhập GitHub/PTIT bằng tài khoản khác trên cùng Profile, Google OAuth hoặc hệ thống SSO sẽ ghi đè lên Cookie `auth_token` cũ. Server CarePulse kiểm tra thấy Cookie không khớp phiên đang kê đơn -> tự động Logout để bảo vệ -> Bác sĩ bị văng giữa ca khám.
-   **Hậu quả khác:** History, Cache, LocalStorage dùng chung nên mới lộ Shopee, Facebook khi chiếu máy chiếu.

**2.Thiết kế Mô hình 3 Profile chuyên dụng**


| Profile | Tên đề xuất | Màu Theme | Mục đích & Dữ liệu độc lập |
| :--- | :--- | :--- | :--- |
| WORK | BS. Truc - CarePulse | Xanh Dương Đậm | Khám bệnh, kê đơn. Cookies CarePulse + BHYT riêng |
| PERSONAL | CaNhan - BS.Ten | Xám / Đen | Giải lao, Facebook, Shopee. Cookies cá nhân riêng |
| SANDBOX/TEST | ThucTap - TEST | Vàng / Cam | Cho SV thực tập, khách mượn. Profile trắng, xóa sau mỗi tuần |

**3.Cây thư mục Bookmark**

| Thư mục | 3 Link mẫu phải có |
| :--- | :--- |
| 📁 01_CarePulse_Clinic | 1. carepulse.ptit.edu.vn - Dashboard<br>2. carepulse/hosobenhan<br>3. gdbhyt.baohiemxahoi.gov.vn |
| 📁 02_Pharma_Portal | 1. Dược Thư Quốc Gia<br>2. Drug Interaction Checker<br>3. dav.gov.vn - Cục Dược |
| 📁 03_Medical_Tools | 1. Google Dịch<br>2. ICD-10 Tra mã bệnh<br>3. Mayo Clinic |


**4. Mẹo Favicon-Only**

| Bước | Thao tác |
| :--- | :--- |
| B1 | Kéo 4 trang hay dùng ra thanh Bar: CarePulse, BHYT, Dược Thư, Google Dịch |
| B2 | Chuột phải > Edit > Xóa hết tên |
| B3 | Chỉ để lại Icon |



**5. Xử lý Bẫy an ninh**


| Tình huống | Phải dùng gì? | Lý do |
| :--- | :--- | :--- |
| Khách mượn 30p | Guest Profile | Guest xóa trắng 100% sau khi đóng. Incognito vẫn lộ pass đã lưu |
| Bẫy Cloud Sync | TẮT Sync | Nếu bật Sync, Cookie, Pass CarePulse sẽ đẩy lên Gmail cá nhân -> rò rỉ dữ liệu bệnh nhân |

