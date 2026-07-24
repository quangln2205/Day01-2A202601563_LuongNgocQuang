# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> temperature 0.0, phản hồi mang tính rập khuôn, an toàn và dễ đoán (như ro  robot). Khi tăng lên 0.5 và 1.0, văn phong trở nên tự nhiên, thông tin đa dạng và thú vị hơn (ví dụ: cà phê trứng, ẩm thực đường phố). mức 1.5, mô hình bắt đầu mất kiểm soát, sinh ra từ ngữ lộn xộn, sai ngữ pháp hoặc bịa đặt thông tin (hallucination).

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Nên đặt ở mức thấp, khoảng 0.1 đến 0.3. Chatbot chăm sóc khách hàng cần sự chính xác tuyệt đối, nhất quán và tuân thủ chặt chẽ chính sách của công ty.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> GPT-4o đắt hơn GPT-4o-mini khoảng 30 đến 50 lần. Trường hợp đáng dùng GPT-4o: Khi cần xử lý logic phức tạp, suy luận đa bước hoặc viết mã nguồn khó (ví dụ: trợ lý phân tích hợp đồng pháp lý). Trường hợp nên dùng GPT-4o-mini: Phù hợp cho các tác vụ đơn giản, lặp đi lặp lại với khối lượng lớn như phân loại phản hồi khách hàng, tóm tắt ý chính tài liệu, hoặc chatbot kịch bản cơ bản.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona giáo viên tiểu học, câu trả lời thường ngắn, dùng từ vựng đời thường và ví dụ ẩn dụ. Ngược lại, persona chuyên gia tài chính sinh ra phản hồi dài hơn, cấu trúc phức tạp và sử dụng thuật ngữ chuyên ngành. System prompt hoạt động như một "khuôn đúc", thiết lập các ràng buộc nghiêm ngặt giúp định hình không chỉ nội dung mà còn cả giọng điệu, định dạng và góc nhìn của mô hình.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Số token đo bằng tiktoken thực tế thường cao hơn ước lượng "số từ / 0.75" khoảng 150% đến 250%. Nguyên nhân là do các bộ tokenizer. được huấn luyện chủ yếu trên kho dữ liệu tiếng Anh, nên một từ tiếng Việt có dấu thường không nằm trong từ điển token gốc và bị chia cắt nhỏ thành nhiều sub-words hoặc thậm chí từng ký tự riêng lẻ, làm phình to số lượng token.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt quan trọng khi làm app có giao diện chat tương tác trực tiếp với user, vì nó giúp giảm mạnh thời gian chờ (TTFB - Time To First Byte). Chữ hiện ra ngay sẽ khiến user thấy hệ thống phản hồi nhanh. Ngược lại, non-streaming phù hợp cho các task chạy ngầm (background jobs) như crawl data, tóm tắt tài liệu, trích xuất dữ liệu ra chuẩn JSON... vì hệ thống chỉ cần kết quả cuối cùng để xử lý bước tiếp theo, không có user nào ngồi đợi để đọc từng chữ cả.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp dãn cách thời gian thử lại, tạo khoảng trống cho server có thời gian "thở" và phục hồi. Nếu setup delay cố định là 1 giây, khi server sập, cả ngàn client sẽ cùng dội request lại vào đúng 1 giây sau đó. Đây là hiệu ứng Thundering Herd (bầy đàn tháo chạy), làm cho server vừa gượng dậy được đã lập tức bị đánh sập tiếp vì quá tải cục bộ.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> System Prompt: "Bạn là một Senior Code Reviewer khó tính. Chỉ trả về mã nguồn đã được tối ưu nhất, giải thích ngắn gọn độ phức tạp thuật toán (Big O) và tuyệt đối không giải thích lại các khái niệm lập trình cơ bản."
Giải thích: Cụm "không giải thích khái niệm cơ bản" giúp đi thẳng vào vấn đề, tiết kiệm đáng kể token đầu ra. Cụm "giải thích độ phức tạp Big O" ép model phải tư duy về tối ưu hóa thuật toán thay vì chỉ nhả ra đoạn code chạy được cho có.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế: Trợ lý không có bộ nhớ dài hạn. Nếu chat quá dài hoặc mở một session mới là nó quên sạch bối cảnh của mình.
Cải thiện: Tích hợp kỹ thuật RAG (Retrieval-Augmented Generation). Mình sẽ dùng một Vector Database để lưu lại các thông tin hoặc code snippet quan trọng. Khi chat câu mới, hệ thống sẽ search các thông tin liên quan trong DB này rồi tự động "nhồi" ngầm vào system prompt trước khi gọi API, giúp bot nhớ chuyện cũ mà không phải ôm toàn bộ lịch sử chat rất tốn token.

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
