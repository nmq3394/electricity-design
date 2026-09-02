# Requirement — Tìm kiếm hợp đồng cung cấp điện

## 1. Tổng quan

Xây dựng chức năng tìm kiếm hợp đồng cung cấp điện.

Người dùng nội bộ có thể nhập một hoặc nhiều điều kiện tìm kiếm để tra cứu thông tin hợp đồng điện của khách hàng.

---

## 2. Mục đích

Nhân viên nghiệp vụ cần tìm nhanh hợp đồng của khách hàng để:

- kiểm tra trạng thái hợp đồng;
- xem thông tin khách hàng;
- xác nhận địa điểm sử dụng điện;
- mở màn hình chi tiết hợp đồng để thực hiện các nghiệp vụ tiếp theo.

---

## 3. Người sử dụng

Đối tượng sử dụng:

- Nhân viên nghiệp vụ điện lực
- Nhân viên chăm sóc khách hàng

Người dùng phải đăng nhập vào hệ thống trước khi sử dụng chức năng.

---

## 4. Điều kiện tìm kiếm

Màn hình cho phép nhập các điều kiện sau:

| Item | Bắt buộc | Mô tả |
| --- | --- | --- |
| Contract ID | Không | ID hợp đồng |
| Customer Name | Không | Tên khách hàng |
| Supply Point Number | Không | Mã điểm cung cấp điện |
| Contract Status | Không | Trạng thái hợp đồng |
| Contract Start Date From | Không | Ngày bắt đầu hợp đồng từ |
| Contract Start Date To | Không | Ngày bắt đầu hợp đồng đến |

### Contract Status

Các giá trị:

- ACTIVE
- SUSPENDED
- TERMINATED

---

## 5. Quy tắc tìm kiếm

### 5.1 Contract ID

Nếu nhập Contract ID:

- tìm kiếm chính xác theo Contract ID.

Ví dụ:

Input:

CON000123

Chỉ trả về:

CON000123

Không trả về:

CON0001234

---

### 5.2 Customer Name

Customer Name được phép tìm kiếm partial match.

Ví dụ:

Input:

Nguyen

Có thể match:

Nguyen Van A
Nguyen Minh B

---

### 5.3 Supply Point Number

Supply Point Number tìm kiếm chính xác.

---

### 5.4 Contract Status

Nếu người dùng chọn Contract Status thì chỉ trả về hợp đồng có trạng thái tương ứng.

---

### 5.5 Contract Start Date

Nếu nhập:

Contract Start Date From = 2026-01-01
Contract Start Date To   = 2026-03-31

thì chỉ lấy hợp đồng có ngày bắt đầu nằm trong khoảng:

2026-01-01 <= Contract Start Date <= 2026-03-31

---

## 6. Validation

### 6.1 Không nhập điều kiện nào

Không cho phép search khi tất cả điều kiện đều trống.

Hiển thị message:

"Vui lòng nhập ít nhất một điều kiện tìm kiếm."

---

### 6.2 Date range không hợp lệ

Nếu:

Contract Start Date From > Contract Start Date To

thì không thực hiện search.

Hiển thị message:

"Ngày bắt đầu From không được lớn hơn ngày To."

---

### 6.3 Contract ID

Contract ID:

- tối đa 20 ký tự;
- chỉ cho phép chữ cái và số.

---

### 6.4 Supply Point Number

Supply Point Number:

- tối đa 22 ký tự;
- chỉ cho phép số.

---

## 7. Kết quả tìm kiếm

Hiển thị danh sách hợp đồng dưới dạng table.

Các column:

| Column | Nội dung |
| --- | --- |
| Contract ID | ID hợp đồng |
| Customer Name | Tên khách hàng |
| Supply Point Number | Mã điểm cung cấp |
| Supply Address | Địa chỉ sử dụng điện |
| Contract Type | Loại hợp đồng |
| Contract Status | Trạng thái |
| Contract Start Date | Ngày bắt đầu |
| Contract End Date | Ngày kết thúc |
| Updated At | Thời gian cập nhật cuối |

---

## 8. Sắp xếp kết quả

Mặc định:

Updated At DESC

Hợp đồng được cập nhật gần nhất hiển thị trước.

---

## 9. Giới hạn kết quả

Một lần search chỉ trả về tối đa:

100 records

Nếu kết quả thực tế lớn hơn 100 records:

- chỉ hiển thị 100 records đầu tiên;
- hiển thị warning:

"Có hơn 100 kết quả. Vui lòng thêm điều kiện tìm kiếm."

---

## 10. Không có dữ liệu

Nếu không tìm thấy hợp đồng phù hợp:

Hiển thị:

"Không tìm thấy dữ liệu phù hợp."

Không coi đây là system error.

---

## 11. Chuyển sang chi tiết hợp đồng

Khi người dùng click vào Contract ID trong kết quả:

Chuyển sang màn hình:

Contract Detail

với Contract ID tương ứng.

Ví dụ:

Search Result

CON000123 | Nguyen Van A

User click:

CON000123

→ mở Contract Detail của CON000123.

---

## 12. Luồng nghiệp vụ

1. Người dùng mở màn hình Contract Search.
2. Hệ thống hiển thị form tìm kiếm.
3. Người dùng nhập điều kiện.
4. Người dùng bấm Search.
5. Hệ thống validate input.
6. Nếu validation error:
   - hiển thị error;
   - không thực hiện search.
7. Nếu validation OK:
   - tìm kiếm dữ liệu hợp đồng.
8. Hệ thống hiển thị kết quả.
9. Người dùng có thể click Contract ID để xem chi tiết.

---

## 13. Error

Nếu xảy ra lỗi hệ thống trong quá trình search:

Hiển thị:

"Đã xảy ra lỗi hệ thống. Vui lòng thử lại."

Không hiển thị:

- SQL error;
- stack trace;
- thông tin database;

cho người dùng.

---

## 14. Performance

Trong điều kiện bình thường:

Search result phải được trả về trong vòng:

3 giây.

---

## 15. Acceptance Criteria

### AC-01

Given:

Người dùng nhập Contract ID hợp lệ.

When:

Bấm Search.

Then:

Hệ thống trả về hợp đồng có Contract ID tương ứng.

---

### AC-02

Given:

Người dùng nhập một phần Customer Name.

When:

Bấm Search.

Then:

Hệ thống trả về các khách hàng có tên chứa keyword đó.

---

### AC-03

Given:

Người dùng không nhập điều kiện nào.

When:

Bấm Search.

Then:

Hệ thống hiển thị validation error và không thực hiện search.

---

### AC-04

Given:

From Date lớn hơn To Date.

When:

Bấm Search.

Then:

Hệ thống hiển thị validation error.

---

### AC-05

Given:

Search có hơn 100 kết quả.

When:

Hệ thống xử lý search.

Then:

Chỉ trả về tối đa 100 records và hiển thị warning.

---

### AC-06

Given:

Không tồn tại dữ liệu phù hợp.

When:

Search hoàn thành.

Then:

Hiển thị "Không tìm thấy dữ liệu phù hợp."

---

### AC-07

Given:

Kết quả search được hiển thị.

When:

Người dùng click Contract ID.

Then:

Hệ thống chuyển sang Contract Detail của hợp đồng tương ứng.
