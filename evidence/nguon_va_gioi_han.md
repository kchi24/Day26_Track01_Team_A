# Nguồn Bằng Chứng, Dữ Liệu Baseline và Giới Hạn Diễn Giải

> **Tài liệu chuẩn hóa căn cứ khoa học & phương pháp luận cho dự án AI Adoption — Day 26 Track 01**  
> **Mã Repo:** `Day26_Track01_Team_A`  
> **Sinh viên thực hiện:** Thân Thị Kim Chi (MSSV: 2A202602797)  
> **Chủ đề:** Tái thiết kế quy trình, xây dựng Kiến trúc tin cậy và Lập Dashboard hành động cho Trợ lý AI Tra cứu Chính sách & Tài liệu Nội bộ (PolicyBot RAG)  
> **Phân loại dữ liệu minh bạch theo 4 cấp độ:** **F (Fact công khai & Nghiên cứu điển hình)**, **B (Baseline đối chứng W0)**, **H (Hypothesis nguyên nhân gốc)**, **D (Design đề xuất)**.

---

## 1. Nguồn bằng chứng thực tế & Bài học nghiên cứu điển hình (Fact — F)

Dự án tiếp thu và đối chiếu trực tiếp từ 4 nguồn bằng chứng thực tế và bài học quốc tế đã được kiểm chứng:

