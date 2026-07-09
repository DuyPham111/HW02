# AI Critique — HW02 Domain Testing

**Sinh viên:** Phạm Vũ Ngọc Duy (23127183)

Trong HW02, em sử dụng chủ yếu Claude Code để hỗ trợ phân tích, xây dựng test case và hoàn thiện báo cáo. Tuy nhiên, AI vẫn còn hạn chế rõ rệt. Ở FR-02, AI dự đoán tài khoản bị khóa sau 2 lần đăng nhập sai chỉ dựa trên đọc tĩnh source code, nhưng thực tế chỉ bị khóa ở lần thứ 3 do thứ tự xử lý của backend. Với FR-15, AI kết luận chức năng sửa sản phẩm hoạt động đúng vì chỉ phân tích backend, trong khi bug nghiêm trọng lại nằm ở tầng client, chỉ phát hiện được khi chạy thử trên giao diện thật. Tương tự, ở FR-02 Mobile, AI không nhận ra khối `catch` đã thay thông báo lỗi backend bằng thông báo chung, khiến bug chỉ lộ ra khi thao tác trên app.

Ngoài ra, Claude Code đôi khi chưa quản lý task hiệu quả — có lúc chưa xong một yêu cầu đã chuyển việc khác mà không báo rõ. AI sinh test case nhanh, cấu trúc rõ ràng, nhưng vẫn bỏ sót vài trường hợp biên nếu không được yêu cầu bổ sung, và đôi khi diễn giải report chưa chính xác nên cần kiểm tra lại.

Qua bài tập này, em nhận thấy AI là công cụ hỗ trợ tốt để tăng tốc phân tích và xây dựng tài liệu, nhưng không thể thay người kiểm thử. Mọi kết quả AI tạo ra cần đối chiếu source code và kiểm chứng bằng thực thi hệ thống trước khi đưa vào báo cáo. Chia nhỏ yêu cầu, mô tả rõ nhiệm vụ và luôn rà soát đầu ra của AI giúp giảm sai sót, nâng cao chất lượng.