# BBOTech GMap Lead Finder Skill — Audit Report

## 1. Tổng Quan

- Skill hiện tại đang ở mức **khá sẵn sàng cho pilot nội bộ có kiểm soát**: đã có identity đúng, reference map rõ, playbook thủ công, framework scoring, template outreach, objection handling, Google Sheets template và automation planning hợp pháp.
- Chưa nên chạy test thật diện rộng ngay. Nên sửa một vòng nhỏ trước khi pilot để chuẩn hóa priority/status, bổ sung cột compliance trong sheet, và làm outreach template an toàn hơn.
- Điểm đánh giá tổng thể: **8/10**.

Lý do chưa đạt 9-10/10: không có lỗi nghiêm trọng, nhưng còn một số điểm lệch giữa `SKILL.md` và reference files có thể khiến AI tạo output không nhất quán khi sales dùng thật.

## 2. File Đã Kiểm Tra

- `README.md`
- `SKILL.md`
- `references/email-templates.md`
- `references/objection-handling.md`
- `references/customer-profiles.md`
- `references/gmap-search-playbook.md`
- `references/scoring-examples.md`
- `references/google-sheets-template.md`
- `references/automation-workflow.md`

## 3. Lỗi Nghiêm Trọng

Không phát hiện lỗi nghiêm trọng.

Đã kiểm tra:

- YAML frontmatter hợp lệ, `name: bbotech-gmap-lead-finder`.
- Không còn dấu vết `bbotech-consultant` theo nghĩa skill cũ.
- Không còn nội dung cũ về thu hồi công nợ, invoice, customer success sau bán hoặc command cũ như `/gap-khach`, `/thu-tien`, `/phan-tich`.
- Không có hướng dẫn bypass CAPTCHA, né chặn, vượt rate limit hoặc scraping Google Maps trái phép.
- Automation workflow nhấn mạnh Google Places API, dữ liệu hợp pháp, Sales review và chống spam.

## 4. Lỗi Trung Bình

### 4.1. Priority và follow-up status chưa nhất quán giữa `SKILL.md` và Google Sheets template

- File: `SKILL.md`, `references/google-sheets-template.md`, `references/scoring-examples.md`
- Vị trí/heading: `Module 2 - Chuẩn Hóa Dữ Liệu Lead`, `Quy Ước Trạng Thái Lead`, `Quy Ước Mức Ưu Tiên`
- Vấn đề: `SKILL.md` dùng mức ưu tiên kiểu "Nóng/tiềm năng/trung bình/chưa ưu tiên" và trạng thái tiếng Việt ngắn. Reference template dùng `P1 Lead nóng / P2 Tiềm năng / P3 Nurture / P4 Không ưu tiên` và trạng thái `New / Checked / Contacted / ... / Do Not Contact`.
- Vì sao nguy hiểm: AI có thể xuất hai hệ thống status khác nhau cho cùng một sheet, gây khó filter và khó đồng bộ CRM.
- Đề xuất xử lý: Chuẩn hóa toàn repo theo hệ `P1/P2/P3/P4` và trạng thái tiếng Anh trong `google-sheets-template.md`. `SKILL.md` nên ghi rõ đây là chuẩn chính.

### 4.2. Google Sheets template thiếu một số cột compliance quan trọng

- File: `references/google-sheets-template.md`, đối chiếu `references/automation-workflow.md`
- Vị trí/heading: `Bảng Cột Chuẩn`, `Field Nên Lưu`, `Guardrail Apps Script`
- Vấn đề: Automation workflow có nhắc `data_source`, `last_checked_at`, `approved_for_outreach`, `do_not_contact`, nhưng Google Sheets template chưa đưa các cột này vào bảng chuẩn.
- Vì sao nguy hiểm: Khi pilot thật, sales/ops thiếu cột để chứng minh nguồn dữ liệu, thời điểm kiểm tra, trạng thái được duyệt outreach và yêu cầu không liên hệ.
- Đề xuất xử lý: Bổ sung tối thiểu `Data source`, `Last checked at`, `Approved for outreach`, `Do not contact` vào template chính.