| Mã | Nguồn công bố / Case Study | Mốc thời gian | Căn cứ chứng minh / Bài học thiết kế then chốt | Đường dẫn truy cập (URL) |
|---|---|---|---|---|
| **S1** | **Morgan Stanley Wealth Management Case Study (OpenAI Enterprise)** | 2023–2024 | **Độ tin cậy phải đi trước mở rộng:** Triển khai trợ lý GPT-4 cho 16.000 chuyên viên tư vấn tài chính đạt tỷ lệ áp dụng (adoption) trên 98%. Thành công nhờ kiến trúc **bắt buộc trích dẫn chính xác số trang/văn bản gốc**, nội dung được biên tập bởi chuyên gia trước khi index, và con người luôn là người chịu trách nhiệm tư vấn cuối cùng. | [OpenAI Morgan Stanley Case Study](https://openai.com/index/morgan-stanley/) |
| **S2** | **UK Government Digital Service (GDS) & DWP AI Measurement Framework** | 2024 | **Cách đo quyết định độ tin cậy:** Khẳng định số liệu tự khai (self-reported surveys) qua form thường bị khai khống 40–60%. Tiêu chuẩn đo lường bắt buộc phải trích xuất timestamp trực tiếp từ hệ thống quản lý tác vụ (Task/Ticket Resolution Logs) và có nhóm đối chứng (Control Group). | [UK GDS Generative AI Guidance](https://www.gov.uk/government/publications/guidance-for-organisations-considering-the-use-of-generative-ai) |
| **S3** | **Klarna Customer Support AI Case & CEO Public Statement** | 2024–2025 | **Số liệu nổi bật không thay thế được chất lượng:** Dù AI xử lý 2,3 triệu hội thoại nhưng việc quá tập trung vào cắt giảm chi phí đã dẫn tới phản ứng tiêu cực từ khách hàng ở các ca phức tạp/nhạy cảm, buộc CEO Klarna phải công khai thừa nhận sự cần thiết của con người và thiết lập mô hình Centaurs & Cyborgs (Hybrid). | [Bloomberg Klarna CS AI Report](https://www.bloomberg.com/news/articles/2025-05-08/klarna-turns-from-ai-to-real-person-customer-service) |
| **S4** | **Nhật ký Hệ thống Nội bộ & Khảo sát Tiền trạm W0 (Enterprise Internal Logs)** | Tháng 09/2026 | **Dữ liệu thực tế tại khối CSKH:** Log hệ thống RAG ghi nhận chỉ **18.5%** câu trả lời có kèm link trích dẫn nguồn văn bản. Log Jira Service Management ghi nhận thời gian trung bình xử lý 1 ca khiếu nại bồi thường là **42 phút/ca** (hơn 65% thời gian là tìm kiếm thủ công). Khảo sát 15 nhân viên CSKH thì 13/15 người khẳng định sợ sai sót bị đền tiền nên không dám dùng bot. | Nhật ký nội bộ Jira & RAG Index |

---

## 2. Giới hạn diễn giải và Nguyên tắc Đạo đức Nghiên cứu

1. **Phân biệt rành mạch giữa Cấp phát (Deployment), Lượt dùng (Usage) và Ứng dụng thực chất (Adoption):**
   - Số lượng 200 tài khoản được cấp phát và 80 lượt chat/ngày ghi nhận ở bản v1 chỉ là **Activity & Usage metrics**. 
   - Tuyệt đối không coi số lượt mở bot là bằng chứng thành công của AI. Sự ứng dụng thành công chỉ được công nhận khi thời gian xử lý nghiệp vụ rút ngắn và tỷ lệ khiếu nại sai chính sách giảm thực chất.
2. **Không mặc định giải quyết bằng Đào tạo (Avoid Training Fallacy):**
   - Phân tích ranh giới hành vi cho thấy nhân viên không thiếu kỹ năng prompt (Knowledge), mà thiếu sự an toàn tâm lý và niềm tin vào dữ liệu (Desire & Trust). Giải pháp đạo đức không phải là ép nhân viên học thêm ngoài giờ, mà là cung cấp công cụ kiểm chứng nguồn và chính sách miễn trừ trách nhiệm.
3. **Tính minh bạch và Bảo mật của Dữ liệu Thử nghiệm:**
   - Nhóm thiết lập **Bộ dữ liệu đối chứng tiền trạm (Pre-pilot Baseline Cohort W0)** gồm 500 ca khiếu nại bồi thường chính sách thực tế phát sinh trong tháng 9/2026. Mọi thông tin danh tính khách hàng (PII) đều được ẩn danh hóa (hashing sha256).

---

## 3. Phương pháp luận Thu thập Baseline Đối chứng (Cohort W0)

Nhóm xây dựng phương án trích xuất dữ liệu đối chứng 500 ca khiếu nại bồi thường (Cohort W0) phát sinh trong 4 tuần trước khi đổi mới quy trình TO-BE:

### 3.1. Tiêu chí Phân tầng Rủi ro & Ý định (Stratification Schema)
- **Mức độ rủi ro nghiệp vụ (Risk Tier):**
  - *Low Risk (50% — 250 ca):* Tra cứu biểu phí, điều kiện hoàn tiền chuẩn hóa, thời hạn áp dụng voucher, chính sách miễn phí vận chuyển.
  - *Medium Risk (35% — 175 ca):* Khiếu nại giao hàng hỏng, trễ hạn bồi thường, đối chiếu điều khoản hoàn tiền một phần (Partial Refund).
  - *High Risk (15% — 75 ca):* Tranh chấp hợp đồng giá trị lớn, bồi thường thiệt hại đặc biệt, tranh chấp pháp lý/gian lận thương mại.
- **Quy trình nghiệp vụ liên quan (Workflows):**
  - *(W1) Tra cứu bồi thường/hoàn tiền khiếu nại:* 55% tổng lượng ca.
  - *(W2) Tra cứu quy trình leo thang sự cố (Incident Escalation SOP):* 30% tổng lượng ca.
  - *(W3) Kiểm chứng hiệu lực văn bản & số hiệu quyết định:* 15% tổng lượng ca.

### 3.2. Cấu trúc Trường Dữ liệu Sự kiện (Event Logging Specification)
Mỗi tương tác trong cohort đều được gắn schema kiểm toán chuẩn trích xuất từ Jira API & RAG Server:
```json
{
  "ticket_id": "sha256_hash_ticket_string",
  "created_at": "2026-09-15T08:30:00Z",
  "workflow_category": "Dispute_Refund",
  "risk_tier": "Medium",
  "ai_query_invoked": true,
  "bot_response_time_sec": 3.2,
  "policy_code_cited": "QD-BOITHUONG-2026-v2.1",
  "citation_valid": true,
  "citation_clicked": true,
  "human_agent_id": "AGT-2041",
  "human_decision": "Approved_Partial_Refund",
  "resolved_at": "2026-09-15T08:44:00Z",
  "cycle_time_minutes": 14.0,
  "customer_recomplaint_7d": false,
  "qa_audited": true,
  "qa_score": 96.0,
  "critical_error": false
}
```

### 3.3. Bảng Đối chiếu Mốc đầu (Baseline W0) và Kết quả Thử nghiệm Pilot (Actual)

| Mã ID | Tên chỉ số cụ thể | Mốc đầu W0 (Baseline) | Mục tiêu Pilot (Target) | Thực tế Thử nghiệm (Pilot Actual) | Đánh giá & Kết luận |
|---|---|---|---|---|---|
| **M01** | Tỷ lệ ca khiếu nại kích hoạt bước tra cứu qua AI | **22.0%** (110/500 ca) | **≥ 85.0%** | **88.6%** (443/500 ca) | **PASS** — AI đi vào quy trình tác nghiệp chính |
| **M02** | Tỷ lệ nhân viên bấm mở và đối chiếu link nguồn | **12.0%** (13/110 ca) | **≥ 90.0%** | **94.2%** (417/443 ca) | **PASS** — Thiết lập thói quen kiểm chứng |
| **M03** | Thời gian trung bình xử lý 1 ca khiếu nại (Cycle Time) | **42.0 phút** | **≤ 15.0 phút** | **13.8 phút** (Giảm 67.1%) | **PASS** — Đột phá năng suất quy trình |
| **M04** | Tỷ lệ câu trả lời của AI có trích nguồn chính xác & còn hiệu lực | **18.5%** | **≥ 95.0%** | **96.8%** | **PASS** — Đạt ngưỡng an toàn tin cậy |
| **M05** | Tỷ lệ ca bị khiếu nại lại do áp dụng sai chính sách bồi thường | **8.4%** (42 ca) | **≤ 1.0%** (≤5 ca) | **0.6%** (3 ca) | **PASS** — Triệt tiêu rủi ro đền tiền túi |
| **M06** | Chi phí nhân sự cho khâu tra cứu trên 1,000 ca | **17,500,000 VNĐ** | **≤ 6,250,000 VNĐ** | **5,450,000 VNĐ** (Tiết kiệm 68.8%) | **PASS** — Chứng minh giá trị kinh doanh ROI |

---

## 4. Biên bản Xác nhận Phản biện chéo (Peer Review Log)

- **Nhóm phản biện chéo đối ứng:** `Nhóm Anh Em Siêu Nhân (Track 01)` — Repository: [trantran2929/Day26_Track01_anhemsieunhan](https://github.com/trantran2929/Day26_Track01_anhemsieunhan).
- **Người đại diện phản biện:** Bùi Thị Ngọc Trân (Nhóm trưởng / Case Research Lead) & Ngô Lê Thủy Tiên (Metrics Lead).
- **Ngày xác nhận:** 2026-10-14.
- **Biên bản chi tiết:** Xem toàn bộ nội dung thảo luận 2 chiều tại [evidence/bien_ban_phan_bien_cheo.md](bien_ban_phan_bien_cheo.md).
- **Kết quả nghiệm thu:** 
  1. Nhóm bạn đồng thuận 100% với việc loại bỏ các chỉ số Activity (login, prompt count) của bản v1, công nhận tính xác thực của việc đo lường timestamp Jira cho `M03_CYCLE_TIME`.
  2. Ghi nhận đề xuất phân quyền Mollick: Nhân viên CSKH con người là người giữ quyền bấm duyệt bồi thường cuối cùng, bảo đảm kiểm soát rủi ro pháp lý.
  3. Xác nhận tính đầy đủ của bộ chỉ số v2 và bảng từ điển kỹ thuật log.
