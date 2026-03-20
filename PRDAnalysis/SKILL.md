---
name: PRD Analysis
description: 11-dimension PRD quality scoring framework with weighted subcriteria, evidence-based assessment, and structured JSON output.
version: 4.0.0
status: active
triggers:
  - "review PRD"
  - "analyze PRD"
  - "score PRD"
  - "PRD quality"
  - "đánh giá PRD"
---

# PRD Analysis Skill

Perform a comprehensive, evidence-based quality assessment of a Product Requirements Document (PRD) using an 11-dimension scoring framework. Each dimension is scored independently with weighted subcriteria, producing a structured JSON result with a total weighted score and verdict.

## How to Use

When the user asks you to analyze/review/score a PRD:

1. Read the PRD content (from file, URL, or pasted text)
2. Run **Structure Validation** first (gate check)
3. Score all **11 dimensions** in one pass
4. Calculate the **weighted total score**
5. Return structured results in the JSON output format below

---

## Step 1: Structure Validation (Gate Check)

Before scoring, verify the PRD contains these 4 required sections (keyword match, supports Vietnamese):

| Required Section | Keywords to Match |
|-----------------|-------------------|
| Problem Statement | `problem statement`, `vấn đề`, `bối cảnh`, `context`, `the customer problem`, `mục tiêu` |
| User Stories | `user stories`, `user story`, `use cases`, `use case`, `kịch bản sử dụng`, `chân dung người dùng`, `user requirements`, `hành trình người dùng` |
| Acceptance Criteria | `acceptance criteria`, `ac`, `tiêu chí chấp nhận`, `điều kiện nghiệm thu`, `yêu cầu chức năng`, `functional requirements`, `given/when/then` |
| Success Metrics | `success metrics`, `kpi`, `thước đo thành công`, `tiêu chí thành công`, `đo lường`, `north star`, `metrics`, `chỉ số thành công` |

**Short keywords (≤3 chars like "ac")**: use word-boundary matching to avoid false positives (e.g., "ac" in "compact").

If any section is missing → return `{ structureFailed: true, missingSections: [...] }` and STOP.

---

## Step 2: Score All 11 Dimensions

### Dimension Definitions

Each dimension has an ID, title, weight (% of total), core question, and 5-6 subcriteria with individual weights.

---

#### D1 — Xác thực vấn đề (Problem Validation) · Weight: 12%
**Core question:** Vấn đề có thực, có quy mô và đúng thời điểm?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D1.1 | Mức độ cụ thể của vấn đề | 20% | Vấn đề được phát biểu dưới dạng hành vi/nỗi đau quan sát được, không phải nhu cầu giả định. |
| D1.2 | Chất lượng bằng chứng | 25% | Được xác thực bằng dữ liệu sơ cấp (phỏng vấn, phân tích, khảo sát). |
| D1.3 | Mức độ nghiêm trọng & tần suất | 20% | Được lượng hóa: xảy ra bao lâu, đau đớn ra sao, bao nhiêu người bị ảnh hưởng. |
| D1.4 | Lý do thời điểm | 15% | Rõ ràng "tại sao bây giờ?" — thay đổi thị trường, quy định, công nghệ mới. |
| D1.5 | Giải pháp thay thế & cách khắc phục | 20% | Các giải pháp hiện tại được ghi nhận kèm nhược điểm chứng minh bằng dữ liệu. |

---

#### D2 — Hiểu biết người dùng (User Understanding) · Weight: 10%
**Core question:** Chúng ta có thực sự hiểu sâu đối tượng mục tiêu?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D2.1 | Chất lượng Persona | 25% | Persona dựa trên nghiên cứu với nhân khẩu học, hành vi, động lực. |
| D2.2 | Việc cần hoàn thành (JTBD) | 25% | Các công việc chức năng, cảm xúc và xã hội với kết quả đo lường được. |
| D2.3 | Hành trình người dùng | 20% | Hành trình từ đầu đến cuối được lập bản đồ với rủi ro bỏ cuộc ở mỗi giai đoạn. |
| D2.4 | Chi phí & rào cản chuyển đổi | 15% | Mức độ phụ thuộc vào giải pháp hiện tại được lượng hóa. Lộ trình chuyển đổi rõ ràng. |
| D2.5 | Tiếp cận & hòa nhập | 15% | Phủ sóng thiết bị, tiêu chuẩn WCAG, nhu cầu bản địa hóa. |

---

