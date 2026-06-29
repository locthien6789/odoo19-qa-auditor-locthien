# Checklist Test — Sales B2B Hóa chất

> Module: **Sales** + **CRM** + tích hợp Pricelist, Inventory, Invoicing
> Đặc thù: Hàng lỏng/bao, đa UoM quy về KG, pricelist phức tạp, công nợ B2B

---

## A. Master Data — Sản phẩm hóa chất

| # | Test case | Expected | Mức |
|---|---|---|---|
| A01 | Tạo sản phẩm HCl 32% với UoM mặc định KG, UoM phụ Can 25L | Tỉ trọng 1.16 → 1 can = 29 KG, hệ thống quy đổi đúng 2 chiều | 🔴 |
| A02 | Tạo sản phẩm có nhiều nồng độ (HCl 32%, HCl 35%) — mỗi nồng độ phải là 1 product riêng | KHÔNG dùng variant, vì tỉ trọng khác → quy đổi khác | 🔴 |
| A03 | Bắt buộc trường: CAS Number, MSDS file, UN Number (vận chuyển), Hazard Class | Form không cho save nếu thiếu | 🟠 |
| A04 | Sản phẩm bao 25kg — UoM mặc định KG, bao = đóng gói | 1 bao = 25 KG, không cần tỉ trọng | 🟡 |
| A05 | Đổi UoM mặc định sau khi đã có giao dịch | Hệ thống chặn hoặc cảnh báo mạnh | 🔴 |

## B. Pricelist B2B phức tạp

| # | Test case | Expected | Mức |
|---|---|---|---|
| B01 | KH thường mua HCl 32% — apply giá list chuẩn | Giá list × số KG | 🔴 |
| B02 | KH VIP có pricelist riêng — đặt 500 KG | Giá VIP áp dụng, không phải giá list | 🔴 |
| B03 | KH VIP đặt > 1000 KG — có tier discount thêm 5% | Áp tier cao nhất phù hợp | 🔴 |
| B04 | Pricelist VIP đã hết hạn ngày hôm qua — đặt hàng hôm nay | Fallback về pricelist mặc định, KHÔNG báo lỗi câm | 🟠 |
| B05 | KH có hợp đồng khung 1000 tấn/năm — đã mua 950 tấn, đặt 100 tấn | Cảnh báo vượt cam kết, vẫn cho đặt nhưng cần phê duyệt | 🟠 |
| B06 | Pricelist tính theo Total Amount (đơn hàng > 50 triệu giảm 3%) | Tính sau khi cộng VAT hay trước? — verify theo policy DN | 🟠 |
| B07 | Promotion code + Pricelist VIP — có cộng dồn không? | Theo policy: chỉ áp 1, lấy giảm nhiều hơn | 🟡 |

## C. Sales Order — Tạo & duyệt

| # | Test case | Expected | Mức |
|---|---|---|---|
| C01 | Tạo SO với UoM Can 25L, hệ thống tự tính KG | Quantity (KG) = Quantity (Can) × 29 (theo HCl 32%) | 🔴 |
| C02 | Đặt 2.5 can (số lẻ) — quy đổi KG có làm tròn không? | Làm tròn theo policy (1 chữ số thập phân hay nguyên KG?) | 🟠 |
| C03 | KH có công nợ quá hạn 90 ngày — tạo SO mới | Cảnh báo + chặn confirm cho đến khi Manager duyệt | 🔴 |
| C04 | KH vượt hạn mức tín dụng — cố confirm SO | Chặn, hiển thị thông báo rõ "Vượt X% hạn mức" | 🔴 |
| C05 | Sửa giá SO sau khi đã confirm — quyền ai? | Chỉ Sales Manager + lý do sửa, ghi log | 🔴 |
| C06 | Đặt hàng có sẵn trong kho nhưng đang giữ cho SO khác | Cảnh báo "Available < Reserved", không cho confirm vượt | 🟠 |
| C07 | Đặt hàng không có trong kho — chấp nhận MTO (make-to-order) | Tự sinh PO hoặc MO theo route | 🟠 |
| C08 | Đặt cọc 30% trước, giao 2 đợt | Bút toán đặt cọc TK 131x; giao đợt 1 ghi nhận DT đúng tỉ lệ? | 🔴 |
| C09 | KH yêu cầu xuất hóa đơn dồn cuối tháng (consolidation) | Có gom được nhiều DO vào 1 hóa đơn không? | 🟠 |

## D. Delivery & xuất kho