### 4.3. Danh sách phản đối trong `SKILL.md` và reference không khớp hoàn toàn

- File: `SKILL.md`, `references/objection-handling.md`
- Vị trí/heading: `Module 6 - Xử Lý Phản Đối Cho Lead Từ Google Maps`
- Vấn đề: `SKILL.md` liệt kê "Bên tôi nhỏ, chưa cần AI", "Đang bận, gọi lại sau", "Bên tôi có người làm marketing rồi"; reference dùng biến thể "Bên tôi nhỏ lắm", "Tôi đang bận", "Bên tôi có team marketing rồi", đồng thời có thêm "Tôi không quan tâm AI".
- Vì sao nguy hiểm: Không quá nghiêm trọng, nhưng AI có thể không map đúng phản đối nếu người dùng gọi đúng cụm trong `SKILL.md`.
- Đề xuất xử lý: Đồng bộ danh sách 10-11 phản đối trong cả hai file, giữ cùng wording hoặc thêm alias.

### 4.4. `Contact Score` có thể bị hiểu là càng nhiều contact càng tốt

- File: `SKILL.md`, `references/google-sheets-template.md`, `references/scoring-examples.md`
- Vị trí/heading: `Module 3 - Chấm Điểm Lead`, `Framework 100 Điểm`
- Vấn đề: Hướng dẫn hiện tại cộng điểm cao khi có số điện thoại + website + fanpage/email. Nếu đọc máy móc, AI/sales có thể nghĩ cần thu thập càng nhiều contact càng tốt.
- Vì sao nguy hiểm: Có thể lệch khỏi nguyên tắc tối thiểu hóa dữ liệu và chỉ dùng thông tin doanh nghiệp công khai.
- Đề xuất xử lý: Đổi framing thành "có kênh liên hệ doanh nghiệp công khai, rõ nguồn, đủ để liên hệ đúng phép" thay vì tối đa hóa số kênh.

### 4.5. Điểm website có thể hơi ưu ái lead không có website

- File: `SKILL.md`, `references/scoring-examples.md`
- Vị trí/heading: `Module 3 - Chấm Điểm Lead`
- Vấn đề: "Không có website: +25" có thể đẩy một lead lên P2/P1 dù rating/review/fit chưa đủ mạnh.
- Vì sao nguy hiểm: Sales có thể ưu tiên lead "thiếu website" nhưng chưa chắc có nhu cầu hoặc khả năng mua.
- Đề xuất xử lý: Thêm guardrail: không có website là tín hiệu mạnh cho WaaS, nhưng không tự động thành lead nóng nếu review thấp, ngành không fit hoặc contact yếu.

### 4.6. Outreach template first-touch chưa luôn có câu opt-out rõ

- File: `references/email-templates.md`
- Vị trí/heading: `Tin Nhắn Zalo Tiếp Cận Lần Đầu`, `Email Cold Outreach`, `Script Gọi Điện 30 Giây`
- Vấn đề: Một số template đầu tiên lịch sự và có ngữ cảnh, nhưng chưa luôn có câu kiểu "Nếu chưa phù hợp, anh/chị báo em để em không làm phiền thêm".
- Vì sao nguy hiểm: Cold outreach dễ bị cảm nhận là làm phiền nếu thiếu quyền từ chối rõ.
- Đề xuất xử lý: Thêm opt-out nhẹ vào first-touch Zalo/email/call.

### 4.7. Placeholder `[Tên khách]` có thể khiến AI bịa tên người liên hệ

