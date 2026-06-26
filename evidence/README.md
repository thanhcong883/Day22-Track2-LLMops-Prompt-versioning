# Phân tích kết quả đánh giá RAGAS (V1 vs V2)

Dưới đây là bảng so sánh điểm đánh giá RAGAS giữa 2 phiên bản prompt:

| Chỉ số | V1 (Ngắn gọn, Thân thiện) | V2 (Chuyên nghiệp, Có cấu trúc) | Người chiến thắng |
| :--- | :---: | :---: | :---: |
| **Faithfulness (Độ trung thực)** | **0.9469** | 0.8804 | **V1** |
| **Answer Relevancy (Độ liên quan)** | 0.9152 | 0.8836 | **V1** |
| **Context Recall (Độ thu hồi ngữ cảnh)**| 1.0000 | 1.0000 | Hòa |
| **Context Precision (Độ chuẩn xác)** | 0.9483 | 0.9417 | **V1** |

---

## Phân tích & Giải thích chi tiết

### 1. Tại sao V1 có điểm Faithfulness cao hơn V2?
* **Faithfulness** đo lường tỷ lệ các tuyên bố (claims) trong câu trả lời được hỗ trợ trực tiếp bởi ngữ cảnh (context) được truy xuất.
* **Prompt V1** yêu cầu LLM trả lời ngắn gọn, cô đọng (2-4 câu) và đi thẳng vào vấn đề. Do đó, mô hình chỉ trích xuất các thông tin cốt lõi nhất từ context để tạo câu trả lời, hạn chế tối đa việc thêm thắt suy luận hoặc kiến thức bên ngoài ngữ cảnh.
* **Prompt V2** yêu cầu mô hình có cấu trúc chi tiết và phong cách chuyên nghiệp. Điều này thúc đẩy LLM viết dài hơn, diễn giải nhiều hơn và đôi khi tự động bổ sung thêm kiến thức nền (dù đúng về mặt lý thuyết nhưng không được ghi cụ thể trong context được truy xuất). Điều này làm RAGAS đánh giá các câu trả lời V2 chứa những tuyên bố "không có cơ sở trong context", làm giảm điểm Faithfulness.

### 2. Answer Relevancy
* V1 cũng đạt điểm số cao hơn ở độ liên quan của câu trả lời. Nhờ trả lời ngắn gọn và tập trung trực tiếp vào câu hỏi, câu trả lời của V1 ít bị loãng bởi các thông tin bổ sung và định dạng phức tạp như ở V2.

### 3. Context Recall và Context Precision
* Điểm số Context Recall của cả 2 phiên bản đều đạt 1.0000 vì cả hai đều dùng chung một bộ retriever với tham số `k=3` trên cùng một cơ sở dữ liệu vector.
* Context Precision của V1 nhỉnh hơn một chút (0.9483 so với 0.9417) do định dạng ngắn gọn của V1 giúp RAGAS dễ dàng ánh xạ sự tương quan giữa các đoạn context được trả về và các câu trả lời tương ứng.

### Kết luận
Đối với hệ thống hỏi đáp dựa trên RAG trong bài lab này, **phiên bản Prompt V1 (Ngắn gọn, Thân thiện)** đem lại hiệu quả cao hơn về mặt định lượng, đặc biệt là giảm thiểu rủi ro ảo tưởng (hallucination) nhờ việc hạn chế độ dài câu trả lời của LLM.