| # | Test case | Expected | Mức |
|---|---|---|---|
| D01 | Xuất kho theo FIFO — lot cũ nhất ra trước | Hệ thống đề xuất đúng lot, có quyền override không? | 🔴 |
| D02 | Xuất 1 SO từ nhiều lot khác nhau | Mỗi lot 1 dòng, COA đính kèm đủ | 🔴 |
| D03 | Lot sắp hết hạn (còn 30 ngày) — cảnh báo? | Có cảnh báo, có policy ưu tiên xuất trước? | 🟠 |
| D04 | Xuất sai lot (lot chưa duyệt QC) | Chặn, yêu cầu chọn lot khác | 🔴 |
| D05 | Backorder — giao 70% đợt 1, 30% đợt 2 | Tạo DO con tự động, không sót | 🟠 |
| D06 | Hủy DO sau khi đã xuất kho | Tự đảo bút toán + nhập lại kho đúng lot? | 🔴 |
| D07 | In phiếu xuất kho — có đủ: lot, KG quy đổi, COA reference | Template VN chuẩn | 🟠 |

## E. Trả hàng (Return)

| # | Test case | Expected | Mức |
|---|---|---|---|
| E01 | KH trả 100 KG do sai chất lượng | Tạo Return Picking → nhập kho lại lot gốc | 🔴 |
| E02 | Hóa đơn đã phát hành — cần điều chỉnh giảm | Sinh Credit Note + HĐĐT điều chỉnh, gửi CQT | 🔴 |
| E03 | Trả hàng không thể nhập lại kho (hàng hỏng) | Xuất hủy, ghi nhận chi phí vào đâu? (TK 642 hay 811?) | 🔴 |
| E04 | Trả 1 phần lot, lot còn lại vẫn xuất tiếp | Tồn kho theo lot phải chính xác | 🟠 |

## F. Tích hợp với Invoicing & Accounting

| # | Test case | Expected | Mức |
|---|---|---|---|
| F01 | SO confirm → có sinh bút toán dự thu không? | Theo policy TT133, có thể không sinh; verify | 🟠 |
| F02 | DO done → bút toán: Nợ 632 / Có 156 (giá vốn) | Đúng giá lot xuất theo FIFO | 🔴 |
| F03 | Invoice posted → Nợ 131 / Có 511 / Có 3331 (VAT) | Số tiền, % VAT đúng | 🔴 |
| F04 | Payment registered → Nợ 111/112 / Có 131 | Đối trừ đúng hóa đơn nào | 🔴 |
| F05 | Hóa đơn xuất quá thời hạn 5 ngày kể từ giao hàng | Cảnh báo vi phạm quy định | 🟠 |
| F06 | VAT 8% (theo NQ 142/2024) — sản phẩm nào được giảm? | Verify danh mục sản phẩm hóa chất có/không được giảm | 🔴 |

## G. Báo cáo & KPI Sales

| # | Test case | Expected | Mức |
|---|---|---|---|
| G01 | Dashboard CEO: doanh thu hôm nay/tuần/tháng có realtime? | < 10 giây refresh | 🟠 |
| G02 | Top 10 khách hàng theo doanh thu — chính xác? | Đối chiếu với SQL trực tiếp | 🟠 |
| G03 | Báo cáo công nợ theo tuổi nợ (aging): 0-30, 31-60, 61-90, >90 | Số khớp với sổ chi tiết TK 131 | 🔴 |
| G04 | Biên lợi nhuận gộp theo sản phẩm | (Giá bán - Giá vốn FIFO) / Giá bán | 🟠 |
| G05 | Forecast doanh thu tháng — có dùng dữ liệu CRM pipeline? | Logic forecast minh bạch | 🟡 |

## H. Edge cases & Negative

| # | Test case | Expected | Mức |
|---|---|---|---|
| H01 | Tạo SO với số lượng âm | Chặn ngay khi nhập | 🟠 |
| H02 | Tạo SO với số lượng 0 | Chặn, hoặc cảnh báo | 🟡 |
| H03 | Tạo SO với customer chưa active | Chặn | 🟠 |
| H04 | Confirm SO 2 lần liên tiếp (double click) | Chỉ tạo 1 SO, không double | 🔴 |
| H05 | Xóa SO đã confirm | Chỉ cho cancel, không cho delete; audit log đầy đủ | 🔴 |
| H06 | Import SO từ Excel với data lỗi (UoM sai, KH không tồn tại) | Báo lỗi rõ ràng, không import phần đúng nửa vời | 🟠 |
| H07 | Tạo SO với 200 dòng sản phẩm — performance? | < 5 giây save | 🟡 |