- File: `references/email-templates.md`
- Vị trí/heading: `Placeholder Dùng Chung`
- Vấn đề: Lead Google Maps thường chỉ có tên doanh nghiệp, không có tên người phụ trách. Placeholder `[Tên khách]` nếu không có nguồn công khai có thể khiến AI tự điền/hallucinate tên.
- Vì sao nguy hiểm: Vi phạm nguyên tắc không bịa dữ liệu và không thu thập dữ liệu cá nhân không công khai.
- Đề xuất xử lý: Đổi thành `[Tên khách nếu có nguồn công khai]` hoặc dùng mặc định "anh/chị phụ trách [Tên doanh nghiệp]".

## 5. Lỗi Nhỏ / Tối Ưu Văn Phong

### 5.1. README chưa liệt kê quick commands

- File: `README.md`
- Vấn đề: README mô tả đúng skill nhưng chưa có danh sách 9 quick commands.
- Đề xuất: Thêm mục "Quick Commands" ngắn để người dùng mới biết cách gọi skill.

### 5.2. Một số câu outreach hơi dài cho Zalo

- File: `references/email-templates.md`
- Vấn đề: Zalo first-touch và một số message theo tình huống dài hơn mức tối ưu cho cold lead.
- Đề xuất: Thêm phiên bản 3 dòng cho Zalo sau này, chưa cần thay toàn bộ template.

### 5.3. Một số câu có thể nghe chắc chắn quá khi dữ liệu chưa đủ

- File: `SKILL.md`, `references/email-templates.md`, `references/customer-profiles.md`
- Ví dụ: "bên mình đang có lượng khách quan tâm khá tốt", "có thể tăng lịch hẹn".
- Đề xuất: Dùng wording có điều kiện hơn: "em thấy có tín hiệu khách quan tâm", "có thể xem xét hướng tăng chuyển đổi".

### 5.4. Internal reference trong playbook chưa thống nhất style

- File: `references/gmap-search-playbook.md`
- Vấn đề: Có dòng `google-sheets-template.md` không kèm `references/`. Vì cùng thư mục nên không sai, nhưng style khác `SKILL.md`.
- Đề xuất: Có thể đổi thành `references/google-sheets-template.md` hoặc ghi "xem file cùng thư mục".

### 5.5. Ví dụ Google Sheets chỉ có 18 cột, trong khi template chuẩn có hơn 30 cột

- File: `references/google-sheets-template.md`
- Vị trí/heading: `Ví Dụ 5 Dòng Dữ Liệu Mẫu`
- Vấn đề: Bảng mẫu là phiên bản rút gọn, không nói rõ đây là bảng rút gọn.
- Đề xuất: Ghi "Ví dụ rút gọn các cột chính" để tránh sales copy thiếu cột.

## 6. Điểm Dư Thừa / Trùng Lặp

- `SKILL.md` đang chứa khá nhiều chi tiết đã có trong reference files: bảng sheet cơ bản, scoring table đầy đủ, ví dụ Zalo mở đầu, objection list.
- `references/gmap-search-playbook.md` và `SKILL.md` lặp công thức keyword. Lặp nhẹ này chấp nhận được vì keyword là core workflow.
- `references/customer-profiles.md` và `references/scoring-examples.md` cùng có script mở đầu theo ngành. Không trùng xấu vì một file là phân loại ngành, một file là scoring giả lập.
- `references/email-templates.md` có nhiều template cùng cấu trúc mở đầu. Có thể thêm "short variants" thay vì nhân bản thêm template dài.

Đề xuất kiến trúc:

- Giữ `SKILL.md` ở mức điều phối: vai trò, context, module chính, scoring tóm tắt, output format, quick commands, safety, reference map.
- Đẩy phần bảng cột chi tiết, ví dụ scoring, checklist và template dài hoàn toàn vào references.
- Không cần đổi cấu trúc repo hiện tại.

## 7. Điểm Thiếu Trước Khi Test Thật

Nên bổ sung ở vòng sau trước pilot thật:

