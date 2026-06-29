# Mapping Hệ thống TK TT133 ↔ Odoo

> Tham khảo nhanh khi audit bút toán. KHÔNG dùng làm tài liệu kế toán chính thức.
> Luôn verify với Kế toán trưởng trước khi áp dụng.

---

## 1. Tài sản

### 1.1 Tiền và tương đương tiền
| TK TT133 | Tên | Odoo type | Ghi chú |
|---|---|---|---|
| 111 | Tiền mặt | Bank/Cash | Sổ quỹ |
| 1111 | Tiền mặt VND | Cash | Default |
| 1112 | Tiền mặt USD | Cash multi-currency | Cần tỉ giá |
| 112 | Tiền gửi ngân hàng | Bank | Theo từng ngân hàng |
| 1121 | TGNH VND | Bank | Mỗi tài khoản 1 sub-account |
| 1122 | TGNH USD/EUR | Bank multi-currency | |

### 1.2 Phải thu
| TK | Tên | Ghi chú |
|---|---|---|
| 131 | Phải thu KH | Receivable, theo từng KH (Partner-based) |
| 133 | Thuế GTGT được khấu trừ | VAT input |
| 1331 | VAT khấu trừ hàng hóa, dịch vụ | |
| 1332 | VAT khấu trừ TSCĐ | |
| 138 | Phải thu khác | Tạm ứng, ký quỹ |
| 1388 | Phải thu khác | |

### 1.3 Hàng tồn kho
| TK | Tên | Ghi chú |
|---|---|---|
| 151 | Hàng mua đang đi đường | In-transit |
| 152 | Nguyên liệu, vật liệu | Tùy DN |
| 153 | Công cụ, dụng cụ | |
| 154 | Chi phí SXKD dở dang | WIP nếu có sản xuất |
| 155 | Thành phẩm | Nếu sản xuất |
| **156** | **Hàng hóa** | **Lộc Thiên chính: hóa chất nhập về bán** |
| 1561 | Giá mua hàng hóa | Net cost |
| 1562 | Chi phí mua hàng hóa | Landed cost |

### 1.4 TSCĐ
| TK | Tên |
|---|---|
| 211 | TSCĐ hữu hình |
| 213 | TSCĐ vô hình |
| 214 | Hao mòn TSCĐ |
| 217 | Bất động sản đầu tư |
| 241 | XDCB dở dang |
| 242 | Chi phí trả trước |

---

## 2. Nguồn vốn

### 2.1 Nợ phải trả
| TK | Tên | Ghi chú |
|---|---|---|
| 311 | Vay và nợ thuê tài chính | Loan |
| 331 | Phải trả NCC | Payable, theo từng NCC |
| 333 | Thuế và các khoản phải nộp NN | |
| 3331 | VAT phải nộp | VAT output |
| 33311 | VAT đầu ra | |
| 33312 | VAT hàng nhập khẩu | |
| 3332 | Thuế tiêu thụ đặc biệt | |
| 3333 | Thuế xuất, nhập khẩu | |
| 3334 | Thuế TNDN | |
| 3335 | Thuế TNCN | |
| 3338 | Các loại thuế khác | Môn bài, đất... |
| 334 | Phải trả người lao động | Lương |
| 335 | Chi phí phải trả | Accrued expense |
| 338 | Phải trả, phải nộp khác | BHXH, BHYT, BHTN, KPCĐ |
| 3382 | KPCĐ (Kinh phí công đoàn) | |
| 3383 | BHXH | |
| 3384 | BHYT | |
| 3386 | BHTN | |
| 341 | Vay và nợ thuê dài hạn | |
| 352 | Dự phòng phải trả | |

### 2.2 Vốn chủ sở hữu
| TK | Tên |
|---|---|
| 411 | Vốn đầu tư của chủ sở hữu |
| 4111 | Vốn góp |
| 4112 | Thặng dư vốn |
| 414 | Quỹ đầu tư phát triển |
| 418 | Các quỹ khác thuộc vốn CSH |
| 419 | Cổ phiếu quỹ |
| 421 | Lợi nhuận sau thuế chưa phân phối |
| 4211 | LNST năm trước |
| 4212 | LNST năm nay |

---

## 3. Doanh thu

| TK | Tên | Bút toán điển hình |
|---|---|---|
| 511 | Doanh thu bán hàng và CCDV | Nợ 131 / Có 511 + 3331 |
| 5111 | DT bán hàng hóa | Lộc Thiên dùng chính |
| 5112 | DT bán thành phẩm | Nếu sản xuất |
| 5113 | DT cung cấp dịch vụ | Dịch vụ môi trường |
| 515 | DT hoạt động tài chính | Lãi tiền gửi, lãi chênh lệch tỉ giá |
| 521 | Các khoản giảm trừ DT | Chiết khấu, giảm giá, hàng trả lại |
| 5211 | Chiết khấu thương mại | |
| 5212 | Hàng bán bị trả lại | |
| 5213 | Giảm giá hàng bán | |
| 711 | Thu nhập khác | Thanh lý TSCĐ, hàng thừa kiểm kê |

---

## 4. Chi phí

