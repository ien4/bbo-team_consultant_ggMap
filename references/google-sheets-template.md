# Google Sheets Lead Template — BBOTech

Template này dùng để quản lý lead tìm được từ Google Maps / Google Business Profile. Chỉ nhập thông tin doanh nghiệp công khai hoặc dữ liệu người dùng cung cấp hợp pháp.

## Bảng Cột Chuẩn

| Cột | Giải thích |
|---|---|
| STT | Số thứ tự lead trong sheet |
| Batch ID | Mã đợt tìm lead, ví dụ `2026-05-spa-q3` |
| Ngày nhập | Ngày đưa lead vào sheet |
| Nguồn keyword | Keyword Google Maps đã dùng |
| Tên doanh nghiệp | Tên hiển thị trên Google Business Profile |
| Ngành nghề | Nhóm ngành chính |
| Khu vực | Quận/huyện/thành phố |
| Địa chỉ | Địa chỉ doanh nghiệp công khai |
| Số điện thoại | Số doanh nghiệp công khai |
| Website | Website công khai nếu có |
| Link Google Maps | Link Google Maps/Business Profile |
| Rating | Điểm đánh giá |
| Số lượng review | Tổng số review |
| Trạng thái hoạt động | Đang hoạt động/tạm đóng/đóng cửa/khác |
| Fanpage / Social link nếu có | Link fanpage/social công khai |
| Email công khai nếu có | Email doanh nghiệp công khai |
| Người liên hệ nếu có nguồn công khai | Chỉ ghi nếu là thông tin công khai của doanh nghiệp |
| Tình trạng website | Không có/cũ/lỗi mobile/thiếu CTA/tốt |
| Dấu hiệu pain point | Quan sát khách quan từ Maps, website, review, fanpage |
| Dịch vụ BBOTech phù hợp | WaaS/Chatbot AI/Email Automation/Workflow/Agentic AI |
| Website Score | Điểm nhóm website, tối đa 25 |
| CSKH Score | Điểm nhóm nhu cầu chăm sóc khách, tối đa 20 |
| Fit Score | Điểm phù hợp dịch vụ BBOTech, tối đa 25 |
| Contact Score | Điểm khả năng liên hệ, tối đa 15 |
| Opportunity Score | Điểm tín hiệu cơ hội, tối đa 15 |
| Lead Score | Tổng điểm 0-100 |
| Mức ưu tiên | P1/P2/P3/P4 |
| Góc tiếp cận đề xuất | Một câu định hướng outreach |
| Kênh tiếp cận đề xuất | Zalo/email/call/fanpage |
| Trạng thái follow-up | New/Checked/Contacted/Replied/Demo Booked/Proposal Sent/Won/Lost/Nurture/Do Not Contact |
| Ngày liên hệ lần 1 | Ngày gửi tin/gọi đầu tiên |
| Ngày follow-up tiếp theo | Ngày cần follow-up |
| Owner sales | Người phụ trách lead |
| Ghi chú | Ghi chú ngắn, không chứa dữ liệu nhạy cảm |

## Quy Ước Trạng Thái Lead

| Trạng thái | Ý nghĩa |
|---|---|
| `New` | Lead mới nhập, chưa kiểm tra đủ |
| `Checked` | Đã kiểm tra profile, website, score, đủ điều kiện liên hệ |
| `Contacted` | Đã liên hệ lần đầu |
| `Replied` | Khách đã phản hồi |
| `Demo Booked` | Đã hẹn demo/call |
| `Proposal Sent` | Đã gửi proposal/báo giá |
| `Won` | Chốt thành khách hàng |
| `Lost` | Không phù hợp hoặc từ chối rõ |
| `Nurture` | Chưa mua ngay, cần chăm sóc sau |
| `Do Not Contact` | Khách yêu cầu không liên hệ hoặc không nên liên hệ |

## Quy Ước Mức Ưu Tiên

| Mức | Quy ước | Điểm gợi ý | Cách xử lý |
|---|---|---:|---|
| `P1 Lead nóng` | Rất phù hợp, liên hệ ngay | 80-100 | Cá nhân hóa sâu, gọi/Zalo trong ngày |
| `P2 Tiềm năng` | Có fit rõ, cần thông điệp tốt | 60-79 | Outreach cá nhân hóa, follow-up 7-14 ngày |
| `P3 Nurture` | Có thể phù hợp nhưng thiếu tín hiệu | 40-59 | Lưu nurture, chưa ưu tiên gọi ngay |
| `P4 Không ưu tiên` | Không đủ fit hoặc thiếu dữ liệu | Dưới 40 | Không outreach hoặc chỉ xử lý khi có thêm dữ liệu |

## Công Thức Gợi Ý

