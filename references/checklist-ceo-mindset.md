# Checklist Audit — Tư duy CEO B2B Hóa chất

> Góc nhìn: **Ông chủ DN** — cái gì làm mất tiền, mất khách, mất uy tín, mất cơ hội scale?
> Không quan tâm chi tiết kỹ thuật. Chỉ quan tâm: tiền, khách, rủi ro, tốc độ.

---

## A. Visibility — CEO nhìn thấy gì trong 30 giây?

| # | Câu hỏi CEO | Expected | Mức |
|---|---|---|---|
| A01 | Hôm nay bán được bao nhiêu? | Dashboard hiển thị realtime, không phải báo cáo cuối ngày | 🔴 |
| A02 | Lợi nhuận gộp tháng này vs tháng trước? | So sánh trực quan, biểu đồ | 🔴 |
| A03 | Tồn kho có bao nhiêu ngày bán? | Inventory days = Stock / Avg daily COGS | 🔴 |
| A04 | KH nào sắp hết hạn công nợ? | List 10 KH có DSO cao nhất | 🔴 |
| A05 | Tiền mặt + ngân hàng hiện có? | Cash position realtime, multi-currency | 🔴 |
| A06 | PO nào cần CEO duyệt? | Inbox phê duyệt, có notification mobile | 🟠 |
| A07 | KPI nhân viên sales tháng này? | Top performer, doanh số, tỉ lệ chốt | 🟠 |
| A08 | Sản phẩm nào đang lỗ? | Sort by margin ascending, có sản phẩm âm margin? | 🔴 |

## B. Mobile & Remote — CEO đi công tác

| # | Test case | Expected | Mức |
|---|---|---|---|
| B01 | Duyệt SO/PO lớn qua mobile app Odoo | Smooth, không lỗi layout | 🟠 |
| B02 | Xem dashboard qua mobile | Charts hiển thị đúng, không bị crop | 🟠 |
| B03 | Approve qua email link (mà không cần login lại) | Có nút Approve/Reject trong email | 🟡 |
| B04 | Notification push khi có giao dịch lớn (> 100tr) | Realtime, không delay > 1 phút | 🟠 |
| B05 | Mất sóng tạm thời — app có cache offline không? | Read-only mode khi offline | 🟢 |

## C. Cảnh báo bất thường (Anomaly)

| # | Test case | Expected | Mức |
|---|---|---|---|
| C01 | Giá bán lệch > 20% so với trung bình 90 ngày | Cảnh báo trên SO, yêu cầu lý do | 🔴 |
| C02 | KH mới mua lần đầu > 50 triệu | Cảnh báo, yêu cầu duyệt | 🟠 |
| C03 | NCC tăng giá đột ngột > 15% | Cảnh báo trên PO | 🟠 |
| C04 | Sản phẩm có biên lợi nhuận âm | Dashboard cảnh báo đỏ | 🔴 |
| C05 | Tồn kho 1 SP > 6 tháng không xuất | Báo cáo "Dead stock" | 🟠 |
| C06 | DSO đột ngột tăng > 30% so với tháng trước | Alert | 🟠 |
| C07 | Phát hiện 1 user login từ IP lạ / nhiều giờ ngoài giờ làm | Security alert | 🟠 |

## D. Trải nghiệm KH B2B

| # | Test case | Expected | Mức |
|---|---|---|---|
| D01 | KH tra cứu công nợ, lịch sử mua qua portal (Odoo Customer Portal) | Có portal, có HĐĐT, có COA | 🟠 |
| D02 | Đặt hàng online qua portal (B2B Webshop) | Pricelist riêng KH, không lộ giá public | 🟠 |
| D03 | Email confirm SO có đầy đủ: bảng giá, ETA giao hàng, hướng dẫn nhận hàng | Template chuyên nghiệp | 🟠 |
| D04 | KH tải HĐĐT bằng 1 click từ portal | Có XML + PDF | 🟠 |
| D05 | Chatbot hỗ trợ tra cứu cơ bản 24/7 | Tích hợp Odoo Livechat | 🟡 |

