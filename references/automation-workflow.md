# Automation Workflow Cho GMap Lead Finder — BBOTech

Tài liệu này mô tả cách nâng cấp quy trình prospecting bằng automation hợp pháp. Không hướng dẫn scraping trái phép Google Maps. Chỉ dùng **Google Places API**, dữ liệu người dùng cung cấp hợp pháp, hoặc thông tin doanh nghiệp công khai được phép xử lý.

## Nguyên Tắc Bắt Buộc

- Không scrape Google Maps trái phép.
- Không vượt qua giới hạn bảo mật, CAPTCHA, rate limit hoặc điều khoản nền tảng.
- Ưu tiên Google Places API cho dữ liệu địa điểm.
- Chỉ lưu thông tin doanh nghiệp cần thiết cho prospecting.
- Không thu thập dữ liệu cá nhân nhạy cảm.
- Không gửi outreach hàng loạt không cá nhân hóa.
- Cung cấp cách dừng liên hệ khi khách không muốn nhận thông tin.
- Ghi rõ nguồn dữ liệu và thời điểm lấy dữ liệu.

## Luồng Tổng Quát

```text
Keyword input
→ Google Places API
→ Google Sheets
→ AI scoring
→ AI message suggestion
→ CRM update
→ Sales review
→ Outreach cá nhân hóa
```

Luồng đúng nên có bước **Sales review** trước khi liên hệ. AI chỉ đề xuất điểm và tin nhắn, không tự động spam.

## Field Nên Lưu

- `batch_id`
- `source_keyword`
- `place_id`
- `business_name`
- `category`
- `area`
- `formatted_address`
- `phone_public`
- `website`
- `google_maps_url`
- `rating`
- `user_ratings_total`
- `business_status`
- `opening_hours`
- `social_link_public`
- `email_public`
- `website_status`
- `observed_pain_point`
- `recommended_service`
- `website_score`
- `customer_care_score`
- `fit_score`
- `contact_score`
- `opportunity_score`
- `lead_score`
- `priority`
- `message_suggestion`
- `follow_up_status`
- `owner_sales`
- `last_checked_at`
- `data_source`

## Workflow Nếu Dùng n8n

**Mục tiêu:** nhập keyword, lấy lead qua Google Places API, ghi vào Google Sheets, dùng AI chấm điểm và gợi ý message, sau đó cập nhật CRM.

Node gợi ý:

1. **Manual Trigger hoặc Form Trigger**
   - Input: ngành, khu vực, dịch vụ muốn bán, số lead mong muốn.

2. **Set / Function Node**
   - Tạo danh sách keyword từ input.
   - Ví dụ: `spa quận 3`, `spa chăm sóc da quận 3`, `clinic quận 3` nếu phù hợp.

3. **HTTP Request - Google Places Text Search API**
   - Gửi keyword tới Google Places API.
   - Lấy các field cần thiết theo quyền API.
   - Tuân thủ quota và chính sách Google.

4. **Split In Batches**
   - Xử lý từng place.
   - Tránh gọi API dồn dập.

5. **HTTP Request - Place Details API**
   - Lấy chi tiết như phone, website, business status, rating, review count.
   - Không lấy dữ liệu ngoài phạm vi API cho phép.

6. **Google Sheets - Upsert Row**
   - Upsert theo `place_id` để tránh trùng.
   - Ghi trạng thái ban đầu là `New`.

7. **AI Node**
   - Input: dữ liệu lead.
   - Output: điểm theo 5 nhóm, lead score, dịch vụ phù hợp, pain point, message suggestion.
   - Prompt phải yêu cầu ghi rõ giả định nếu thiếu dữ liệu.

8. **Google Sheets - Update Row**
   - Cập nhật score, priority, message suggestion.
   - Nếu score >= 80 và đủ thông tin, trạng thái có thể là `Checked`.

9. **CRM Node**
   - Tạo/cập nhật lead trong CRM.
   - Gán owner sales.

10. **Human Review**
   - Sales kiểm tra trước khi gửi tin nhắn.
   - Không tự động gửi hàng loạt.

Chống spam trong n8n:

- Không nối thẳng AI suggestion sang node gửi email/Zalo hàng loạt.
- Bắt buộc có bước review hoặc checkbox `approved_for_outreach`.
- Giới hạn số lead liên hệ mỗi ngày theo owner sales.
- Tự động bỏ qua `Do Not Contact`.

## Workflow Nếu Dùng Python

**Mục tiêu:** chạy batch có kiểm soát, lấy dữ liệu qua API hợp pháp, ghi vào Google Sheets/CSV, gọi AI để scoring.

Luồng đề xuất:

1. Đọc input từ file YAML/CSV:

```text
industry: spa
area: quận 3
service: Chatbot AI
keywords:
  - spa quận 3
  - spa chăm sóc da quận 3
  - thẩm mỹ viện quận 3
limit_per_keyword: 20
```

