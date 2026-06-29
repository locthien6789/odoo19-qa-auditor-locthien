# Test Cases — 5 lớp tư duy (L1–L5)

> Mọi quy trình audit PHẢI có test case ở đủ 5 lớp. Không bỏ lớp nào.

---

## L1 — Happy Path (Luồng đúng)

**Mục tiêu**: Verify hệ thống làm đúng khi dữ liệu chuẩn, user thao tác đúng.

### Đặc điểm
- Dữ liệu hợp lệ, đúng format
- User có đủ quyền
- Thao tác theo đúng SOP
- Khối lượng vừa phải

### Ví dụ Sales
| TC | Mô tả | Expected |
|---|---|---|
| L1-S01 | Tạo SO 1 sản phẩm, KH thường, UoM mặc định | SO confirm OK, sinh DO |
| L1-S02 | Confirm DO → Invoice → Payment | Đầy đủ bút toán, không lỗi |
| L1-S03 | Báo cáo doanh thu tháng | Đúng số liệu |

### Tỉ lệ chiếm
~30% tổng test case. **Đây là baseline**, mọi feature đều phải qua được.

---

## L2 — Edge Case (Giá trị biên)

**Mục tiêu**: Verify hệ thống ổn định ở các giá trị biên, không crash hay sai logic.

### Đặc điểm
- Giá trị min/max của field
- Số 0, số âm (khi business cho phép)
- Chuỗi rỗng, chuỗi rất dài
- Ngày đầu/cuối tháng/năm
- Multi-byte chars (tiếng Việt có dấu, emoji)
- Timezone boundaries

### Ví dụ
| TC | Mô tả | Expected |
|---|---|---|
| L2-S01 | SO với số lượng = 0.001 KG (3 chữ số thập phân) | Hệ thống xử lý đúng precision, không round về 0 |
| L2-S02 | SO ngày 31/12 23:59 → invoice ngày 01/01 năm sau | Bút toán đúng kỳ |
| L2-S03 | Tên KH có dấu tiếng Việt + ký tự đặc biệt: "Công ty TNHH ABC & Co., Ltd." | Hiển thị đúng trong HĐĐT |
| L2-S04 | Số tiền > 10 tỷ VND | Không tràn số, hiển thị đầy đủ |
| L2-S05 | Lot có ngày hết hạn = hôm nay | Cảnh báo nhưng vẫn cho xuất (theo policy) |
| L2-S06 | Pricelist hiệu lực từ 00:00:00 hôm nay | Áp dụng ngay từ đầu ngày |
| L2-S07 | Discount = 100% (hàng tặng) | Doanh thu = 0, có cảnh báo? |
| L2-S08 | UoM Can với tỉ trọng 0.5 (rất nhẹ) | Quy đổi đúng, không lỗi divide |

### Tỉ lệ chiếm
~25% tổng test case.

---

## L3 — Negative Test (Người dùng cố làm sai)

**Mục tiêu**: Verify hệ thống chặn được hành vi sai, không cho data rác lọt vào.

### Đặc điểm
- User cố nhập sai
- Bypass UI bằng API
- Double submit
- SQL injection / XSS attempt
- Sử dụng quyền không có

### Ví dụ
| TC | Mô tả | Expected |
|---|---|---|
| L3-S01 | Nhập số lượng âm (-10) trên SO | Chặn ngay với message rõ |
| L3-S02 | Nhập text vào field số | Form không cho save |
| L3-S03 | Confirm SO 5 lần liên tiếp (rapid click) | Chỉ tạo 1, không double |
| L3-S04 | Gọi API xml-rpc để confirm SO của user khác | 403 Forbidden |
| L3-S05 | Nhập SQL injection vào search: `' OR 1=1; DROP TABLE--` | Sanitize, không execute |
| L3-S06 | Upload file .exe vào field COA | Chặn, chỉ accept PDF |
| L3-S07 | Tạo HĐĐT khi đã hết số (sequence cạn) | Báo lỗi rõ, không câm | 
| L3-S08 | Xóa Lot đang có tồn kho | Chặn |
| L3-S09 | Sửa Pricelist của user khác (chưa share) | 403 |
| L3-S10 | Cố post invoice vào kỳ đã khóa | Chặn |

### Tỉ lệ chiếm
~20% tổng test case.

---

## L4 — Integration Test (Liên module)

**Mục tiêu**: Verify dữ liệu nhất quán giữa các module, bút toán đúng end-to-end.

### Đặc điểm
- Bắt buộc trace ngược/xuôi
- Đối chiếu UI ↔ Database
- Test các trigger, automated action
- Test webhook và API integration