## E. Khả năng mở rộng (Scale-up)

| # | Câu hỏi | Expected | Mức |
|---|---|---|---|
| E01 | DN tăng 10x volume — DB localhost có chịu được không? | Test với 100k record, performance < 5s | 🟠 |
| E02 | Cần thêm 50 user — license, phần cứng đủ không? | Plan rõ ràng | 🟠 |
| E03 | Mở chi nhánh tỉnh khác — multi-warehouse có sẵn không? | Cấu hình được không cần dev | 🟠 |
| E04 | Tích hợp e-commerce Shopee/Lazada (nếu mở rộng B2C) | Có connector hoặc API | 🟢 |
| E05 | Tích hợp ERP với phần mềm khác (CRM, MES) | API REST/XML-RPC hoạt động ổn | 🟠 |

## F. Tài chính — Sức khỏe DN

| # | Test case | Expected | Mức |
|---|---|---|---|
| F01 | Cash flow forecast 30/60/90 ngày | Dựa trên SO confirmed + PO confirmed | 🔴 |
| F02 | Working capital = AR + Inventory - AP | Hiển thị realtime | 🟠 |
| F03 | Phân tích KH 80/20 (Pareto) | Top 20% KH đóng góp bao nhiêu doanh thu | 🟠 |
| F04 | LTV (Lifetime Value) theo KH | Tổng doanh thu KH từ ngày đầu | 🟡 |
| F05 | Lợi nhuận theo segment (theo sản phẩm/khu vực/sales) | Drill-down được | 🟠 |

## G. Risk Management

| # | Rủi ro | Question | Mức |
|---|---|---|---|
| G01 | Mất localhost (cháy nổ, mất điện kéo dài) | Backup offsite đủ phục hồi trong 4h không? | 🔴 |
| G02 | Cloudflare Tunnel ngừng dịch vụ | Có VPN backup hoặc IP public dự phòng? | 🔴 |
| G03 | NCC chính ngừng cung cấp | Có second-source list trong hệ thống? | 🟠 |
| G04 | KH lớn chiếm > 30% doanh thu | Dashboard cảnh báo concentration risk | 🟠 |
| G05 | Nhân viên kế toán nghỉ — transfer kiến thức? | Hệ thống có documentation built-in? | 🟠 |
| G06 | Tấn công mạng / ransomware | 2FA bật cho admin? Backup offline? | 🔴 |
| G07 | Sửa luật thuế (vd: NQ giảm VAT hết hạn) | Có update kịp không? | 🟠 |

## H. ROI Odoo Implementation

| # | Câu hỏi | Cách đo | Mức |
|---|---|---|---|
| H01 | Giảm thời gian xuất hóa đơn từ X giờ → Y giờ/ngày | Đo trước-sau, quy ra tiền | 🟠 |
| H02 | Giảm sai sót kho (chênh lệch kiểm kê) | % chênh trước/sau | 🟠 |
| H03 | Tăng tốc độ thu hồi công nợ (DSO giảm) | DSO trước/sau | 🟠 |
| H04 | Giảm nhân sự kế toán/admin | Số FTE tiết kiệm × lương | 🟠 |
| H05 | Tăng doanh thu nhờ data-driven (cross-sell, upsell) | Doanh thu/KH tăng X% | 🟢 |
| H06 | Tổng chi phí Odoo (license + hardware + customize + maintain) | Bảng TCO 3 năm | 🔴 |

---

## I. CEO Decision Framework — Khi audit xong

Sau audit, CEO cần answer 5 câu:

1. **Hệ thống có chạy đúng không?** → Yes/No/Partial
2. **Rủi ro lớn nhất là gì?** → Top 3 với impact bằng tiền
3. **Cần đầu tư thêm bao nhiêu để hoàn thiện?** → VNĐ, thời gian
4. **Khi nào có thể go-live an toàn?** → Date cụ thể
5. **Nếu KHÔNG fix, mất bao nhiêu?** → VNĐ/năm (chi phí cơ hội)

Báo cáo audit phải trả lời được 5 câu này ở Executive Summary.
