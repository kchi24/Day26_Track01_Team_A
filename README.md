# BÁO CÁO BÀI LAB NHÓM: GIẢI CỨU SẢN PHẨM AI BỊ KẸT
**Mã Repo:** `Day26_Track01_Team_A`  
**Chủ đề:** Tái thiết kế quy trình, xây dựng Kiến trúc tin cậy và Lập Dashboard hành động đo lường giá trị ứng dụng Trợ lý AI tra cứu tài liệu nội bộ

---

## 1. Bảng thành viên & Đóng góp phản biện chéo

> *Lưu ý: Báo cáo được thực hiện theo hình thức **Cá nhân độc lập**.*

| Họ và tên | MSSV | Phần phụ trách trong bài | Góp ý cụ thể đã đưa cho nhóm bạn (Chặng 3) |
| :--- | :--- | :--- | :--- |
| **Nguyễn Văn A** *(Điền tên bạn)* | **2A20260001** *(Điền MSSV)* | Toàn bộ các phần: Khóa phạm vi, Chẩn đoán (Gartner-Lite, Mollick, ADKAR), Thiết kế TO-BE Workflow, Lộ trình 30-60-90, Xây dựng Dashboard v1/v2, Soạn thảo Memo | **Phản biện Nhóm 05 (CSKH E-commerce):**<br>1. *Về chỉ số:* Chỉ số "Số câu hỏi prompt vào bot" thuần túy là Activity metric, đề xuất đổi sang Workflow metric: "Thời gian xử lý khiếu nại đo từ log hệ thống ticket" và "Tỷ lệ ca khiếu nại giải quyết đúng hạn không cần QA sửa lại".<br>2. *Về quy trình:* Quy trình TO-BE chưa có người chịu trách nhiệm pháp lý cuối cùng; cần áp dụng Mollick để chỉ định rõ nhân viên CSKH là người giữ quyền duyệt bồi thường, AI chỉ hỗ trợ trích dẫn chính sách. |

---

## 2. Phạm vi đã khóa (Scope Lock)
* **Sản phẩm AI:** Trợ lý AI tra cứu chính sách và tài liệu quy định nội bộ (Internal Knowledge AI Assistant / RAG-based PolicyBot).
* **Nhóm người dùng chính:** Chuyên viên Vận hành & Dịch vụ Khách hàng (Operations & CSKH).
* **3 Quy trình tác nghiệp cốt lõi:**
  1. *Quy trình 1:* Tra cứu điều khoản bồi thường / hoàn tiền khi phát sinh khiếu nại từ khách hàng.
  2. *Quy trình 2:* Tra cứu quy trình leo thang xử lý sự cố dịch vụ gián đoạn (Incident Escalation SOP).
  3. *Quy trình 3:* Kiểm chứng tính hiệu lực văn bản và đối chiếu số hiệu quyết định áp dụng.

---

## 3. Nguyên nhân gốc & Bằng chứng thực tế (Root Cause Analysis)
* **Nguyên nhân gốc 1 (Kiến trúc tin cậy - Trust Architecture):** Bot trả lời không có trích dẫn nguồn văn bản, không hiển thị ngày ban hành chính sách và từng xuất hiện hiện tượng bịa thông tin (hallucination). Áp dụng **ADKAR**, người dùng bị tắc nghẽn nghiêm trọng ở mắt xích **Desire (Động lực/Niềm tin)** do sợ bị phạt tiền túi nếu áp dụng sai chính sách cũ.
  * *Bằng chứng:* Log hệ thống RAG cho thấy chỉ **18.5%** câu trả lời có link trích dẫn nguồn; phỏng vấn nhanh 15 nhân viên CSKH thì 13/15 người khẳng định không dám dùng bot vì sợ sai sót nghiệp vụ.
* **Nguyên nhân gốc 2 (Quy trình & Bàn giao - Workflow & Readiness):** Áp dụng **Gartner-Lite**, tổ chức đạt về Hướng đi (Direction) nhưng thiếu nghiêm trọng tầng **Readiness & Absorption**: kho dữ liệu chưa có Data Owner chịu trách nhiệm cập nhật, AI nằm ngoài quy trình tác nghiệp chính thức, chưa phân định ranh giới trách nhiệm theo **Mollick**.
  * *Bằng chứng:* Log hệ thống Jira Service Management ghi nhận thời gian xử lý 1 ca khiếu nại trung bình là **42 phút/ca**, trong đó hơn 65% thời gian là nhân viên tự tìm file trên Drive hoặc đi hỏi miệng đồng nghiệp cũ.

---