- `references/test-cases.md`: 5-7 case mẫu để test skill không cần lead thật.
- `references/qa-checklist.md`: checklist QA trước khi gửi lead cho sales.
- `references/pilot-runbook.md`: quy trình pilot 1 tuần, số lead/ngày, cách ghi phản hồi, tiêu chí dừng.
- Short outreach variants: Zalo 3 dòng, call 20 giây, follow-up cực nhẹ.
- Chuẩn sheet cuối cùng có cột compliance: `Data source`, `Last checked at`, `Approved for outreach`, `Do not contact`.

## 8. Kiểm Tra Quick Commands

| Command | Có trong SKILL.md chưa? | Có logic hỗ trợ chưa? | Ghi chú |
|---|---|---|---|
| `/gmap-plan [ngành] [khu vực] [dịch vụ]` | Có | Có | Được hỗ trợ bởi Module 1 và playbook. |
| `/gmap-keywords [ngành] [khu vực]` | Có | Có | Có công thức keyword trong `SKILL.md` và playbook. |
| `/gmap-sheet` | Có | Có | Có bảng cơ bản và reference template. Nên đồng bộ status/priority. |
| `/gmap-score [dữ liệu lead]` | Có | Có | Có framework và scoring examples. |
| `/gmap-message [lead] [dịch vụ]` | Có | Có | Có email/Zalo templates. Nên thêm opt-out ở first touch. |
| `/gmap-call [lead]` | Có | Có | Có call script 30 giây. Có thể thêm call 20 giây sau. |
| `/gmap-followup [tình huống]` | Có | Có | Có follow-up 3/7/14 ngày. |
| `/gmap-report [danh sách lead]` | Có | Có một phần | Output format cho danh sách lead có đủ phân loại/score/ưu tiên, nhưng chưa có report template riêng. |
| `/gmap-automation [mục tiêu]` | Có | Có | Có automation workflow hợp pháp. |

Ghi chú: README nên nhắc 9 command này để người dùng mới dễ phát hiện.

Đề xuất command cho vòng sau, chưa thêm:

- `/gmap-audit [danh sách lead]`: kiểm tra chất lượng và compliance của list lead.
- `/gmap-clean [sheet data]`: chuẩn hóa dữ liệu sheet, loại trùng, chuẩn hóa status.
- `/gmap-test [case mẫu]`: chạy test bằng case giả lập trước khi dùng lead thật.

## 9. Kiểm Tra Scoring Framework

- Tổng điểm framework đúng **100 điểm**: 25 + 20 + 25 + 15 + 15.
- Các nhóm điểm có hướng dẫn rõ trong `SKILL.md`.
- `SKILL.md` có yêu cầu nếu thiếu dữ liệu thì ghi "chưa có dữ liệu".
- Các ví dụ trong `references/scoring-examples.md` cộng điểm đúng tổng:
  - Nha Khoa An Tâm: 25 + 18 + 24 + 12 + 11 = 90.
  - IELTS Bright Future: 14 + 19 + 23 + 15 + 11 = 82.
  - Luna Spa: 22 + 20 + 25 + 15 + 10 = 92.
  - Nhà Hàng Chay Mộc: 10 + 13 + 16 + 15 + 14 = 68.
  - Homestay Biển Xanh: 23 + 15 + 20 + 12 + 9 = 79.
  - Nội Thất Gia Phát: 18 + 10 + 21 + 15 + 9 = 73.
  - Bao Bì Tân Minh: 13 + 14 + 23 + 15 + 11 = 76.
  - Vận Tải Nam An: 15 + 16 + 25 + 15 + 13 = 84.
  - Sàn Nhà Đất Đông Thành: 10 + 18 + 22 + 15 + 13 = 78.
  - Điện Nước Nhanh 24h: 25 + 12 + 15 + 8 + 7 = 67.
- Priority P1/P2 đúng theo thang điểm trong toàn bộ ví dụ.
- Điểm cần chỉnh: thêm guardrail để "không có website" không tự động biến lead thành nóng; chỉnh `Contact Score` để không khuyến khích gom nhiều kênh liên hệ hơn mức cần thiết.