#### D3 — Giải pháp & phạm vi (Solution & Scope) · Weight: 12%
**Core question:** Giải pháp có rõ ràng, phân chia ưu tiên và có ranh giới MVP?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D3.1 | Định nghĩa tính năng | 25% | Mỗi tính năng có mô tả, giá trị người dùng và lý do liên kết với JTBD. |
| D3.2 | Ranh giới MVP | 25% | Phạm vi IN vs. OUT rõ ràng. MVP xác thực giả thuyết cốt lõi. |
| D3.3 | Phân chia ưu tiên | 20% | MoSCoW hoặc tương đương. Ưu tiên theo tác động × công sức. |
| D3.4 | Lộ trình & phân giai đoạn | 15% | Các giai đoạn MVP → V1 → V2 với mốc thời gian và phụ thuộc. |
| D3.5 | Truy xuất nguồn gốc | 15% | Chuỗi: Tính năng → User Story → Nhu cầu Persona → Mục tiêu kinh doanh. |

---

#### D4 — Yêu cầu & tiêu chí chấp nhận (Requirements & AC) · Weight: 12%
**Core question:** Engineering có thể build và QA có thể verify từ spec này không?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D4.1 | Yêu cầu chức năng | 25% | User stories với Given/When/Then. Cụ thể, kiểm thử được, không mơ hồ. Tách biệt rõ ràng tác vụ Frontend (UX) và Backend (Technical Tasks). |
| D4.2 | Yêu cầu phi chức năng | 20% | Mục tiêu hiệu suất, độ tin cậy, khả năng mở rộng với con số chính xác. |
| D4.3 | Tiêu chí chấp nhận | 25% | Đạt/không đạt đo lường được cho mỗi tính năng. Bao gồm giá trị biên. |
| D4.4 | Phải / Nên / Có thể | 15% | Mỗi yêu cầu được gắn nhãn rõ ràng. |
| D4.5 | Định nghĩa hoàn thành | 15% | Checklist chung: code xong, test, tài liệu, review, triển khai. |

---

#### D5 — Phù hợp mô hình kinh doanh (BMC Fit) · Weight: 10%
**Core question:** PRD có phù hợp với Business Model Canvas (BMC) của công ty không?

> **Note to skill users:** D5 compares the PRD against the company's BMC. If you have a company BMC, paste it as context. If not, evaluate the PRD's internal business model coherence.

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D5.1 | Phù hợp phân khúc khách hàng | 25% | PRD nhắm đúng phân khúc khách hàng đã định nghĩa trong BMC. |
| D5.2 | Nhất quán đề xuất giá trị | 25% | Giải pháp củng cố value propositions hiện có, không mâu thuẫn với định vị chiến lược. |
| D5.3 | Tận dụng nguồn lực & hoạt động chính | 20% | PRD tận dụng key resources hiện có. Không yêu cầu xây dựng năng lực hoàn toàn mới mà không có lý do. |
| D5.4 | Phù hợp kênh & quan hệ khách hàng | 15% | Kênh phân phối và mô hình quan hệ khách hàng phù hợp với BMC. |
| D5.5 | Đóng góp doanh thu & hiệu quả chi phí | 15% | PRD đóng góp vào revenue streams của BMC. Chi phí triển khai hợp lý. |

---

#### D6 — Khả thi kỹ thuật (Technical Feasibility) · Weight: 8%
**Core question:** Có thể xây dựng với nguồn lực hiện tại?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D6.1 | Tổng quan kiến trúc | 25% | Thiết kế hệ thống cấp cao, lựa chọn công nghệ có lý do. Chốt rõ chiến lược lưu trữ trạng thái (Local vs Server sync). |
| D6.2 | Phụ thuộc & điểm lỗi đơn | 25% | Dịch vụ bên ngoài được xác định. SPOF được chỉ ra kèm phương án dự phòng. |
| D6.3 | Khả năng mở rộng & hạ tầng | 20% | Dự phóng tải được xác định. Chiến lược mở rộng rõ ràng. |
| D6.4 | Rủi ro kỹ thuật | 15% | Sổ rủi ro với khả năng xảy ra/tác động. Các hạng mục Spike/POC được xác định. |
| D6.5 | Kế hoạch tích hợp | 15% | Hợp đồng API và đặc tả giao diện (trường dữ liệu, API mở, tần suất cập nhật) khi kết nối hệ thống ngoài. |

---

#### D7 — Trường hợp biên & xử lý lỗi (Edge Cases) · Weight: 8%
**Core question:** Điều gì xảy ra khi có sự cố?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D7.1 | Điều kiện biên | 25% | Giới hạn đầu vào, trạng thái rỗng/loading, phân trang (pagination/scroll), và luồng tương tác nhiều item cùng lúc được tài liệu hóa. |
| D7.2 | Trạng thái lỗi & phục hồi | 25% | Thông báo lỗi hiển thị cho người dùng. Logic thử lại. Suy giảm nhẹ nhàng. |
| D7.3 | Lỗi phụ thuộc bên ngoài | 25% | Hành vi khi API chậm/lỗi. Timeout và circuit-breaker. |
| D7.4 | Lạm dụng & sử dụng sai | 15% | Giới hạn tốc độ, chống spam, xử lý đầu vào độc hại. |
| D7.5 | Trường hợp biên dữ liệu | 10% | Tập dữ liệu rỗng, dữ liệu hỏng, vấn đề múi giờ/ngôn ngữ. |