## 4. Cách làm mới (Tái cấu trúc Quy trình & Kiến trúc tin cậy)
Quy trình được tái thiết kế từ **AS-IS** (tìm file thủ công ➔ hỏi đồng nghiệp ➔ phỏng đoán áp dụng) sang **TO-BE** với **3 thay đổi bắt buộc**:
1. **Nguồn kiểm chứng (Citations & Metadata):** 100% câu trả lời của AI bắt buộc phải kèm link văn bản gốc, số hiệu điều khoản, số trang và ngày cập nhật hiệu lực gần nhất. Bổ sung nút *Báo lỗi 1-click (Feedback loop)* để người dùng phản hồi khi AI trả lời sai lệch.
2. **Người chịu trách nhiệm cuối cùng (Human-in-command theo Mollick):** Nhân viên CSKH là người trực tiếp đối chiếu trích dẫn, giữ quyền quyết định cuối cùng và chịu trách nhiệm nghiệp vụ; AI chỉ đóng vai trò trợ lý tăng tốc tra cứu và tóm tắt điều khoản.
3. **Cách xử lý khi AI không chắc chắn (Fallback & Escalation Protocol):** Nếu độ tin cậy của thuật ngữ < 85% hoặc tài liệu chưa được index, AI lập tức hiển thị cảnh báo *"Không đủ căn cứ xác thực"*, đồng thời tự động kích hoạt luồng chuyển tiếp ticket sang Trưởng ca/Chuyên viên Pháp chế xử lý thủ công trong vòng 15 phút.

---

## 5. Hệ thống chỉ số ra quyết định (Trích từ Dashboard v2)

| Loại chỉ số | Tên chỉ số cụ thể | Mốc ban đầu (Baseline) | Mục tiêu hướng tới (Target) | Nguồn dữ liệu khách quan | Người phụ trách (Owner) | Hành động kích hoạt khi chỉ số xấu |
| :--- | :--- | :---: | :---: | :--- | :--- | :--- |
| **Workflow Metric** *(Năng suất quy trình)* | Thời gian trung bình hoàn tất 1 ca tra cứu & xử lý khiếu nại chính sách | **42 phút** *(Đo tháng 9/2026)* | **≤ 15 phút** *(Giảm 64% thời gian)* | Task resolution timestamp trên hệ thống **Jira Service Management** | Chủ quy trình CSKH (CS Process Owner) | Rà soát lại template prompt chuẩn; kiểm tra điểm nghẽn tại bước đối chiếu nguồn; tổ chức kèm cặp 1-1 cho ca xử lý >25 phút. |
| **Product Metric** *(Chất lượng sản phẩm)* | Tỷ lệ câu trả lời của AI có trích dẫn nguồn văn bản chính xác và còn hiệu lực | **18.5%** *(Hiện trạng)* | **≥ 95.0%** *(Ngưỡng an toàn)* | Báo cáo kiểm toán chất lượng định kỳ hàng tuần từ mẫu 50 câu hỏi của **Tổ QA** | AI Technical Lead & QA Lead | **LẬP TỨC:** Tạm khóa các thư mục dữ liệu chưa chuẩn hóa; chỉ cho phép bot truy xuất vào tập dữ liệu đã có chữ ký số của Data Owner. |

---

## 6. Quyết định chiến lược & Bài học nâng cấp v1 ➔ v2

> ### 🟡 QUYẾT ĐỊNH: SỬA (PIVOT / FIX READINESS) — CHƯA MỞ RỘNG (SCALE)
> **Lý do:** Hướng đi nghiệp vụ (Direction) đúng đắn nhưng nền tảng dữ liệu và độ tin cậy (Readiness & Trust) chưa đạt ngưỡng an toàn; tạm dừng việc triển khai toàn công ty, giữ pilot hẹp tại nhóm 30 nhân viên CSKH để sửa lỗi hệ thống và hoàn thiện quy trình kiểm chứng.

### Hai thay đổi cốt lõi so với bản v1 (Minh chứng nâng cấp sau kiểm tra chéo):
1. **Thay đổi 1 (Hệ thống đo lường):** Loại bỏ hoàn toàn các chỉ số bề nổi vô giá trị ở bản v1 (*Số lượt login, số câu prompt gửi vào bot*) để chuyển sang chỉ số ra quyết định ở bản v2 (*Thời gian xử lý ca đo từ log Jira khách quan* và *Tỷ lệ trích nguồn kiểm chứng được đạt ≥95%*).
2. **Thay đổi 2 (Giải pháp xử lý điểm nghẽn):** Bản v1 đề xuất mở thêm lớp đào tạo (do nhầm lẫn nguyên nhân là thiếu Knowledge). Bản v2 nhận diện điểm nghẽn nằm ở **Desire (nỗi sợ rủi ro do AI thiếu tin cậy)** theo ADKAR, từ đó thay thế đào tạo bằng việc thiết lập **Kiến trúc tin cậy (Trust Architecture)** kèm cơ chế *miễn trừ trách nhiệm cho nhân viên khi thực hiện đúng bước kiểm chứng nguồn*.
