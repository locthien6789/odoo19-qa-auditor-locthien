# Bug Report Template

> Mỗi bug PHẢI dùng đúng format này. Tester KHÔNG gửi bug thiếu các trường bắt buộc.

---

## BUG-[YYYYMMDD]-[NNN]

### 1. Thông tin chung
| Trường | Giá trị |
|---|---|
| **Title** | (Câu mô tả ngắn, có động từ + chủ thể + hiện tượng) |
| **Severity** | 🔴 Critical / 🟠 High / 🟡 Medium / 🟢 Low |
| **Priority** | P0 (fix ngay) / P1 (sprint này) / P2 (sprint sau) / P3 (backlog) |
| **Module** | Sales / Purchase / Inventory / Accounting / HĐĐT / Other |
| **Component** | Tên view/model cụ thể (vd: `sale.order` form) |
| **Environment** | Odoo 19.0 Enterprise / localhost / Cloudflare Tunnel |
| **Reporter** | (Tên tester) |
| **Date** | YYYY-MM-DD |
| **Assigned to** | (Dev/Functional) |

### 2. Mô tả

#### 2.1 Steps to Reproduce
1. Login với user [role]
2. Vào menu [Path]
3. Thao tác [chi tiết]
4. ...

#### 2.2 Expected Result
> Mô tả kết quả ĐÚNG theo nghiệp vụ + reference (TT133 điều X, hoặc SOP nội bộ)

#### 2.3 Actual Result
> Mô tả kết quả SAI hiện tại, kèm số liệu cụ thể

#### 2.4 Reproduction Rate
- [ ] 100% (luôn xảy ra)
- [ ] Sometimes (% bao nhiêu lần)
- [ ] Rare (chỉ 1 lần, chưa reproduce lại được)

### 3. Bằng chứng
- Screenshot: [link]
- Video: [link Loom/file]
- Log: ```paste tail log nếu có```
- SQL verify: ```sql
SELECT ...
```

### 4. Phân tích 3-góc-nhìn

#### 🧑‍💼 4.1 Góc CEO — Tác động kinh doanh
- **Thiệt hại tiềm năng**: VNĐ/giao dịch × tần suất
- **Ảnh hưởng KH**: Có mất KH không? Có khiếu nại không?
- **Uy tín**: Có ảnh hưởng thương hiệu Lộc Thiên không?

#### 📊 4.2 Góc Kế toán trưởng — Tác động tài chính/tuân thủ
- **Sai bút toán**: Bút toán nào sai? Số tiền sai bao nhiêu?
- **Vi phạm TT133**: Điều/khoản nào?
- **Rủi ro thuế/HĐĐT**: Có bị phạt CQT không?
- **Audit trail**: Có log đầy đủ để truy vết không?

#### 🔧 4.3 Góc QA Engineer — Root cause kỹ thuật
- **Root cause giả định**: (Logic / Config / Data / UI / Integration)
- **File/Method nghi vấn**: `module/models/file.py::method_name()`
- **Test case nào miss**: Loại nào (L1-L5)?

### 5. Đề xuất fix

#### 5.1 Phương án A — Quick fix (config)
- Mô tả: ...
- Ước tính: X giờ
- Rủi ro: ...

#### 5.2 Phương án B — Customization (code)
- Mô tả: ...
- Ước tính: X ngày
- Rủi ro: phải duy trì khi upgrade Odoo

#### 5.3 Phương án C — Workaround quy trình (SOP)
- Mô tả: ...
- Ước tính: 0 (chỉ training)
- Rủi ro: phụ thuộc kỷ luật người dùng

**Khuyến nghị**: A / B / C — vì [lý do]

### 6. Acceptance Criteria (sau khi fix)
- [ ] Reproduce theo Steps 2.1 → KHÔNG còn lỗi
- [ ] Test thêm 5 case tương tự → tất cả PASS
- [ ] Regression: chạy lại test suite [tên] → không bug mới
- [ ] Kế toán trưởng xác nhận bút toán đúng
- [ ] Log audit đầy đủ

### 7. Related
- Bug liên quan: BUG-...
- Feature request liên quan: FR-...
- Test case ID: TC-...

---

## Ví dụ minh họa

**BUG-20260628-001**

| | |
|---|---|
| Title | Quy đổi UoM Can 25L → KG sai khi sản phẩm có nhiều version tỉ trọng |
| Severity | 🔴 Critical |
| Priority | P0 |
| Module | Sales + Inventory |
| Environment | Odoo 19.0, localhost, Chrome 140 |

**Steps to Reproduce**:
1. Login user Sales
2. Tạo SO mới, chọn KH ABC
3. Chọn sản phẩm "HCl 32% công nghiệp" (tỉ trọng 1.16)
4. Chọn UoM "Can 25L", nhập số lượng 10 can
5. Save

**Expected**: Quantity (KG) = 10 × 25 × 1.16 = **290 KG**
**Actual**: Quantity (KG) = 250 (chỉ nhân thể tích, không nhân tỉ trọng)

**Góc CEO**: Mỗi đơn 10 can sai ~40 KG × giá ~5.000đ/KG = thiệt hại 200.000đ/đơn. Với 50 đơn/tháng = **120 triệu/năm**.

**Góc Kế toán**: Doanh thu ghi nhận thiếu 200.000đ × 50 = 10tr/tháng. Giá vốn cũng sai. Vi phạm nguyên tắc "ghi nhận đúng kỳ, đúng số".

**Góc QA**: Root cause nghi vấn ở `product.template.uom_conversion()` — đang dùng generic conversion thay vì tỉ trọng product-level.

**Đề xuất**: Phương án B (customize) — bắt buộc, vì đây là logic core. Ước tính 3 ngày dev + 2 ngày test.