## 10. Kiểm Tra Compliance

| Tiêu chí | Đạt / Chưa đạt | Ghi chú |
|---|---|---|
| Không hướng dẫn scraping Google Maps trái phép | Đạt | Nhiều file nhấn mạnh không scrape trái phép. |
| Không hướng dẫn bypass CAPTCHA/vượt rate limit/né chặn | Đạt | Automation workflow nêu không vượt CAPTCHA/rate limit. |
| Không khuyến khích gửi tin hàng loạt | Đạt | Có chống spam; nên thêm opt-out vào first-touch templates. |
| Không khuyến khích thu thập dữ liệu cá nhân nhạy cảm | Đạt | Có nhiều guardrail về dữ liệu doanh nghiệp công khai. |
| Không dùng dữ liệu cá nhân không công khai | Đạt một phần | Placeholder `[Tên khách]` nên đổi để tránh bịa tên người liên hệ. |
| Không tự động gửi email/Zalo trước Sales review | Đạt | Automation workflow bắt buộc Sales/Human review. |
| Có trạng thái `Do Not Contact` | Đạt | Có trong sheet template và automation. |
| Có nguyên tắc dừng liên hệ khi khách yêu cầu | Đạt | Có trong playbook, sheet và automation. |
| Có ghi rõ nguồn dữ liệu và thời điểm lấy dữ liệu trong automation | Đạt | Automation có `data_source`, `last_checked_at`; sheet template chưa có cột này. |
| Có nhấn mạnh Google Places API hoặc dữ liệu hợp pháp | Đạt | Rõ trong `SKILL.md`, README, playbook, automation workflow. |

## 11. Đề Xuất Sửa Ở Vòng Tiếp Theo

### Must Fix

- Đồng bộ priority/status giữa `SKILL.md`, `google-sheets-template.md`, `scoring-examples.md`.
- Bổ sung cột `Data source`, `Last checked at`, `Approved for outreach`, `Do not contact` vào Google Sheets template.
- Chỉnh placeholder `[Tên khách]` thành `[Tên khách nếu có nguồn công khai]` hoặc mặc định "anh/chị phụ trách".
- Thêm câu opt-out nhẹ vào first-touch Zalo/email/call.

### Should Fix

- Thêm guardrail cho scoring: "không có website" không tự động là lead nóng.
- Chỉnh `Contact Score` theo hướng đủ kênh công khai, đúng phép, không phải càng nhiều càng tốt.
- Thêm danh sách quick commands vào README.
- Đồng bộ wording phản đối giữa `SKILL.md` và `objection-handling.md`.
- Ghi rõ bảng ví dụ Google Sheets là bản rút gọn.

### Nice To Have

- Tạo `references/test-cases.md`.
- Tạo `references/qa-checklist.md`.
- Tạo `references/pilot-runbook.md`.
- Thêm Zalo 3 dòng, call 20 giây, follow-up không gây phiền.
- Thêm template báo cáo `/gmap-report` riêng.

## 12. Kết Luận

- **Chưa nên test thật diện rộng ngay.**
- Có thể test nội bộ bằng case giả lập ngay, nhưng trước khi dùng lead thật nên sửa các mục Must Fix ở trên.
- Nếu muốn test thật sau khi sửa, nên bắt đầu bằng một case hẹp:
  - Ngành: spa hoặc nha khoa.
  - Khu vực: một quận cụ thể như Quận 3 hoặc Gò Vấp.
  - Dịch vụ: WaaS + Chatbot AI.
  - Số lead: 10-15 lead đã được sales review thủ công.
  - Điều kiện: không automation gửi tin, chỉ dùng sheet + message suggestion + sales chỉnh tay.

Trong lần audit này, không sửa nội dung file hiện có. Chỉ tạo mới `AUDIT_REPORT.md`.