---

#### D8 — Chỉ số & tiêu chuẩn đo lường (Metrics & Benchmarks) · Weight: 10%
**Core question:** Cách đo lường, so sánh và đưa ra quyết định?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D8.1 | Chỉ số North Star | 20% | Một chỉ số chính nắm bắt giá trị sản phẩm với baseline + mục tiêu. |
| D8.2 | Chỉ báo dẫn dắt | 20% | Tín hiệu sớm dự đoán thành công (tỷ lệ kích hoạt, thời gian đạt giá trị). |
| D8.3 | Chỉ báo trễ / KPI | 20% | 3-5 chỉ số kết quả: tiếp nhận, tương tác, giữ chân, doanh thu. |
| D8.4 | Tiêu chuẩn đối sánh | 20% | Tiêu chuẩn nội bộ + bên ngoài có nguồn trích dẫn. |
| D8.5 | Tiêu chí đánh giá & ngưỡng | 20% | Cổng go/no-go rõ ràng. Kế hoạch A/B test với ý nghĩa thống kê. |

---

#### D9 — Sẵn sàng ra thị trường (GTM Readiness) · Weight: 8%
**Core question:** Có thể ra mắt, bán và phát triển?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D9.1 | Định vị & thông điệp | 20% | Tuyên bố định vị rõ ràng. Bản đồ định vị cạnh tranh. |
| D9.2 | Chiến lược kênh | 20% | Kênh thu hút với CAC ước tính cho mỗi kênh. |
| D9.3 | Kế hoạch ra mắt | 20% | Triển khai theo giai đoạn. Chương trình beta. Checklist ra mắt. Tiêu chí rollback. |
| D9.4 | Định giá & đóng gói | 20% | Các gói giá. Phân tích cạnh tranh. Ranh giới miễn phí vs. trả phí. |
| D9.5 | Tiếp nhận & tăng trưởng | 20% | Quy trình onboarding. Mốc kích hoạt. Cơ chế giữ chân. |

---

#### D10 — Cấu trúc tài liệu (Document Structure) · Weight: 5%
**Core question:** Tài liệu PRD có được viết tốt và dễ sử dụng?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| D10.1 | Tính nhất quán cấu trúc | 25% | Các phần bắt buộc đầy đủ: Vấn đề, User Stories, AC, Chỉ số. |
| D10.2 | Hệ thống ID & truy xuất | 20% | ID duy nhất (US/FR). Tham chiếu chéo nhất quán, đặc biệt phải có link/bản đồ map tới các PRD có logic/giao diện liên quan. |
| D10.3 | Nhất quán thuật ngữ | 15% | Có bảng thuật ngữ. Cùng một thuật ngữ xuyên suốt. |
| D10.4 | Luồng logic & mạch lạc | 15% | Các phần xây dựng trên nhau. Phân cấp thông tin rõ ràng. |
| D10.5 | Kiểm tra đầy đủ | 15% | Tất cả phần tiêu chuẩn có mặt. Không có TBD chưa gán người phụ trách. |
| D10.6 | Hỗ trợ trực quan | 10% | Sơ đồ, wireframe, flowchart khi chúng mang lại sự rõ ràng. |

---

#### C — Tuân thủ & rủi ro (Compliance & Risk) · Weight: 5%
**Core question:** Các rủi ro tuân thủ, bảo mật và đạo đức đã được giải quyết?

| ID | Subcriteria | Weight | "Good" Standard |
|----|------------|--------|-----------------|
| C.1 | Quyền riêng tư dữ liệu & đồng ý | 20% | Bảo mật ngay từ thiết kế. Cơ chế đồng ý được chỉ định. |
| C.2 | Tuân thủ quy định | 20% | GDPR, HIPAA, PDPA, luật địa phương được ánh xạ vào tính năng. |
| C.3 | Yêu cầu bảo mật | 20% | Xác thực, mã hóa, mô hình mối đe dọa được chỉ định. |
| C.4 | Rủi ro đạo đức | 20% | Thiên lệch AI, công bằng, minh bạch được xem xét. |
| C.5 | Rủi ro vận hành | 20% | DR, quy trình ứng phó sự cố, giám sát được tài liệu hóa. |