| TK | Tên | Bút toán |
|---|---|---|
| 632 | Giá vốn hàng bán | Nợ 632 / Có 156 (khi xuất hàng) |
| 635 | Chi phí tài chính | Lãi vay, chênh lệch tỉ giá (lỗ) |
| 642 | Chi phí quản lý kinh doanh | Phổ biến cho DNNVV theo TT133 |
| 6421 | Chi phí bán hàng | Lương sales, vận chuyển bán hàng |
| 6422 | Chi phí quản lý DN | Lương admin, văn phòng, khấu hao |
| 811 | Chi phí khác | Phạt, hao hụt bất thường |
| 821 | Chi phí thuế TNDN | |

---

## 5. Xác định kết quả kinh doanh

| TK | Tên |
|---|---|
| 911 | Xác định kết quả kinh doanh |

**Bút toán kết chuyển cuối kỳ (TT133)**:
```
Nợ 511, 515, 711 / Có 911  (kết chuyển doanh thu, thu nhập)
Nợ 911 / Có 632, 635, 642, 811, 821  (kết chuyển chi phí)
Nợ 911 / Có 4212  (kết chuyển lãi)
Hoặc Nợ 4212 / Có 911  (nếu lỗ)
```

---

## 6. Cấu hình Odoo Account Type

Khi audit, kiểm tra mapping **Type** trên Odoo cho mỗi TK:

| Odoo Type | TK TT133 tương ứng |
|---|---|
| Receivable | 131, 1388 |
| Payable | 331, 338x |
| Bank and Cash | 111, 112 |
| Current Assets | 133, 138, 141, 142, 151-157, 242 |
| Non-current Assets | 211, 213, 217, 221, 241 |
| Prepayments | 242 |
| Fixed Assets | 211, 213, 217 |
| Current Liabilities | 311, 331, 333, 334, 335, 338, 341x |
| Non-current Liabilities | 341 dài hạn, 352 |
| Equity | 411, 414, 418, 419, 421 |
| Current Year Earnings | 421 (auto Odoo) |
| Income | 511, 515, 711 |
| Other Income | 711 |
| Expenses | 632, 635, 811 |
| Cost of Revenue | 632 |
| Other Expenses | 642, 635, 811, 821 |

---

## 7. Báo cáo tài chính TT133

| Mã | Tên | TK liên quan |
|---|---|---|
| **B01-DNN** | Báo cáo tình hình tài chính | Toàn bộ Asset, Liability, Equity |
| **B02-DNN** | Báo cáo KQ kinh doanh | 511, 515, 711, 632, 635, 642, 811, 821 |
| **B03-DNN** | Báo cáo lưu chuyển tiền tệ | Phân tích dòng tiền |
| **B09-DNN** | Bản thuyết minh BCTC | Bổ sung |

---

## 8. Quick verify SQL (PostgreSQL)

```sql
-- Verify bút toán Sales: SO confirm → DO done → Invoice posted
SELECT
  so.name as so_number,
  dom.name as do_number,
  am.name as invoice_number,
  aml.account_id, acc.code as account_code, acc.name as account_name,
  aml.debit, aml.credit
FROM sale_order so
JOIN stock_picking dom ON dom.sale_id = so.id
JOIN account_move am ON am.invoice_origin = so.name
JOIN account_move_line aml ON aml.move_id = am.id
JOIN account_account acc ON acc.id = aml.account_id
WHERE so.name = 'SO00001'
ORDER BY am.id, aml.id;

-- Verify giá vốn (632) khớp tồn kho xuất theo lot
SELECT
  sm.product_id,
  sm.product_uom_qty,
  sml.lot_id, sl.name as lot_name,
  svl.value as cost_value,
  aml.debit
FROM stock_move sm
JOIN stock_move_line sml ON sml.move_id = sm.id
JOIN stock_lot sl ON sl.id = sml.lot_id
JOIN stock_valuation_layer svl ON svl.stock_move_id = sm.id
JOIN account_move_line aml ON aml.id = svl.account_move_line_id
WHERE sm.picking_id = (SELECT id FROM stock_picking WHERE name = 'WH/OUT/00001');

-- Đối chiếu công nợ KH
SELECT
  partner.name as customer,
  SUM(aml.debit - aml.credit) as balance_vnd
FROM account_move_line aml
JOIN account_account acc ON acc.id = aml.account_id
JOIN res_partner partner ON partner.id = aml.partner_id
WHERE acc.code LIKE '131%'
  AND aml.parent_state = 'posted'
GROUP BY partner.name
HAVING SUM(aml.debit - aml.credit) <> 0
ORDER BY balance_vnd DESC;
```

---

## 9. Lưu ý quan trọng

1. **TT133 KHÁC TT200**: TT200 dùng 627 (CP SX chung), 641 (CP bán hàng), 642 (CP QLDN) riêng. TT133 gộp tất cả vào **642**.
2. **TT133 không có TK 627** — DNNVV không bóc tách CP SXC riêng.
3. **TT133 không có TK 412** (chênh lệch đánh giá lại tài sản) — DNNVV không định giá lại.
4. **TT133 có 821** (CP thuế TNDN) gộp, không tách 8211/8212 như TT200.
5. **Số dư TK 421** = **4211** (năm trước) + **4212** (năm nay), kết chuyển cuối năm tài chính.

Khi audit Odoo, **verify l10n_vn module có đúng template TT133 không**, vì nhiều phiên bản cũ chỉ có TT200.
