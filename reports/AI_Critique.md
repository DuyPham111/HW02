# AI Critique — HW02 Domain Testing

**Sinh viên:** Phạm Vũ Ngọc Duy (23127183)

AI (Claude Code) đã sai hoặc thiếu ở vài điểm. Ở FR-02, AI dự đoán khóa sau 2 lần sai, chỉ dựa đọc tĩnh dòng `login_attempts + 2`, nhưng thực thi thật cho thấy khóa đúng ở lần thứ 3 — do AI không truy vết thứ tự xử lý request (backend kiểm tra trạng thái khóa trước khi cập nhật bộ đếm của chính lần gọi đó). Ở FR-15, chỉ đọc code backend, AI kết luận sửa sản phẩm đúng đặc tả vì UPDATE chỉ tác động một dòng theo id — nhưng bug nặng nhất (sửa một sản phẩm làm đổi tên toàn bộ danh sách) nằm ở tầng client, độc lập backend, chỉ lộ khi tự chạy thử. Ở FR-02 Mobile, AI mặc định app hiển thị đúng lỗi gốc từ backend theo thói quen React Native, không phát hiện khối `catch` đã âm thầm thay lỗi thật bằng câu chung chung.

AI không tự bắt được vì nó suy diễn theo khuôn mẫu phổ biến thay vì bám sát code cụ thể, và không thể tự chạy ứng dụng để quan sát hành vi thật — mọi kết luận chỉ là giả thuyết chờ xác nhận.

Nguyên tắc rút ra: AI rất mạnh khi sinh cấu trúc kiểm thử có hệ thống (phân vùng, điểm biên, bảng test case) nhanh và đầy đủ, nhưng không thể thay con người xác nhận kết quả thực tế — mọi "Actual" chỉ có giá trị sau khi tự tay kiểm chứng trên giao diện. Với hệ thống nhiều tầng (web, admin, mobile dùng chung backend), bug có thể nằm độc lập ở bất kỳ tầng nào, phải kiểm tra tách bạch, không suy đoán tầng này giống tầng kia.
