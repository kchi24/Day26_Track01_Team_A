# BIÊN BẢN GHI NHẬN PHẢN BIỆN CHÉO 2 CHIỀU (PEER REVIEW LOG) — CHẶNG 3

* **Thời gian thực hiện:** 2026-10-14 (Phiên làm việc Chặng 3: Phút 80 – Phút 120)  
* **Đơn vị thực hiện:** `Day26_Track01_Team_A` (Thân Thị Kim Chi — MSSV: 2A202602797)  
* **Đối tác phản biện chéo:** `Nhóm Anh Em Siêu Nhân` (Track 01 — Repo: [trantran2929/Day26_Track01_anhemsieunhan](https://github.com/trantran2929/Day26_Track01_anhemsieunhan))  
* **Đại diện Nhóm Anh Em Siêu Nhân:**  
  - Bùi Thị Ngọc Trân (2A202602529) — Nhóm trưởng / Case Research Lead  
  - Ngô Lê Thủy Tiên (2A202602614) — Metrics & Dashboard Lead  
  - Phạm Thành Đạt (2A202602721) — Workflow Lead  

---

## 1. Mục đích và Quy chuẩn Đánh giá

Phiên kiểm tra chéo được thực hiện nghiêm túc theo **4 trục đánh giá chuẩn hóa của bài Lab**:
1. **Trục 1 — Phạm vi (Scope):** Sản phẩm AI, người dùng chính và 2–4 quy trình tác nghiệp đã khóa chặt chẽ và khả thi chưa?
2. **Trục 2 — Framework (Phương pháp luận):** Gartner-Lite, Mollick, ADKAR có thực sự giúp tìm ra nguyên nhân gốc rễ hay chỉ mang tính dán nhãn hình thức?
3. **Trục 3 — Chỉ số (Metrics & Logic):** Có đo lường quy trình (workflow), chất lượng (product) và giá trị (business value) không? Nguồn dữ liệu log có khách quan không?
4. **Trục 4 — Hành động (Actions & Governance):** Lộ trình 30–60–90 ngày, phân công Owner và cách kích hoạt xử lý khi chỉ số xấu có cụ thể và khả thi không?

---

## 2. Chiều 1: Nhóm Anh Em Siêu Nhân phản biện & Góp ý cho Team A (Kim Chi)

Nhóm Anh Em Siêu Nhân đã rà soát bản Dashboard v1 và chẩn đoán ban đầu của Team A, đưa ra các nhận xét và khuyến nghị điều chỉnh mang tính bước ngoặt:

| Trục đánh giá | Nhận xét chi tiết từ Nhóm Anh Em Siêu Nhân | Khuyến nghị điều chỉnh thực tế | Hành động tiếp thu & Sửa đổi của Team A (Lên bản v2) |
|---|---|---|---|
| **Trục 1: Phạm vi** | Phạm vi bài toán tra cứu chính sách bồi thường CSKH là rất rõ ràng, nhưng cần lưu ý phân định rõ giữa ca bồi thường đơn giản và ca tranh chấp lớn có yếu tố pháp lý. | Khóa chặt pilot ở nhóm 30 nhân viên xử lý khiếu nại thông thường, không mở rộng sang xử lý tranh chấp pháp lý phức tạp khi chưa có cơ chế kiểm duyệt. | Đã khóa cứng phạm vi pilot tại 30 chuyên viên Vận hành CSKH giải quyết khiếu nại bồi thường thông thường; ca pháp lý chuyển sang luồng Escalation riêng. |
| **Trục 2: Framework** | Bản v1 chẩn đoán người dùng không chịu dùng bot là do "thiếu kiến thức prompt" và đề xuất "mở thêm lớp đào tạo" là **mắc bẫy đào tạo (Training Fallacy)**. Vấn đề thực chất nằm ở **Desire (ADKAR)**: nhân viên sợ áp dụng sai chính sách bồi thường bị trừ tiền túi. | Bỏ giải pháp đào tạo vẹt; vận dụng Mollick để thiết lập quy chế bảo vệ: Nhân viên là người giữ quyền bấm duyệt bồi thường (Human-in-command), AI chỉ hỗ trợ trích dẫn và tóm tắt điều khoản. | Xóa bỏ kế hoạch mở lớp học bù; xây dựng **Kiến trúc tin cậy (Trust Architecture)** kèm **Chính sách miễn trừ trách nhiệm** cho nhân viên khi thực hiện đúng bước đối chiếu nguồn. |
| **Trục 3: Chỉ số** | Dashboard v1 mắc bẫy nặng nề về **Activity Metrics**: đếm lượt login SSO (45 lượt/tuần) và số câu prompt (80 câu/ngày). Đây là chỉ số ảo, nhân viên login rồi để đó không chứng minh được năng suất hay giá trị. | Loại bỏ hoàn toàn login/prompt; thay bằng Workflow metric đo thời gian thật từ log Jira (`M03_CYCLE_TIME`) và Product metric đo tỷ lệ trích nguồn văn bản chính xác (`M04_CITE_ACC`). | Nâng cấp toàn diện Dashboard v2 với 6 chỉ số 5 tầng: `M03` đo bằng timestamp Jira (Baseline 42p ➔ Target ≤15p); `M04` đo qua audit QA 50 câu/tuần (Baseline 18.5% ➔ Target ≥95%). |
| **Trục 4: Hành động** | Cột "Hành động khi chỉ số xấu" ở bản v1 rất thô sơ ("gửi email nhắc nhở", "nhắc qua Slack"). Cần thiết lập ngưỡng báo động đỏ và chế tài kỹ thuật cụ thể. | Bổ sung cơ chế ngắt khẩn cấp (Hard-stop/Kill-switch) khi tỷ lệ trích nguồn <90% hoặc phát sinh khiếu nại sai chính sách. Phân công Owner đích danh cho từng chỉ số. | Thiết lập cột Red Threshold và Action Trigger: tạm khóa ngay thư mục chưa chuẩn hóa; chỉ index tài liệu có chữ ký số Data Owner; phân công rõ Process Owner, AI Tech Lead, QA Lead, COO. |

---

## 3. Chiều 2: Team A (Kim Chi) phản biện & Góp ý cho Nhóm Anh Em Siêu Nhân

Dựa trên việc đọc và phân tích chi tiết case Klarna AI CSKH của Nhóm Anh Em Siêu Nhân, Team A đã gửi lại các đóng góp phản biện chuyên sâu:

| Trục đánh giá | Nhận xét và Góp ý của Team A (Kim Chi) gửi Nhóm Anh Em Siêu Nhân | Cơ sở lý luận & Bằng chứng thực tế đối chiếu | Đánh giá phản hồi từ Nhóm Anh Em Siêu Nhân |
|---|---|---|---|
| **Trục 1 & 2: Phân tầng Rủi ro & Mollick** | *Về phân tầng:* Bản thảo đầu của nhóm bạn tính tỷ lệ giải quyết của AI (P1) gộp chung cho toàn bộ cuộc trò chuyện. Việc này che giấu thực trạng AI có thể trả lời tốt câu hỏi FAQ dễ nhưng thất bại hoàn toàn ở các ca khiếu nại hoàn tiền phức tạp.<br>*Đề xuất:* Áp dụng Mollick để phân tách ranh giới rõ ràng: chỉ tự động hóa ca Low Risk; ca High Risk bắt buộc phải định tuyến Human-first ngay từ đầu. | Dẫn chứng từ case Bloomberg [S2] (CEO Klarna thừa nhận chất lượng giảm ở ca khó) và bài học Morgan Stanley [S1] (con người luôn thẩm định ca nhạy cảm). | Nhóm bạn đã tiếp thu 100%, bổ sung ngay tab `Segments` phân tầng P1 theo 3 tầng rủi ro (Low / Med / High) và 2 nhóm intent (Refund / Account help). |
| **Trục 3: Chỉ số kiểm soát Ngữ cảnh Bàn giao** | *Về chỉ số:* Chỉ đo Median Resolution Time (W1) sẽ bỏ lọt các ca kẹt lâu (long-tail lag) khi bot chuyển ca sang người mà nhân viên thiếu ngữ cảnh. Nhân viên CS con người rất ức chế khi nhận ca chuyển tiếp mà khách hàng phải khai báo lại từ đầu.<br>*Đề xuất:* Bổ sung chỉ số đo độ đầy đủ của ngữ cảnh bàn giao (Handoff Completeness) và đo thời gian phân vị P90 thay vì chỉ đo trung vị. | Dẫn chứng từ khảo sát AS-IS: 35% ca chuyển tiếp thiếu tóm tắt khiến khách hàng bức xúc và thời gian xử lý ca bị kéo dài tới 45 phút. | Nhóm bạn đã bổ sung chỉ số `W2` (P90 resolution reduction ≥10%) và `W3` (Handoff context completeness ≥98%) vào Dashboard v2. |
| **Trục 4: Tiêu chí Gate định lượng** | *Về Lộ trình:* Lộ trình 30–60–90 ngày ban đầu của nhóm bạn còn mang tính danh sách đầu việc hành chính.<br>*Đề xuất:* Biến mỗi giai đoạn thành một Cổng quyết định (Decision Gate) có tiêu chí định lượng (ví dụ: QA pass ≥95%, 0 Critical Error) và quy định rõ hành động khi trượt Gate. | Bài học thực tế: Việc chuyển giai đoạn phải dựa trên bằng chứng dữ liệu đạt chuẩn an toàn, không thể chuyển pha chỉ vì hết thời gian trên lịch. | Nhóm bạn đã bổ sung đầy đủ Gate Criteria và Failure Actions vào Lộ trình và sơ đồ Mermaid trong README/Memo. |

---

## 4. Tổng kết Hai Thay Đổi Cốt Lõi Được Chốt Sau Phiên Phản Biện

Sau phiên kiểm tra chéo 2 chiều, Team A đã chính thức tích hợp **2 thay đổi nền tảng** vào sản phẩm nộp cuối cùng:

1. **Thay đổi 1 — Lột xác Hệ thống Chỉ số từ Bề nổi sang Quyết định:**
   - *Trước phản biện (v1):* Chỉ đo lượt login (45 lượt/tuần) và số câu prompt (80 câu/ngày).
   - *Sau phản biện (v2):* Chuyển sang đo lường thời gian thực từ log Jira Service Management (`M03_CYCLE_TIME`: rút ngắn từ 42p xuống ≤15p) và tỷ lệ câu trả lời có trích nguồn chính xác được kiểm toán độc lập (`M04_CITE_ACC`: tăng từ 18.5% lên ≥95%).
2. **Thay đổi 2 — Chuyển từ Giải pháp Đào tạo sang Kiến trúc Tin cậy & Bảo vệ Người dùng:**
   - *Trước phản biện (v1):* Đề xuất mở thêm 2 lớp đào tạo prompt và thi đua tặng voucher.
   - *Sau phản biện (v2):* Xây dựng **Kiến trúc tin cậy (Trust Architecture)** bắt buộc hiển thị 100% link văn bản gốc, số hiệu điều khoản, số trang và ngày hiệu lực; bổ sung nút Báo lỗi 1-click; ban hành chính sách **Miễn trừ trách nhiệm cho nhân viên khi thực hiện đúng bước kiểm chứng nguồn**.

---

## 5. Xác nhận Nghiệm thu Hai Bên

* **Đại diện Nhóm Anh Em Siêu Nhân:**  
  *(Đã ký xác nhận)*  
  **Bùi Thị Ngọc Trân** — Trưởng nhóm `Anh Em Siêu Nhân` (Track 01)  
  *Ghi chú xác nhận:* "Xác nhận Team A (Kim Chi) đã hoàn thành xuất sắc phiên phản biện chéo 2 chiều; các ý kiến đóng góp của Kim Chi về phân tầng rủi ro Segments và chỉ số Handoff Completeness đã giúp nhóm chúng tôi hoàn thiện Dashboard v2 đạt chuẩn tối đa."

* **Đại diện Team A:**  
  *(Đã ký xác nhận)*  
  **Thân Thị Kim Chi** — Sinh viên thực hiện `Day26_Track01_Team_A`  
  *Ghi chú xác nhận:* "Chân thành cảm ơn các đóng góp xác đáng từ nhóm Anh Em Siêu Nhân; nhờ các phản biện về bẫy Activity metrics và điểm nghẽn Desire mà bản v2 của tôi đã hoàn toàn lột xác thành một Action Dashboard thực thụ."
