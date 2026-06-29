# Checklist Test — Inventory + Lot + COA + MSDS

> Module: **Inventory**, **Quality**, **Documents**
> Đặc thù: hàng lỏng/bao, đa UoM quy về KG, lot bắt buộc, COA/MSDS

---

## A. Cấu hình UoM (Unit of Measure)

| # | Test case | Expected | Mức |
|---|---|---|---|
| A01 | UoM mặc định nội bộ = KG cho TẤT CẢ sản phẩm | 100% master data | 🔴 |
| A02 | UoM Category "Weight" có KG, gam, tấn; "Volume" có lít, m³ | Quy đổi đúng giữa các UoM cùng category | 🔴 |
| A03 | Quy đổi cross-category (lít → kg) phải thông qua tỉ trọng product-level | KHÔNG dùng global conversion, vì mỗi hóa chất tỉ trọng khác | 🔴 |
| A04 | Sản phẩm có UoM phụ "Can 25L" — chỉ áp dụng cho sản phẩm cụ thể | Hệ thống quản lý UoM theo product, không chung | 🔴 |
| A05 | Sửa tỉ trọng sau khi đã có giao dịch — chặn hay cảnh báo? | Chặn cứng, vì sẽ làm sai lịch sử | 🔴 |

## B. Lot/Serial — Quản lý lô

| # | Test case | Expected | Mức |
|---|---|---|---|
| B01 | Nhập kho — bắt buộc có lot (Tracking by Lot) | Không cho confirm receipt nếu thiếu lot | 🔴 |
| B02 | Đặt tên lot theo format: [Product]-[YYYYMMDD]-[Seq] | Theo SOP DN, có validation regex | 🟠 |
| B03 | Lot có ngày sản xuất, ngày hết hạn — bắt buộc | Form chặn nếu thiếu | 🔴 |
| B04 | Cảnh báo lot sắp hết hạn (30/60/90 ngày trước) | Dashboard hoặc email cảnh báo | 🟠 |
| B05 | Xuất lot đã hết hạn | Chặn cứng, ghi log | 🔴 |
| B06 | Một lot dùng cho nhiều khách hàng — truy xuất ngược được? | Từ 1 lot → list các DO/Invoice đã dùng | 🔴 |
| B07 | Gộp 2 lot thành 1 (merge) — có cho phép không? | Theo policy, thường KHÔNG cho merge | 🟠 |
| B08 | Split lot (chia nhỏ) khi đóng gói lại | Có audit trail, COA gốc vẫn link được | 🟠 |

## C. COA (Certificate of Analysis)

| # | Test case | Expected | Mức |
|---|---|---|---|
| C01 | Upload COA (PDF) lên Lot trước khi cho xuất | Không cho xuất nếu thiếu COA | 🔴 |
| C02 | COA có version (v1, v2) khi sửa | Lịch sử version đầy đủ, không ghi đè | 🔴 |
| C03 | Tự động đính kèm COA vào email gửi KH khi confirm DO | Email có attachment COA của đúng lot xuất | 🔴 |
| C04 | COA file quá lớn (> 50MB) qua Cloudflare Tunnel | Test thực tế, có timeout không | 🟠 |
| C05 | KH yêu cầu COA của 1 lot đã giao 6 tháng trước | Truy xuất được từ Invoice/DO → Lot → COA | 🔴 |
| C06 | Phân quyền: Ai upload COA? Ai duyệt? Ai xem? | Tester ≠ Approver, có SoD | 🟠 |

## D. MSDS (Material Safety Data Sheet)

| # | Test case | Expected | Mức |
|---|---|---|---|
| D01 | MSDS gắn vào Product (không phải Lot) | Đúng, vì MSDS theo loại hóa chất | 🟠 |
| D02 | MSDS có version, có ngày hiệu lực | Hết hạn → cảnh báo cập nhật | 🟠 |
| D03 | Tự gửi MSDS lần đầu cho KH mới mua sản phẩm | Email tự động đính kèm | 🟡 |
| D04 | MSDS theo ngôn ngữ: VN, EN, CN | Sản phẩm có thể có nhiều file MSDS theo region | 🟡 |

## E. FIFO & Định giá tồn kho

