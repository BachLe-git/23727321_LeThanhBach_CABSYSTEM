| TS ID     | Test Scenario                            | Requirement bao phủ           | Ví dụ Test Case có thể sinh                                                                 |
| --------- | ---------------------------------------- | ----------------------------- | ------------------------------------------------------------------------------------------- |
| **TS-01** | Quản lý tài khoản và xác thực người dùng | FR-01 → FR-03, FR-26          | Đăng ký hợp lệ, email trùng, thiếu dữ liệu, đăng nhập đúng/sai, đăng xuất, cập nhật profile |
| **TS-02** | Tạo và quản lý Booking                   | FR-04 → FR-05                 | Tạo booking hợp lệ, thiếu thông tin, địa chỉ không hợp lệ, xem trạng thái, hủy booking      |
| **TS-03** | Tìm kiếm và phân công Driver             | FR-07 → FR-12                 | Tìm Driver, ranking, Driver Accept, Reject, Timeout, không có Driver                        |
| **TS-04** | Thực hiện và theo dõi Trip               | FR-05, FR-06, FR-19, BR-05/06 | Cập nhật trạng thái, xem vị trí, trạng thái hợp lệ/không hợp lệ, hoàn thành Trip            |
| **TS-05** | Tính cước và thanh toán                  | FR-13 → FR-16                 | Tính fare, Cash, Online Payment, thanh toán thất bại, retry, timeout                        |
| **TS-06** | Gửi và xử lý Notification                | FR-17 → FR-18                 | Thông báo booking, Driver nhận chuyến, trip hoàn thành, notification lỗi, retry             |
| **TS-07** | Đánh giá chuyến đi                       | Rating, BR-09                 | Đánh giá hợp lệ, rating ngoài phạm vi, đánh giá trip chưa hoàn thành, đánh giá 2 lần        |
| **TS-08** | Quản lý vận hành                         | FR-19 → FR-22                 | Quản lý Customer, Driver, Vehicle, theo dõi Trip, tra cứu Transaction                       |
| **TS-09** | Báo cáo và thống kê                      | FR-23 → FR-25                 | Báo cáo Trip, doanh thu, hiệu suất Driver, lọc theo thời gian                               |
| **TS-10** | Phân quyền, Audit Log và bảo mật         | FR-27 → FR-28                 | Customer/Driver/Staff/Admin truy cập chức năng, từ chối quyền, ghi Audit Log                |
| **TS-11** | Hiệu năng và khả năng chịu lỗi           | NFR-01, NFR-02, BR-12         | API chậm, nhiều request, Payment lỗi, Notification lỗi, mất kết nối                         |
