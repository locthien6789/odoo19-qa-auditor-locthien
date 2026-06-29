# Checklist Audit — Tư duy Kế toán trưởng & Kiểm soát gian lận

> Góc nhìn: **Chief Accountant / Internal Control Officer**
> Mục tiêu: phát hiện kẽ hở cho gian lận, sai sót, vi phạm SoD (Segregation of Duties)

---

## A. Segregation of Duties (SoD) — Phân tách nhiệm vụ

Nguyên tắc vàng: **1 người không được làm cả vòng tròn**. Phải có ít nhất 2 người ở các điểm sau.

### A1. Chu trình Mua hàng
| Vai trò | Người làm | Không được kiêm |
|---|---|---|
| Tạo Purchase Request | A | ≠ B, C, D |
| Duyệt PO | B | ≠ A |
| Nhận hàng (Receipt) | C | ≠ A, B |
| Validate Bill (đối chiếu) | D | ≠ A, C |
| Register Payment | E | ≠ A, D |

| # | Test case | Expected | Mức |
|---|---|---|---|
| A01 | User A tạo PO → User A duyệt PO | Chặn cứng | 🔴 |
| A02 | User C nhận hàng → User C validate Bill | Chặn hoặc warning | 🔴 |
| A03 | User E thanh toán → User E là người tạo PO | Chặn | 🔴 |
| A04 | Admin có thể bypass SoD không? | Phải log + cảnh báo CEO | 🔴 |

### A2. Chu trình Bán hàng
| Vai trò | Người làm | Không được kiêm |
|---|---|---|
| Tạo Quote/SO | Sales | ≠ Approve |
| Duyệt giá đặc biệt | Sales Manager | ≠ Sales |
| Xuất hàng (DO) | Thủ kho | ≠ Sales |
| Xuất hóa đơn | Kế toán | ≠ Thủ kho |
| Thu tiền | Thủ quỹ | ≠ Kế toán ghi sổ |

### A3. Chu trình Kế toán
| Vai trò | Người làm | Không được kiêm |
|---|---|---|
| Tạo Journal Entry | Kế toán viên | ≠ Approve |
| Approve Journal | Kế toán trưởng | ≠ Tạo |
| Đóng kỳ kế toán | Kế toán trưởng + CEO | Cần 2 chữ ký |

---

## B. Audit Trail — Dấu vết kiểm toán

| # | Test case | Expected | Mức |
|---|---|---|---|
| B01 | Mọi thay đổi field quan trọng (giá, số lượng, KH, TK) được log | Bật `track_visibility` trên model | 🔴 |
| B02 | Log có: ai sửa, sửa lúc nào, sửa từ giá trị nào → giá trị nào | Đầy đủ 4 thông tin | 🔴 |
| B03 | Log không thể xóa kể cả Admin | Hard-locked | 🔴 |
| B04 | Xem được log từ UI (không cần SQL) | Tab Chatter / History | 🟠 |
| B05 | Export log để audit định kỳ (Excel/PDF) | Có function export | 🟠 |
| B06 | Log lưu giữ tối thiểu 10 năm (theo luật kế toán VN) | Không bị purge sớm | 🔴 |

---

## C. Approval Workflow — Phê duyệt nhiều cấp

| # | Test case | Expected | Mức |
|---|---|---|---|
| C01 | PO > 50 triệu — cần Sales Manager duyệt | Workflow chặn cho đến khi duyệt | 🔴 |
| C02 | PO > 500 triệu — cần CEO duyệt | 2 cấp duyệt | 🔴 |
| C03 | Discount > 10% — cần Sales Manager duyệt | Áp dụng cho SO | 🔴 |
| C04 | Giá thấp hơn giá vốn — chặn cứng hoặc cần CFO duyệt | Cảnh báo + duyệt | 🔴 |
| C05 | Sửa hóa đơn đã post — cần Kế toán trưởng duyệt | Workflow rõ ràng | 🔴 |
| C06 | Approval bị reject — có lý do bắt buộc nhập | Field required | 🟠 |
| C07 | Approval timeout (3 ngày không duyệt) — escalate cấp trên | Notification | 🟠 |

---

## D. Phát hiện gian lận điển hình

### D1. Gian lận trong Mua hàng
| # | Kịch bản | Cách phát hiện | Mức |
|---|---|---|---|
| D01 | Tạo NCC "ma" (fake vendor) → tạo PO → thanh toán | Báo cáo: NCC mới + giao dịch lớn lần đầu | 🔴 |
| D02 | Mua giá cao bất thường để ăn chênh lệch | Cảnh báo giá > 15% so với trung bình | 🔴 |
| D03 | Tạo PO khớp với invoice không có hàng thực (split invoice) | Đối chiếu Bill ↔ Receipt phải có | 🔴 |
| D04 | Sửa bank account của NCC sang tài khoản cá nhân | Audit trail bank account change + duyệt cấp cao | 🔴 |

### D2. Gian lận trong Bán hàng
| # | Kịch bản | Cách phát hiện | Mức |
|---|---|---|---|
| D05 | Bán giá thấp cho KH "thân quen" để kickback | Cảnh báo giá < 10% trung bình | 🔴 |
| D06 | Tạo Credit Note giả để xóa công nợ KH | Mọi Credit Note cần duyệt + lý do | 🔴 |
| D07 | Xuất hàng nhưng không xuất hóa đơn (bán ngoài sổ) | Đối chiếu DO ↔ Invoice phải khớp 100% | 🔴 |
| D08 | Thu tiền mặt nhưng ghi nhận thiếu | Đối chiếu sổ quỹ thường xuyên | 🔴 |