### Ví dụ
| TC | Mô tả | Expected |
|---|---|---|
| L4-I01 | SO 100 KG HCl 32% → DO → Invoice → Payment → BCTC | Doanh thu, giá vốn, công nợ, tồn kho đều khớp |
| L4-I02 | Trả hàng 20 KG → Credit Note → HĐĐT điều chỉnh gửi CQT | Sequence đúng, bút toán đảo đúng |
| L4-I03 | Đặt cọc 30% trước → giao đợt 1 50% → giao đợt 2 50% | Doanh thu ghi theo từng đợt, công nợ chính xác |
| L4-I04 | Mua NK USD → landed cost VND → bán VND | Giá vốn phản ánh đủ landed cost, chênh tỉ giá đúng |
| L4-I05 | Kiểm kê thiếu 50 KG → bút toán Nợ 632 / Có 156 → BCTC giảm tồn, tăng CP | Đúng TT133 |
| L4-I06 | Bật/tắt route MTO → SO không có hàng có tự sinh PO? | Workflow đúng |
| L4-I07 | Xóa Invoice → Payment liên quan bị unreconcile → trạng thái KH đúng | Cascade đúng |
| L4-I08 | Pricelist update giữa kỳ → SO đã confirm trước có bị thay đổi giá không? | KHÔNG bị thay đổi (đúng) |
| L4-I09 | Tạo Project ↔ link SO ↔ Timesheet (nếu có) | Liên kết đúng |
| L4-I10 | Webhook ngân hàng → auto reconcile invoice | Match đúng, không nhầm |

### Tỉ lệ chiếm
~15% tổng test case nhưng **bug ở L4 thường nghiêm trọng nhất**.

---

## L5 — Security & SoD (Phân quyền + Kiểm soát)

**Mục tiêu**: Verify phân quyền, audit trail, chống gian lận.

### Đặc điểm
- Test theo từng group user
- Test record rule (KH/SO/PO của ai)
- Test field-level security (cost, margin)
- Test workflow approval
- Test audit trail

### Ví dụ
| TC | Mô tả | Expected |
|---|---|---|
| L5-SE01 | Salesman A xem SO của Salesman B | 403 hoặc list rỗng |
| L5-SE02 | Salesman xem field "cost_price" của product | Field ẩn |
| L5-SE03 | Thủ kho xem field "list_price" | Field ẩn |
| L5-SE04 | Kế toán viên approve Invoice của mình | Chặn (cần Kế toán trưởng) |
| L5-SE05 | Một user vừa tạo PO vừa duyệt PO | Chặn SoD |
| L5-SE06 | Admin sửa giá trên SO đã confirm | Cho phép NHƯNG có log đầy đủ + cảnh báo |
| L5-SE07 | Đăng nhập từ IP nước ngoài | Cảnh báo CEO |
| L5-SE08 | 5 lần login fail → lock 15 phút | Hoạt động đúng |
| L5-SE09 | Session timeout sau 30 phút idle | Auto logout |
| L5-SE10 | Disable 2FA cho CEO account | Chặn cứng |
| L5-SE11 | Export toàn bộ KH ra Excel — user thường có quyền? | Chỉ Manager+ |
| L5-SE12 | Truy cập database trực tiếp (psql) — log gì? | Có log audit |
| L5-SE13 | Sửa COA đã đính kèm — version history | Đầy đủ |
| L5-SE14 | Approval workflow 2 cấp: PO > 500tr cần CEO duyệt | Đúng workflow |
| L5-SE15 | Hủy Invoice đã post — log đầy đủ ai/khi nào/lý do | Có |

### Tỉ lệ chiếm
~10% tổng test case nhưng **bug ở L5 là rủi ro pháp lý**.

---

## Phân bổ test case mẫu

Cho 1 module trung bình (vd: Sales):

| Lớp | Số TC | Effort |
|---|---|---|
| L1 Happy | 15 | 1.5 ngày |
| L2 Edge | 12 | 2 ngày |
| L3 Negative | 10 | 1.5 ngày |
| L4 Integration | 8 | 2 ngày |
| L5 Security/SoD | 5 | 1 ngày |
| **Tổng** | **50** | **8 ngày** |

Toàn hệ thống ~6 module core → khoảng **300 test case, 48 ngày làm việc** cho audit lần đầu.

---

## Quy tắc viết test case

**Mỗi test case CÓ ĐỦ 7 phần:**

```
TC-ID: L[1-5]-[Module]-[NNN]
Title: <Động từ + đối tượng + điều kiện>
Precondition: <Setup trước khi test>
Test Data: <Dữ liệu cụ thể, không nói chung chung>
Steps: <1. ... 2. ... 3. ...>
Expected: <Kết quả mong đợi, đo lường được>
Postcondition: <Cleanup, restore data>
```

**Mỗi test case CHỈ test 1 điều.** Đừng gộp 5 thứ vào 1 case.

**Test data phải reproducible.** Không nói "1 KH bất kỳ" — phải nói cụ thể "KH Cty TNHH XYZ, mã KH00123".
