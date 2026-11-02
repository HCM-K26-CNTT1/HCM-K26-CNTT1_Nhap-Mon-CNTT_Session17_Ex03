# BÀI 3: XỬ LÝ SỰ CỐ GIAO DIỆN WEBSITE DOANH NGHIỆP

## Phần 1. Báo cáo phân tích (I/O)

### 1. Phân tích lỗi giao diện

**Hiện trạng:**

Trên PTIT-LMS, khung "Nội dung bài học" bị chồng lên khung "Danh sách video", khiến người dùng khó xem và thao tác.

**Cách kiểm tra bằng DevTools:**

1. Mở PTIT-LMS trên Chrome.
2. Nhấn `F12` để mở DevTools.
3. Nhấn `Ctrl + Shift + C` để chọn phần tử bị lỗi.
4. Chọn khung "Nội dung bài học", sau đó mở tab **Computed**.
5. Tiếp tục kiểm tra khung "Danh sách video".
6. Quan sát 4 thành phần của Box Model.

**Sơ đồ Box Model:**

```text
+---------------------------+
|          Margin           |
|  +---------------------+  |
|  |        Border       |  |
|  |  +---------------+  |  |
|  |  |    Padding    |  |  |
|  |  |  +---------+  |  |  |
|  |  |  | Content |  |  |  |
|  |  |  +---------+  |  |  |
|  |  +---------------+  |  |
|  +---------------------+  |
+---------------------------+
```

**Ý nghĩa 4 thành phần:**

- Content: Phần chứa nội dung bên trong khung.
- Padding: Khoảng cách từ nội dung đến viền.
- Border: Đường viền của khung.
- Margin: Khoảng cách bên ngoài khung với các phần tử khác.

**Nguyên nhân dự đoán:**

- Margin quá nhỏ hoặc bị đặt giá trị âm.
- Padding quá lớn làm khung chiếm thêm diện tích.
- Chiều rộng của hai khung không phù hợp.
- Hai phần tử sử dụng thuộc tính `position` không hợp lý.

Lưu ý: Chưa có ảnh chụp DevTools nên các nguyên nhân và thông số dưới đây là phương án dự kiến, cần kiểm tra thực tế trước khi sửa.

## Phần 2. Thiết kế giải pháp

### 2. Điều chỉnh CSS trong bảng Styles

**Mục tiêu:** Tách hai khung nội dung, tạo khoảng cách hợp lý và không làm vỡ giao diện.

Ví dụ CSS đề xuất:

```css
.lesson-content {
    box-sizing: border-box;
    width: 70%;
    padding: 16px;
    margin-bottom: 20px;
    position: static;
}

.video-list {
    box-sizing: border-box;
    width: 30%;
    padding: 16px;
    margin: 0;
    position: static;
}
```

**Cách sửa bằng DevTools:**

1. Chọn khung "Nội dung bài học".
2. Mở tab **Styles**.
3. Thử điều chỉnh `padding`, `margin` và `width`.
4. Làm tương tự với khung "Danh sách video".
5. Quan sát xem hai khung đã tách biệt và hiển thị đúng chưa.

Lưu ý: Hai giá trị width 70% và 30% chỉ phù hợp khi hai khung nằm cạnh nhau và vùng chứa không có khoảng cách bổ sung. Nếu cần khoảng cách giữa hai khung, phải tính lại chiều rộng.

### 3. Bẫy dữ liệu: Thay đổi Margin mà không kiểm tra Padding

Nếu chỉ tăng Margin mà không kiểm tra Padding, khung có thể bị đẩy xa hơn nhưng vẫn chiếm quá nhiều diện tích do Padding lớn.

Hậu quả:
- Nội dung bị lệch vị trí.
- Khung có thể tràn khỏi vùng hiển thị.
- Giao diện bị vỡ trên màn hình nhỏ.

Vì vậy, cần kiểm tra cả Margin và Padding trước khi điều chỉnh.

## Phần 3. Kết quả mong đợi

- Hai khung "Nội dung bài học" và "Danh sách video" không còn chồng lấn.
- Nội dung hiển thị rõ ràng, dễ thao tác.
- Khoảng cách giữa hai khung hợp lý.
- Giao diện không bị tràn khi thay đổi kích thước màn hình.