### D3. Gian lận trong Kho
| # | Kịch bản | Cách phát hiện | Mức |
|---|---|---|---|
| D09 | Sửa tỉ trọng UoM để xuất ít hơn báo cáo | Chặn sửa tỉ trọng sau khi có giao dịch | 🔴 |
| D10 | Xuất kho không qua DO (manual adjustment) | Báo cáo "Inventory adjustments" định kỳ | 🔴 |
| D11 | Đổi lot xuất sau khi đã DO done | Lock lot sau khi confirm | 🔴 |
| D12 | Kiểm kê chênh lệch ảo để che giấu thiếu hụt | Yêu cầu duyệt cấp cao + lý do + photo | 🔴 |

### D4. Gian lận trong Kế toán
| # | Kịch bản | Cách phát hiện | Mức |
|---|---|---|---|
| D13 | Tạo bút toán điều chỉnh không có chứng từ kèm | Bắt buộc đính kèm chứng từ scan | 🔴 |
| D14 | Đảo bút toán ngày cuối kỳ để "đẹp" báo cáo | Lock date sau cutoff, mọi sửa cần CEO duyệt | 🔴 |
| D15 | Mở lại kỳ đã khóa | Cần 2 chữ ký + log + lý do | 🔴 |
| D16 | Tạo TK "tạp" để "đút" chi phí cá nhân | Review danh sách TK lạ định kỳ | 🟠 |

---

## E. Báo cáo kiểm soát định kỳ

| # | Báo cáo | Tần suất | Người xem |
|---|---|---|---|
| E01 | Top 20 giao dịch lớn nhất tháng | Tháng | CEO + CFO |
| E02 | Danh sách user login bất thường (sau 22h, cuối tuần) | Tuần | IT + CFO |
| E03 | Danh sách bút toán manual (không qua workflow) | Tuần | CFO |
| E04 | Báo cáo SoD violations (nếu có) | Realtime alert | CEO + CFO |
| E05 | Danh sách NCC/KH mới được tạo | Tuần | CFO |
| E06 | Danh sách thay đổi master data (giá, TK ngân hàng KH/NCC) | Tuần | CFO |
| E07 | Inventory adjustments > 5tr | Realtime | CFO |
| E08 | Credit Notes phát hành | Tuần | CFO |
| E09 | Pricelist được sửa | Tuần | Sales Manager + CFO |

---

## F. Phân quyền (Access Rights & Record Rules)

| # | Test case | Expected | Mức |
|---|---|---|---|
| F01 | Salesman A chỉ xem được KH/SO của chính A | Record Rule active | 🔴 |
| F02 | Salesman không xem được giá vốn (cost) | Field-level security | 🔴 |
| F03 | Thủ kho không xem được giá bán | Field-level security | 🔴 |
| F04 | Kế toán viên không có quyền approve invoice | Group rights | 🔴 |
| F05 | User bị deactive — không login được nhưng data còn nguyên | Archive thay vì delete | 🔴 |
| F06 | Reset password chỉ qua email đã verify | Bảo mật | 🔴 |
| F07 | Force logout sau X phút idle | Session timeout | 🟠 |
| F08 | 2FA cho Admin và CEO | Bắt buộc | 🔴 |
| F09 | Login từ IP ngoài VN — cảnh báo | Geo-fencing | 🟠 |

---

## G. Backup & Recovery (Internal Control)

| # | Test case | Expected | Mức |
|---|---|---|---|
| G01 | Backup tự động hàng ngày | Cron job, log thành công | 🔴 |
| G02 | Backup lưu offsite (cloud / NAS riêng) | Không chỉ trên localhost | 🔴 |
| G03 | Test restore định kỳ (quarterly) | Restore thành công vào môi trường test | 🔴 |
| G04 | Backup mã hóa | AES-256 hoặc tương đương | 🔴 |
| G05 | Retention policy: daily 30 ngày, monthly 12 tháng, yearly 10 năm | Theo luật kế toán | 🔴 |
| G06 | RTO (Recovery Time Objective) | ≤ 4 giờ | 🟠 |
| G07 | RPO (Recovery Point Objective) | ≤ 24 giờ | 🟠 |

---

## H. Compliance Checklist (Luật VN)

| # | Quy định | Check |
|---|---|---|
| H01 | Luật Kế toán 2015 — lưu chứng từ 10 năm | ✅/❌ |
| H02 | TT133/2016/TT-BTC — chế độ kế toán DNNVV | ✅/❌ |
| H03 | TT78/2021/TT-BTC + NĐ123/2020 — HĐĐT | ✅/❌ |
| H04 | Luật An toàn thông tin mạng 2018 — bảo vệ dữ liệu cá nhân KH | ✅/❌ |
| H05 | Nghị định 13/2023/NĐ-CP — bảo vệ dữ liệu cá nhân | ✅/❌ |
| H06 | Luật Hóa chất 2007 + sửa đổi — quản lý hóa chất nguy hiểm | ✅/❌ |
| H07 | TT32/2017/TT-BCT — danh mục hóa chất cấm/hạn chế | ✅/❌ |