**Tổng Lead Score**

```text
=SUM(U2:Y2)
```

Nếu cột điểm lần lượt là:

- `U`: Website Score
- `V`: CSKH Score
- `W`: Fit Score
- `X`: Contact Score
- `Y`: Opportunity Score

**Tự gán mức ưu tiên**

```text
=IFS(Z2>=80,"P1 Lead nóng",Z2>=60,"P2 Tiềm năng",Z2>=40,"P3 Nurture",Z2<40,"P4 Không ưu tiên")
```

**Lọc lead nóng cần liên hệ**

```text
=FILTER(A:AH,Z:Z>=80,AD:AD="Checked")
```

**Lọc lead chưa follow-up**

```text
=FILTER(A:AH,AD:AD="Contacted",AF:AF<=TODAY())
```

Điều chỉnh ký tự phân tách `,` hoặc `;` theo locale của Google Sheets.

## Cách Dùng Màu Và Filter

- Bật filter cho toàn bộ hàng tiêu đề.
- Freeze hàng tiêu đề và 4-5 cột đầu.
- Conditional formatting:
  - `P1 Lead nóng`: nền đỏ nhạt hoặc cam nhạt.
  - `P2 Tiềm năng`: nền vàng nhạt.
  - `P3 Nurture`: nền xanh nhạt.
  - `P4 Không ưu tiên`: nền xám nhạt.
  - `Do Not Contact`: chữ đỏ đậm hoặc nền xám.
- Tạo dropdown cho `Mức ưu tiên`, `Trạng thái follow-up`, `Dịch vụ BBOTech phù hợp`, `Owner sales`.
- Filter nhanh theo:
  - `Lead Score >= 80`
  - `Trạng thái follow-up = Checked`
  - `Dịch vụ BBOTech phù hợp = Chatbot AI`
  - `Khu vực = Quận 3`

## Ví Dụ 5 Dòng Dữ Liệu Mẫu

| STT | Batch ID | Ngày nhập | Nguồn keyword | Tên doanh nghiệp | Ngành nghề | Khu vực | Số điện thoại | Website | Rating | Reviews | Tình trạng website | Pain point | Dịch vụ phù hợp | Lead Score | Mức ưu tiên | Kênh | Trạng thái |
|---:|---|---|---|---|---|---|---|---|---:|---:|---|---|---|---:|---|---|---|
| 1 | 2026-05-nhakhoa-q1 | 2026-05-04 | phòng khám nha khoa quận 1 | Nha Khoa An Tâm | Nha khoa | Quận 1 | Công khai | Không có | 4.7 | 126 | Không có website | Review nhiều, chưa có website đặt lịch | WaaS + Chatbot AI | 90 | P1 Lead nóng | Zalo/call | Checked |
| 2 | 2026-05-spa-q3 | 2026-05-04 | spa quận 3 | Luna Spa | Spa | Quận 3 | Công khai | Fanpage | 4.8 | 203 | Chỉ có fanpage | Cần đặt lịch/tư vấn ngoài giờ | Chatbot AI + Email Automation | 92 | P1 Lead nóng | Zalo | Checked |
| 3 | 2026-05-logistics-bd | 2026-05-04 | công ty logistics Bình Dương | Vận Tải Nam An | Logistics | Bình Dương | Công khai | Có | 4.2 | 57 | Thiếu form báo giá | Quy trình báo giá nhiều bước | Workflow + Agentic AI | 84 | P1 Lead nóng | Email/call | New |
| 4 | 2026-05-fnb-q7 | 2026-05-04 | nhà hàng chay quận 7 | Nhà Hàng Chay Mộc | F&B | Quận 7 | Công khai | Có | 4.5 | 312 | Thiếu đặt bàn | Menu có nhưng chưa có form đặt bàn | WaaS + Chatbot AI | 68 | P2 Tiềm năng | Zalo | Contacted |
| 5 | 2026-05-noithat-bt | 2026-05-04 | công ty nội thất Bình Thạnh | Nội Thất Gia Phát | Nội thất | Bình Thạnh | Công khai | Có | 4.3 | 41 | Website cũ | Portfolio yếu, form báo giá chưa rõ | WaaS + Workflow | 73 | P2 Tiềm năng | Email | Nurture |

## Lưu Ý Quản Lý Dữ Liệu

- Không ghi số cá nhân nếu không phải thông tin công khai của doanh nghiệp.
- Không ghi dữ liệu nhạy cảm hoặc nhận xét tiêu cực thiếu kiểm chứng.
- Khi khách yêu cầu dừng liên hệ, chuyển trạng thái `Do Not Contact`.
- Mỗi lead nên có một `Owner sales` rõ ràng để tránh nhiều người liên hệ trùng.
