# MEMO QUYẾT ĐỊNH: ĐIỀU CHỈNH CHIẾN LƯỢC ÁP DỤNG TRỢ LÝ AI TRA CỨU TÀI LIỆU NỘI BỘ

* **Kính gửi:** Ban Điều hành & Hội đồng Chuyển đổi số (Digital Transformation Steering Committee)
* **Người thực hiện:** Thân Thị Kim Chi
* **Mã dự án:** `Day26_Track01_Team_A` (Trợ lý AI Tra cứu Chính sách & Tài liệu Nội bộ)

---

## 1. Vấn đề thực tế và Nguyên nhân gốc

### 1.1 Hiện trạng & Vấn đề quan sát
Doanh nghiệp đã cấp phát tài khoản AI Assistant (RAG-based Knowledge Bot) cho hơn 200 nhân sự khối Vận hành & Dịch vụ Khách hàng (Deployment). Tuy nhiên, sau 2 tháng triển khai, tỷ lệ sử dụng thực tế (Adoption) gần như bằng không: nhân viên vẫn mở hàng chục thư mục Google Drive để tìm file PDF thủ công hoặc nhắn tin hỏi các đồng nghiệp có thâm niên. Khi có khiếu nại phát sinh từ khách hàng, thời gian tra cứu và giải quyết kéo dài trung bình **42 phút/ca**.

### 1.2 Hai nguyên nhân cốt lõi (Root Causes)
1. **Thiếu Kiến trúc tin cậy (Trust Architecture):** Bot trả lời không dẫn chiếu số hiệu văn bản, không có đường link trích dẫn đoạn văn gốc và không hiển thị ngày cập nhật hiệu lực. Nhân viên lo sợ nếu tin theo AI mà áp dụng sai chính sách bồi thường đã hết hạn thì sẽ bị phạt hoặc trừ KPI cá nhân.
2. **Quy trình chưa phân định rõ vai trò và điểm bàn giao (Workflow & Handoff):** Việc dùng AI chưa được đưa vào Quy trình tác nghiệp chuẩn (SOP). Chưa xác định rõ quyền hạn: ai là người kiểm chứng tính xác thực, ai chịu trách nhiệm pháp lý cuối cùng đối với quyết định hoàn tiền cho khách hàng.

---

## 2. Framework đã áp dụng & Bằng chứng thực tế

Nhóm đã sử dụng phối hợp 3 framework chuyên sâu để chẩn đoán toàn diện:

### 2.1 Gartner-Lite (Đánh giá mức độ sẵn sàng tổ chức)
* **Direction (Hướng đi):** **ĐẠT**. Mục tiêu rõ ràng là rút ngắn thời gian xử lý khiếu nại và tra cứu chính sách cho nhân viên CSKH.
* **Readiness (Mức sẵn sàng):** **THIẾU NGHIÊM TRỌNG**. Kho dữ liệu chứa nhiều phiên bản chính sách cũ/mới lẫn lộn; chưa có Data Owner chịu trách nhiệm cập nhật văn bản định kỳ; phân quyền tài liệu chưa đồng bộ.
* **Absorption (Khả năng hấp thụ):** **THIẾU**. Chưa có cơ chế ghi nhận câu trả lời sai (Feedback loop) và chưa có đầu mối hỗ trợ vận hành khi AI gặp ca ngoại lệ (Edge case).

### 2.2 ADKAR Model (Định vị điểm nghẽn hành vi người dùng)
* **Awareness (Nhận thức):** Nhân viên biết có công cụ bot AI.
* **Desire (Động lực/Mong muốn):** **ĐIỂM NGHẼN CHÍNH (NGHẼN TẠI D)**. Nhân viên sợ chịu rủi ro vì bot từng xảy ra hiện tượng "ảo giác" (hallucination). Họ từ chối dùng vì không muốn mạo hiểm trách nhiệm cá nhân. Mọi nỗ lực mở lớp đào tạo (Knowledge) đều thất bại vì chưa gỡ được rào cản tâm lý này.
* **Reinforcement (Duy trì):** Hệ thống quản trị chỉ đo lượt login, không đánh giá chất lượng và không có cơ chế bảo vệ nhân viên khi làm đúng quy trình kiểm chứng.