2. Gọi Google Places API:
   - Text Search để lấy danh sách địa điểm.
   - Place Details để lấy field chi tiết.
   - Lưu `place_id` để chống trùng.

3. Chuẩn hóa dữ liệu:
   - Normalize tên, khu vực, phone, website, rating, review count.
   - Gắn `data_source = Google Places API`.

4. Kiểm tra website ở mức cơ bản nếu được phép:
   - Chỉ kiểm tra URL có tồn tại, HTTP status, tiêu đề trang nếu cần.
   - Không crawl sâu hoặc scrape nội dung trái phép.

5. Gọi AI scoring:
   - Truyền dữ liệu lead.
   - Yêu cầu JSON output: điểm 5 nhóm, tổng điểm, dịch vụ phù hợp, message suggestion.

6. Ghi Google Sheets/CSV:
   - Upsert theo `place_id`.
   - Không ghi đè ghi chú sales nếu đã có.

7. Xuất báo cáo:
   - Top P1/P2 theo ngành/khu vực.
   - Lead thiếu dữ liệu cần kiểm tra thủ công.

Guardrail Python:

- Có `dry_run = true` mặc định.
- Có rate limit.
- Có log nguồn dữ liệu.
- Có danh sách loại trừ `Do Not Contact`.
- Không tự gửi outreach nếu chưa có phê duyệt.

## Workflow Nếu Dùng Google Apps Script

**Mục tiêu:** chạy ngay trong Google Sheets để tạo keyword, gọi API, cập nhật score và trạng thái.

Luồng đề xuất:

1. Sheet `Input`
   - Cột: ngành, khu vực, dịch vụ, keyword, limit, batch_id.

2. Script `buildKeywords()`
   - Tạo keyword từ ngành/khu vực.
   - Ghi vào sheet `Keywords`.

3. Script `fetchPlaces()`
   - Gọi Google Places API bằng API key được lưu trong Script Properties.
   - Ghi kết quả thô vào sheet `Raw Places`.

4. Script `normalizePlaces()`
   - Chuẩn hóa dữ liệu vào sheet `Leads`.
   - Chống trùng bằng `place_id`.

5. Script `scoreLeads()`
   - Gọi AI API hoặc dùng rule-based scoring cơ bản.
   - Cập nhật điểm và mức ưu tiên.

6. Script `suggestMessages()`
   - Tạo message suggestion theo ngành, pain point, dịch vụ.
   - Không tự gửi tin.

7. Sales review
   - Sales lọc P1/P2, chỉnh message, sau đó liên hệ thủ công hoặc qua CRM có kiểm soát.

Guardrail Apps Script:

- Không hard-code API key trong code.
- Không trigger gửi hàng loạt.
- Có cột `approved_for_outreach`.
- Có cột `do_not_contact`.
- Có quota/rate limit rõ.

## AI Scoring Prompt Gợi Ý

```text
Bạn là BBOTech GMap Lead Finder.
Hãy chấm điểm lead theo framework 100 điểm:
- Website & hiện diện online: 25
- Nhu cầu chăm sóc khách hàng: 20
- Mức phù hợp với dịch vụ BBOTech: 25
- Khả năng liên hệ: 15
- Tín hiệu khẩn cấp/cơ hội: 15

Chỉ dùng dữ liệu được cung cấp. Nếu thiếu dữ liệu, ghi "chưa có dữ liệu".
Không bịa thông tin, không bịa case study.
Đề xuất dịch vụ phù hợp và một tin nhắn tiếp cận cá nhân hóa, lịch sự, không spam.
```

JSON output gợi ý:

```json
{
  "website_score": 0,
  "customer_care_score": 0,
  "fit_score": 0,
  "contact_score": 0,
  "opportunity_score": 0,
  "lead_score": 0,
  "priority": "P2 Tiềm năng",
  "recommended_service": "Chatbot AI",
  "observed_pain_point": "chưa có dữ liệu",
  "message_suggestion": "..."
}
```

## Cách Tránh Spam Và Cá Nhân Hóa Outreach

- Mỗi lead phải có `observed_pain_point` trước khi liên hệ.
- Không gửi cùng một nội dung cho cả batch.
- Nhắc rõ lý do liên hệ: biết đến doanh nghiệp qua thông tin công khai trên Google Maps.
- Không nhắc review tiêu cực theo kiểu gây áp lực.
- Không hứa kết quả không có căn cứ.
- Luôn có câu cho phép khách từ chối: "Nếu chưa phù hợp, anh/chị báo em để em không làm phiền thêm."
- Theo dõi tần suất: tối đa 2-3 follow-up nhẹ, sau đó chuyển `Nurture` hoặc `Do Not Contact`.

## Khi Nào Nên Dừng Automation

- API trả lỗi quota/rate limit.
- Dữ liệu thiếu hoặc nghi ngờ không hợp pháp.
- Lead yêu cầu không liên hệ.
- Message suggestion quá chung chung, chưa cá nhân hóa.
- Không có sales review trước outreach.