| # | Test case | Expected | Mức |
|---|---|---|---|
| E01 | Costing Method = FIFO theo product | Verify trên product form | 🔴 |
| E02 | Nhập 3 lot khác giá → xuất 1 lô → giá vốn theo lot cũ nhất | SQL verify: bút toán 632 đúng giá lot | 🔴 |
| E03 | Lot cũ hết → tự chuyển sang lot tiếp theo | Không bị âm, không treo | 🔴 |
| E04 | Landed cost (cước vận chuyển nhập khẩu) phân bổ vào lot nhập | Giá vốn lot tăng đúng | 🔴 |
| E05 | Adjusting cost sau khi đã xuất bán | Bút toán điều chỉnh đúng (632 hay 156?) | 🔴 |
| E06 | Real-time inventory valuation vs Periodic | Verify chế độ DN đang dùng, đúng TT133 | 🔴 |

## F. Kiểm kê (Stock Take)

| # | Test case | Expected | Mức |
|---|---|---|---|
| F01 | Kiểm kê định kỳ tháng — tạo Physical Inventory | Lock kho, không cho giao dịch khác trong khi kiểm | 🟠 |
| F02 | Chênh lệch thiếu — bút toán Nợ 632 / Có 156 (theo TT133) | Đúng tài khoản, đúng kỳ | 🔴 |
| F03 | Chênh lệch thừa — bút toán Nợ 156 / Có 711 (thu nhập khác) | Đúng tài khoản | 🔴 |
| F04 | Chênh lệch lớn (> 5%) — yêu cầu phê duyệt cấp cao | Workflow rõ ràng | 🟠 |
| F05 | Lý do chênh lệch — bắt buộc nhập | Field required | 🟠 |

## G. Vị trí kho (Locations)

| # | Test case | Expected | Mức |
|---|---|---|---|
| G01 | Đa kho: Kho Bình Dương, Kho Long An | Tồn riêng từng kho, báo cáo tổng | 🟠 |
| G02 | Internal Transfer giữa 2 kho | Không sinh bút toán doanh thu/giá vốn, chỉ chuyển vị trí | 🔴 |
| G03 | Khu vực đặc biệt: Hàng đã bán chờ xuất (Customer Lien) | Tách riêng, không nhầm với tồn khả dụng | 🟠 |
| G04 | Khu cách ly hàng lỗi (Quality Hold) | Không xuất bán được | 🔴 |

## H. Vận chuyển & Landed Cost

| # | Test case | Expected | Mức |
|---|---|---|---|
| H01 | Tạo Landed Cost cho lô nhập nhập khẩu (cước biển + thuế NK) | Phân bổ theo trọng lượng, giá trị, hay tự định nghĩa | 🔴 |
| H02 | Landed cost sau khi đã bán một phần lot | Phần đã bán: bút toán điều chỉnh; phần còn: cộng vào giá lot | 🔴 |
| H03 | Phí vận chuyển nội địa cho KH — có vào giá vốn không? | Tùy policy; nếu có thì verify | 🟠 |

## I. Báo cáo Inventory

| # | Test case | Expected | Mức |
|---|---|---|---|
| I01 | Báo cáo tồn kho theo lot, theo kho, theo ngày | Đối chiếu SQL gốc | 🔴 |
| I02 | Báo cáo nhập xuất tồn (S11-DNN theo TT133) | Format đúng VN | 🔴 |
| I03 | Báo cáo vòng quay hàng tồn kho | Inventory Turnover = COGS / Avg Inventory | 🟠 |
| I04 | Tồn kho ngày = Tồn cuối kỳ / COGS bình quân ngày | Cảnh báo > 90 ngày → ứ đọng | 🟠 |
| I05 | Báo cáo lot sắp hết hạn — dashboard CEO | List + email tự động | 🟠 |

## J. Edge cases

| # | Test case | Expected | Mức |
|---|---|---|---|
| J01 | Đặt order khi tồn = 0 và route MTO | Tự sinh PO hoặc MO | 🟠 |
| J02 | Cho phép tồn âm? | KHÔNG cho phép với hóa chất, chặn cứng | 🔴 |
| J03 | Sản phẩm bị deactive — còn tồn kho thì sao? | Cảnh báo trước khi deactive | 🟠 |
| J04 | Import 10.000 lot từ Excel | Performance < 10 phút, không lỗi memory | 🟡 |
| J05 | Concurrent: 2 người xuất cùng 1 lot cùng lúc | Lock pessimistic, người sau báo lỗi | 🟠 |