### 2.3 Mollick Framework (Tái cấu trúc phân công Người – Máy)
* **Người giữ quyền (Human-in-command):** Nhân viên CSKH/Trưởng ca giữ quyền phê duyệt cuối cùng và chịu trách nhiệm nghiệp vụ về quyết định giải quyết khiếu nại.
* **AI hỗ trợ (Centaurs/Cyborgs):** AI đóng vai trò tra cứu tức thì, trích xuất điều khoản và tóm tắt nội dung chính sách kèm link đối chiếu.
* **AI tự động (Full Auto):** Chỉ áp dụng với việc tra cứu mã lỗi chuẩn hóa hoặc đối chiếu định dạng số hợp đồng (rủi ro bằng 0).

### 2.4 Bằng chứng thực tế (Hard Evidence)
* **Log hệ sinh thái AI:** Chỉ có **18.5%** câu trả lời của bot có kèm trích dẫn nguồn văn bản (tháng 9/2026).
* **Khảo sát & Phỏng vấn sâu:** 13/15 nhân viên CSKH được phỏng vấn khẳng định: *"Tôi không dám dùng bot vì từng có bạn áp dụng nhầm chính sách giảm giá cũ bị trừ tiền túi đền cho công ty"*.
* **Log Jira Service Management:** Thời gian trung bình xử lý 1 ca khiếu nại hoàn tiền là **42 phút/ca**, trong đó thời gian đi hỏi đồng nghiệp và tìm văn bản chiếm hơn 65%.

---

## 3. Ít nhất 2 thay đổi cụ thể sau phản biện chéo (Peer Review)

Sau phiên kiểm tra chéo ở Chặng 3 với nhóm đối ứng (Nhóm 05), bản kế hoạch đã tiếp thu và nâng cấp 2 điểm cốt tử từ bản v1 lên v2:

| Hạng mục | Bản v1 (Trước phản biện) | Bản v2 (Sau tiếp thu phản biện) | Lý do điều chỉnh |
| :--- | :--- | :--- | :--- |
| **Thay đổi 1: Hệ thống chỉ số** | Chỉ đo các chỉ số thao tác bề nổi (Activity metrics): *Số lượt login hàng tuần (45 lượt)* và *Số câu prompt gửi vào bot (80 câu)*. | Chuyển dịch toàn diện sang chỉ số quy trình & chất lượng (Decision metrics):<br>• **Workflow metric:** *Thời gian xử lý 1 ca khiếu nại đo từ log Jira (Baseline 42p ➔ Target ≤15p)*.<br>• **Product metric:** *Tỷ lệ trích dẫn nguồn chính xác có kiểm chứng (Baseline 18.5% ➔ Target ≥95%)*. | Số lượt login cao không đồng nghĩa với năng suất tăng; nhân viên có thể login rồi không dùng, hoặc hỏi nhiều câu do bot trả lời lòng vòng. Cần đo lường giá trị thực tế tại quy trình tác nghiệp. |
| **Thay đổi 2: Giải pháp tháo gỡ điểm nghẽn** | Đề xuất giải pháp thuần túy là: *"Mở thêm 2 lớp đào tạo bắt buộc và tổ chức thi đua prompt có thưởng"*. | Bỏ đào tạo đại trà, tập trung xây dựng **Kiến trúc tin cậy (Trust Architecture)**:<br>• Bổ sung tính năng hiển thị số hiệu văn bản + link nguồn + ngày cập nhật.<br>• Thêm nút *Báo lỗi 1-click*.<br>• Ban hành chính sách: *Nhân viên được miễn trừ trách nhiệm nếu đã thực hiện đúng bước đối chiếu nguồn do AI trích xuất*. | Nhận diện đúng điểm nghẽn nằm ở **Desire (Niềm tin & Rủi ro cá nhân)** theo ADKAR, không phải thiếu Knowledge. Đào tạo không thể giải quyết được nỗi sợ bị phạt nếu công cụ chưa đủ tin cậy. |

---

## 4. Quyết định chiến lược

> ### QUYẾT ĐỊNH: SỬA (PIVOT / FIX READINESS) — CHƯA MỞ RỘNG (SCALE)