---

## Step 3: Scoring Rules

### Scoring Rubric (per subcriteria)

| Score Range | Level | Description |
|-------------|-------|-------------|
| 96–100 | Xuất sắc | Production-ready, bao phủ toàn diện với bằng chứng mạnh mẽ. |
| 85–95 | Tốt | Nền tảng vững chắc, một số lỗ hổng cần khắc phục. |
| 70–84 | Cần cải thiện | Lỗ hổng đáng kể ảnh hưởng đến thực thi. |
| 50–69 | Yếu | Cần viết lại đáng kể trước khi phát triển. |
| 0–49 | Nghiêm trọng | Thiếu căn bản, cần bắt đầu lại. |

### Deduction Rules (per subcriteria)

Start each subcriteria at 100 points, then deduct:

| Deduction | When to Apply |
|-----------|--------------|
| −10 to −20 | Missing minor details or evidence not strong enough (→ 80–90) |
| −30 to −40 | Missing core information, weak reasoning (→ 60–70) |
| −50 to −70 | Not mentioned at all, or information is completely wrong/useless (→ 30–50) |
| −80 to −100 | Completely absent or seriously incorrect (→ 0–20) |

### Scoring Principles

1. **Evidence over assertion** — Data-backed claims score higher than vague statements.
2. **Specific over vague** — "p95 < 200ms" > "fast response".
3. **Verifiable** — If QA can't test it, it's not a requirement.
4. **Sources with dates count** — When citations include years (e.g., "WHO 2021", "Google Trends 2023-2025"), do NOT deduct for "missing timeframe".

### Weighted Total Score Calculation

```
totalScore = Σ (dimension_score × dimension_weight / 100) × (100 / Σ active_weights)
```

Where `dimension_score` is the weighted average of its subcriteria:
```
dimension_score = Σ (subcriteria_score × subcriteria_weight) / Σ subcriteria_weights
```

### Verdict Mapping

| Total Score | Verdict |
|-------------|---------|
| ≥ 96 | Xuất sắc — Production-ready |
| 85–95 | Tốt — Nền tảng vững |
| 70–84 | Cần cải thiện — Có lỗ hổng |
| 50–69 | Yếu — Cần viết lại |
| 0–49 | Nghiêm trọng — Bắt đầu lại |

---

## Step 4: Output Format

All output MUST be in **Vietnamese**. Return the following JSON structure:

```json
{
  "totalScore": 75,
  "verdict": "Cần cải thiện — Có lỗ hổng",
  "sections": [
    {
      "title": "D1 — Xác thực vấn đề",
      "score": 72,
      "status": "pass",
      "subcriteria": [
        {
          "id": "D1.1",
          "name": "Mức độ cụ thể của vấn đề",
          "score": 80,
          "comment": "Nhận xét ngắn gọn bằng tiếng Việt",
          "deductionReason": "Lý do cụ thể tại sao tiêu chí này bị trừ điểm",
          "wbs": "1.2.3",
          "evidence": ["Trích dẫn nguyên văn từ PRD chứng minh lý do trừ điểm"]
        }
      ],
      "praise": [
        {
          "praise": "Điểm tốt bằng tiếng Việt",
          "evidence": ["Trích dẫn nguyên văn từ PRD"]
        }
      ],
      "issues": [
        {
          "issue": "Vấn đề bằng tiếng Việt",
          "severity": "critical | major | minor | nice to have",
          "wbs": "1.2.3",
          "evidence": ["Trích dẫn nguyên văn từ PRD gốc — GIỮ NGUYÊN VĂN KHÔNG SỬA ĐỔI"]
        }
      ],
      "suggestions": ["Gợi ý cải thiện bằng tiếng Việt"]
    }
  ]
}
```

### Output Rules

- **All content MUST be in Vietnamese.**
- Each praise/issue MUST include an `evidence` array with verbatim quotes from the PRD.
- Do NOT fabricate evidence. Do NOT use "Không tìm thấy nội dung liên quan trong PRD."
- Score FAIRLY: acknowledge both strengths (praise) and weaknesses (issues).
- Each subcriteria with score < 100 MUST have `deductionReason` explaining WHY and HOW MUCH was deducted.
- `deductionReason` must be PROPORTIONAL to the deduction amount.
- `wbs` = the section number from the PRD's heading structure (e.g., 1.2.3), NOT dimension codes.
- Evidence in `issues` must be VERBATIM quotes from the original PRD for exact highlighting.
- Each dimension MUST have at least 2 issues (even high-scoring dimensions should have improvement areas).
- `status`: `pass` if score ≥ 70, `warning` if 50–69, `fail` if < 50.