* **Tuyên bố quyết định:** **TẠM DỪNG VIỆC ROLLOUT TRÊN TOÀN CÔNG TY.** Giữ nguyên phạm vi pilot hẹp tại Đội Vận hành CSKH (30 nhân sự trọng điểm) trong vòng 60 ngày để khắc phục triệt để tầng sẵn sàng (Readiness) và hoàn thiện Kiến trúc tin cậy (Trust Architecture). Chỉ chuyển sang giai đoạn mở rộng khi vượt qua Gate 60 và Gate 90 bằng bằng chứng số liệu.

---

## 5. Lý do, Bước tiếp theo & Phân công người phụ trách (Owners)

### 5.1 Lập luận bảo vệ quyết định
* **Tại sao không TIẾP TỤC (Scale ngay)?** Tiếp tục ép nhân viên toàn công ty dùng một hệ thống chưa có trích nguồn chính xác (hiện mới đạt 18.5%) sẽ gia tăng rủi ro sai sót nghiệp vụ, làm tổn hại uy tín thương hiệu và triệt tiêu vĩnh viễn niềm tin của nhân sự vào các sáng kiến AI tương lai.
* **Tại sao không DỪNG (Kill dự án)?** Vì Hướng đi (Direction) hoàn toàn đúng đắn: nhu cầu tra cứu là có thật, bài toán tiết kiệm 60% thời gian xử lý ca là khả thi nếu giải quyết được bài toán dữ liệu sạch (RAG indexing) và phân vai Người - Máy rõ ràng.

### 5.2 Kế hoạch hành động 30–60–90 ngày & Owners

```
[0 - 30 NGÀY: CỔNG 1 - FIX READINESS & CHUẨN HÓA NGUỒN DỮ LIỆU]
  ├── Nhiệm vụ 1: Chỉ định Data Owner; dọn dẹp, phân loại và gán nhãn hiệu lực cho 100% tài liệu chính sách.
  ├── Nhiệm vụ 2: Cấu hình lại hệ thống RAG: bắt buộc mọi câu trả lời phải trích dẫn số trang, link tài liệu và ngày cập nhật.
  ├── Nhiệm vụ 3: Đo lường chính xác mốc Baseline thời gian tác vụ trên hệ thống Jira của nhóm pilot.
  └── Owner chịu trách nhiệm Gate 1: AI Technical Lead & Head of Data.

[31 - 60 NGÀY: CỔNG 2 - CHỨNG MINH ĐỘ TIN CẬY & TRIỂN KHAI PILOT KIỂM SOÁT]
  ├── Nhiệm vụ 1: Vận hành quy trình mới (TO-BE) trên nhóm 30 nhân viên CSKH pilot.
  ├── Nhiệm vụ 2: Triển khai kiểm tra mẫu định kỳ (QA sampling 15% câu trả lời/tuần); đo tỷ lệ trích nguồn đạt ≥95%.
  ├── Nhiệm vụ 3: Ban hành cơ chế miễn trừ trách nhiệm khi nhân viên thực hiện đúng bước kiểm chứng nguồn.
  └── Owner chịu trách nhiệm Gate 2: Trưởng phòng Vận hành CSKH & QA Lead.

[61 - 90 NGÀY: CỔNG 3 - ĐÁNH GIÁ GIÁ TRỊ THỰC TẾ & RA QUYẾT ĐỊNH TOÀN DIỆN]
  ├── Nhiệm vụ 1: Đo lường thời gian xử lý ca thực tế (mục tiêu rút xuống ≤15 phút) và tỷ lệ khiếu nại lại (≤1%).
  ├── Nhiệm vụ 2: Đối chiếu chi phí vận hành tiết kiệm được (mục tiêu tiết kiệm >60% chi phí thời gian).
  ├── Nhiệm vụ 3: Họp Ban Điều hành: Nếu đạt toàn bộ chỉ số ➔ Phê duyệt ngân sách mở rộng ra phòng Nhân sự & Pháp chế; Nếu không đạt ➔ Tuyên bố dừng (Kill).
  └── Owner chịu trách nhiệm Gate 3: Giám đốc Vận hành (COO) & Product Owner AI.
```
