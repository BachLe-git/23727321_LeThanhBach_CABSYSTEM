Bước 1: đọc và phân tích yêu cầu sơ khởi của khách hàng ở giai đoạn 1
Dưới đây là phân tích yêu cầu sơ khởi (Preliminary Requirement Analysis) cho dự án CAB System – Nền tảng đặt xe dưới góc nhìn Business Analyst. Ở giai đoạn này, mục tiêu là xác định phạm vi, tác nhân, quy trình, yêu cầu chính, quy tắc nghiệp vụ, ngoại lệ và các điểm cần làm rõ, chưa đi sâu vào thiết kế kỹ thuật chi tiết.

1. Tổng quan dự án
1.1. Bối cảnh
Công ty ABC đang sử dụng hệ thống đặt xe hiện tại thông qua:

Tổng đài.
Ứng dụng đơn giản.
Phân công tài xế còn nhiều thao tác thủ công.
Khách hàng khó theo dõi trạng thái chuyến.
Thanh toán chưa được quản lý tập trung.
Khả năng mở rộng và tích hợp còn hạn chế.
Do đó, ABC muốn xây dựng CAB System như một nền tảng đặt xe mới, hỗ trợ toàn bộ vòng đời của chuyến đi:

Đặt xe → Tìm tài xế → Phân công → Thực hiện chuyến → Tính cước → Thanh toán → Đánh giá → Báo cáo

1.2. Mục tiêu nghiệp vụ
Mục tiêu	Ý nghĩa
Số hóa quy trình đặt xe	Giảm phụ thuộc vào tổng đài và thao tác thủ công
Tự động tìm tài xế	Rút ngắn thời gian phân công
Theo dõi chuyến theo thời gian thực	Tăng trải nghiệm khách hàng
Quản lý thanh toán tập trung	Dễ kiểm soát doanh thu/giao dịch
Hỗ trợ vận hành	Nhân viên có thể giám sát và xử lý sự cố
Có khả năng mở rộng	Hỗ trợ thêm dịch vụ, thanh toán, notification...
Tăng khả năng kiểm soát	Phân quyền, audit log, bảo mật dữ liệu
Cung cấp dữ liệu quản trị	Hỗ trợ báo cáo và ra quyết định

2. Phạm vi hệ thống sơ khởi
2.1. In Scope
Dự kiến CAB System bao gồm các nhóm chức năng:

Quản lý tài khoản
Quản lý hồ sơ khách hàng
Quản lý tài xế
Quản lý phương tiện
Đặt xe
Tìm kiếm và phân công tài xế
Theo dõi chuyến đi
Quản lý trạng thái chuyến
Tính cước
Thanh toán
Thông báo
Đánh giá tài xế
Quản trị vận hành
Quản lý giao dịch
Báo cáo
Phân quyền
Audit log
Quản lý vị trí tài xế
2.2. Out of Scope / chưa xác định
Một số nội dung chưa nên đưa thành requirement chính thức vì khách hàng chưa chốt:

Công thức tính cước cụ thể.
Thuật toán matching tài xế.
Tiêu chí ưu tiên tài xế.
Thời gian tài xế phải phản hồi.
Chính sách hủy.
Chính sách hoàn tiền.
Quy trình xử lý mất kết nối.
Thời gian lưu trữ dữ liệu.
Nhà cung cấp payment cụ thể.
Nhà cung cấp notification cụ thể.
Các loại dịch vụ/loại xe ngoài phạm vi hiện tại.
Các quy định pháp lý cụ thể cần áp dụng.
Đây là Open Issues / Pending Clarification, không nên tự suy đoán thành requirement.

3. Các tác nhân (Actors)
Có thể xác định sơ bộ các actor sau:

Actor 1 – Customer
Khách hàng sử dụng nền tảng để:

Đăng ký/đăng nhập.
Quản lý hồ sơ.
Đặt xe.
Theo dõi chuyến.
Thanh toán.
Xem lịch sử.
Đánh giá tài xế.
Actor 2 – Driver
Tài xế:

Quản lý hồ sơ.
Quản lý phương tiện.
Chuyển trạng thái online/offline/available.
Nhận yêu cầu chuyến.
Chấp nhận/từ chối chuyến.
Cập nhật trạng thái chuyến.
Gửi/ghi nhận vị trí.
Actor 3 – Operation Staff
Nhân viên vận hành:

Quản lý customer.
Quản lý driver.
Quản lý vehicle.
Theo dõi chuyến.
Hỗ trợ xử lý chuyến lỗi.
Tra cứu giao dịch.
Actor 4 – Administrator
Có quyền cao hơn Operation Staff:

Quản lý tài khoản nhân viên.
Phân quyền.
Thực hiện các thao tác quản trị nhạy cảm.
Xem audit log.
Actor 5 – Payment Provider
Hệ thống thanh toán bên ngoài:

Tiếp nhận yêu cầu thanh toán.
Xử lý giao dịch.
Trả kết quả thành công/thất bại.
CAB không lưu trực tiếp dữ liệu nhạy cảm của thẻ/tài khoản.

Actor 6 – Notification Provider
Có thể là:

Push notification.
SMS.
Email.
Các kênh khác trong tương lai.
Nên thiết kế theo hướng có thể thay thế/mở rộng provider.

4. Quy trình nghiệp vụ tổng thể
Quy trình Happy Path có thể mô hình hóa như sau:

Customer
   │
   │ Tạo yêu cầu đặt xe
   ▼
CAB System
   │
   │ Kiểm tra thông tin
   ▼
Tìm tài xế phù hợp
   │
   ├── Không tìm được ──► Thông báo Customer
   │
   ▼
Gửi yêu cầu cho Driver
   │
   ├── Driver từ chối/timeout
   │          │
   │          ▼
   │     Tìm Driver khác
   │
   ▼
Driver chấp nhận
   │
   ▼
Driver đến điểm đón
   │
   ▼
Đã đón khách
   │
   ▼
Đang di chuyển
   │
   ▼
Hoàn thành chuyến
   │
   ▼
Tính cước
   │
   ▼
Thanh toán
   │
   ├── Thành công
   │
   └── Thất bại → Retry/Xử lý theo policy
   │
   ▼
Customer đánh giá
   │
   ▼
Hoàn tất

5. Vòng đời của chuyến đi
Đây là một phần rất quan trọng cần xác định ngay từ giai đoạn phân tích.

Có thể đề xuất trạng thái sơ bộ:

REQUESTED
    ↓
SEARCHING_DRIVER
    ↓
DRIVER_ASSIGNED
    ↓
DRIVER_ARRIVING
    ↓
DRIVER_ARRIVED
    ↓
PASSENGER_PICKED_UP
    ↓
IN_PROGRESS
    ↓
COMPLETED
    ↓
PAYMENT_PENDING
    ↓
PAID

Các trạng thái ngoại lệ:

CANCELLED
NO_DRIVER_FOUND
PAYMENT_FAILED
DRIVER_REJECTED
DRIVER_TIMEOUT
TRIP_FAILED

Lưu ý: Đây mới là đề xuất BA sơ khởi. Cần khách hàng xác nhận state machine chính thức.

6. Phân tích yêu cầu chức năng
FR-01. Quản lý tài khoản
Customer
Hệ thống phải cho phép:

Đăng ký.
Đăng nhập.
Đăng xuất.
Cập nhật thông tin cá nhân.
Có thể khôi phục tài khoản/mật khẩu nếu được thống nhất.
Driver
Đăng ký hoặc được Operation tạo tài khoản.
Đăng nhập.
Cập nhật hồ sơ.
Quản lý thông tin phương tiện.
Staff/Admin
Đăng nhập.
Quản lý tài khoản theo quyền được cấp.
7. FR-02. Đặt xe
Customer có thể:

Nhập điểm đón.
Nhập điểm đến.
Chọn loại xe/dịch vụ.
Xem thông tin dự kiến nếu có.
Gửi yêu cầu đặt xe.
Hệ thống cần:

Validate dữ liệu.
Tạo booking/trip.
Sinh mã chuyến.
Ghi nhận thời gian tạo.
Chuyển chuyến sang trạng thái tìm tài xế.
Gửi notification xác nhận tiếp nhận.
8. FR-03. Tìm và phân công tài xế
Đây là core business process.

Hệ thống phải:

Xác định các tài xế phù hợp.
Lọc theo:
Trạng thái sẵn sàng.
Vị trí.
Loại xe.
Tiêu chí vận hành.
Xếp hạng tài xế.
Gửi yêu cầu.
Chờ phản hồi.
Nếu tài xế:
Accept → assign.
Reject → tìm tài xế tiếp theo.
Timeout → tìm tài xế tiếp theo.
Nếu hết candidate:
Chuyển trạng thái NO_DRIVER_FOUND.
Thông báo khách hàng.
Điểm cần đặc biệt làm rõ
Không nên hard-code ngay:

"Luôn chọn tài xế gần nhất."

Thay vào đó, requirement nên ở mức:

Hệ thống xác định và ưu tiên tài xế phù hợp dựa trên bộ tiêu chí vận hành do doanh nghiệp cấu hình.

Sau đó BA cần xác định chính xác scoring/ranking.

9. FR-04. Quản lý vị trí tài xế
Hệ thống cần:

Nhận vị trí tài xế.
Lưu/ghi nhận vị trí theo chính sách.
Sử dụng vị trí để tìm tài xế.
Hỗ trợ tính ETA.
Cho phép vận hành theo dõi vị trí khi cần.
Cần làm rõ:

Tần suất gửi location.
Có lưu toàn bộ lịch sử GPS hay không.
Lưu trong bao lâu.
Khi driver offline thì xử lý location thế nào.
Có hiển thị vị trí chính xác cho customer không.
10. FR-05. Theo dõi chuyến
Customer có thể xem:

Đang tìm tài xế.
Tài xế đã nhận.
Thông tin tài xế.
Phương tiện.
ETA.
Tài xế đã đến.
Đã đón khách.
Đang di chuyển.
Hoàn thành.
Driver có thể cập nhật:

ACCEPTED
→ ARRIVED
→ PICKED_UP
→ IN_PROGRESS
→ COMPLETED

Hệ thống phải đảm bảo không cho phép chuyển trạng thái bất hợp lệ.

Ví dụ:

Không được phép chuyển SEARCHING_DRIVER → COMPLETED.

11. FR-06. Tính cước
Sau khi chuyến hoàn thành:

Trip information
       +
Service type
       +
Fare rules
       ↓
Fare Calculation
       ↓
Final Amount

Kết quả cần có:

Giá chuyến.
Các thành phần cấu thành giá nếu cần.
Tổng tiền.
Currency.
Thời điểm tính cước.
Đây là requirement chưa hoàn chỉnh
Cần khách hàng xác nhận:

Giá mở cửa?
Giá theo km?
Giá theo thời gian?
Phụ phí?
Surge pricing?
Phí cầu đường?
Khuyến mại?
Thuế?
Làm tròn?
Thay đổi giá trong lúc chuyến đang chạy?
12. FR-07. Thanh toán
Hỗ trợ:

Cash
Trip Completed
       ↓
Calculate Fare
       ↓
Cash Payment
       ↓
Paid

Electronic Payment
CAB
 ↓
Payment Provider
 ↓
Payment Result
 ↓
CAB

CAB không lưu:

Số thẻ đầy đủ.
CVV.
Thông tin nhạy cảm tương tự.
Có thể lưu các thông tin cần thiết như:

Payment transaction ID.
Provider.
Amount.
Status.
Timestamp.
Reference/token nếu phù hợp.
Payment failure
PAYMENT_FAILED
       ↓
Notify Customer
       ↓
Retry / Alternative Payment
       ↓
SUCCESS / FAILED

Cần xác định chính sách retry.

13. FR-08. Notification
Notification nên được thiết kế thành module/service độc lập.

Các sự kiện chính:

Event	Customer	Driver
Booking received	✓	
New trip request		✓
Driver accepted	✓	
Driver arriving	✓	
Driver arrived	✓	
Trip status changed	✓	✓
Trip completed	✓	✓
Payment result	✓	
Trip cancelled	✓	✓

Kiến trúc nên hỗ trợ:

CAB
 │
 ▼
Notification Service
 │
 ├── Push Provider
 ├── SMS Provider
 ├── Email Provider
 └── Future Provider

Như vậy khi thay SMS provider hoặc bổ sung Zalo/WhatsApp... không cần sửa toàn bộ business logic.

14. FR-09. Đánh giá tài xế
Sau khi chuyến hoàn thành:

Customer có thể:

Đánh giá tài xế.
Nhập rating.
Có thể nhập comment nếu doanh nghiệp cần.
Cần xác định:

Rating 1–5 hay hệ khác?
Có bắt buộc đánh giá không?
Có cho sửa rating không?
Có cho đánh giá nhiều lần không?
Có cho tài xế đánh giá khách hàng không?
15. FR-10. Quản trị vận hành
Operation Portal cần hỗ trợ:

Customer Management
Search customer.
Xem hồ sơ.
Xem lịch sử chuyến.
Xem trạng thái tài khoản.
Driver Management
Tạo tài khoản.
Cập nhật hồ sơ.
Active/Inactive.
Xem trạng thái online.
Xem vị trí.
Xem lịch sử chuyến.
Vehicle Management
Tạo phương tiện.
Cập nhật.
Gắn driver.
Active/Inactive.
Trip Management
Xem chuyến đang chạy.
Search trip.
Xem trạng thái.
Hỗ trợ xử lý trip lỗi.
Tra cứu lịch sử.
Transaction Management
Tra cứu payment.
Xem trạng thái.
Xem transaction history.
16. FR-11. Báo cáo
Ban lãnh đạo cần tối thiểu:

Operational metrics
Số chuyến.
Số chuyến hoàn thành.
Số chuyến hủy.
Tỷ lệ hoàn thành.
Tỷ lệ hủy.
Thời gian tìm tài xế.
Thời gian chờ tài xế.
Số tài xế hoạt động.
Revenue
Tổng doanh thu.
Doanh thu theo thời gian.
Doanh thu theo loại dịch vụ.
Doanh thu theo phương thức thanh toán.
Driver performance
Số chuyến.
Tỷ lệ accept.
Tỷ lệ reject.
Tỷ lệ hoàn thành.
Rating.
Thời gian hoạt động.
17. FR-12. Phân quyền
Nên áp dụng RBAC:

User
 ├── Customer
 ├── Driver
 ├── Operation Staff
 └── Administrator

Ví dụ:

Chức năng	Customer	Driver	Operation	Admin
Đặt xe	✓			
Nhận chuyến		✓		
Cập nhật trip		✓	✓	✓
Quản lý driver			✓	✓
Quản lý staff				✓
Phân quyền				✓
Xem audit log			hạn chế	✓
Xử lý nghiệp vụ nhạy cảm			hạn chế	✓

Cần xác định permission-level, không chỉ role-level.

18. FR-13. Audit Log
Các hành động quan trọng cần được lưu vết:

Login.
Tạo/sửa/xóa tài khoản.
Thay đổi quyền.
Thay đổi trạng thái driver.
Thay đổi trạng thái trip bởi staff.
Điều chỉnh giao dịch.
Hoàn tiền.
Thao tác quản trị nhạy cảm.
Audit log nên có:

Who
What
When
Where
Before
After
Result

19. Yêu cầu phi chức năng
Đây là phần rất quan trọng vì khách hàng nhấn mạnh khả năng mở rộng và tính ổn định.

NFR-01. Performance
Cần xác định:

Response time API.
Thời gian tạo booking.
Thời gian tìm driver.
Thời gian cập nhật trạng thái.
Số request/second.
Ví dụ requirement cần chuyển từ:

"Hệ thống phải nhanh."

thành:

"95% request đặt xe phải phản hồi trong ≤ X giây."

Con số X cần khách hàng xác nhận.

NFR-02. Scalability
Hệ thống phải có khả năng:

Scale từng component độc lập.
Xử lý peak demand.
Bổ sung server/service mà không downtime đáng kể.
Đặc biệt các thành phần có tải cao:

Booking
Driver Matching
Location
Notification
Payment

20. Availability & Resilience
Một yêu cầu nổi bật:

Payment hoặc Notification lỗi không được làm toàn bộ hệ thống booking ngừng hoạt động.

Ví dụ:

Booking Service
      │
      ├──────────► Matching Service
      │
      ├──────────► Notification Service
      │
      └──────────► Payment Service

Nếu Notification Service down:

Booking vẫn hoạt động
       │
       ▼
Notification retry / queue

Nếu Payment Provider down:

Trip vẫn được hoàn thành
       │
       ▼
Payment = PENDING / FAILED
       │
       ▼
Retry sau

Đây là một business resilience requirement quan trọng.

21. Security Requirements
Hệ thống cần:

Authentication
Customer phải authenticate.
Driver phải authenticate.
Staff/Admin phải authenticate.
Authorization
Kiểm tra quyền trước mỗi thao tác nhạy cảm.
Data protection
Bảo vệ:

PII.
Driver information.
Vehicle information.
Location data.
Transaction information.
Auditability
Ghi log hành động quan trọng.
Có khả năng truy vết sự cố.
Payment security
Không lưu sensitive payment data trực tiếp.
Sử dụng payment provider/tokenization theo thiết kế được thống nhất.
22. Các Business Rules sơ bộ
Có thể xây dựng danh sách BR như sau:

BR-01
Chỉ tài xế ở trạng thái phù hợp mới được nhận chuyến.

BR-02
Một chuyến chỉ được assign cho một tài xế tại một thời điểm.

BR-03
Nếu driver reject hoặc timeout, hệ thống tiếp tục tìm driver khác.

BR-04
Nếu không còn driver phù hợp, booking chuyển sang NO_DRIVER_FOUND.

BR-05
Chỉ driver được assign mới được cập nhật trạng thái thực hiện chuyến.

BR-06
Chỉ trip hợp lệ mới được chuyển sang COMPLETED.

BR-07
Fare chỉ được xác định theo fare policy được doanh nghiệp cấu hình.

BR-08
Trip completed phải có payment status tương ứng.

BR-09
Customer chỉ được đánh giá trip đã hoàn thành.

BR-10
Staff chỉ được thực hiện chức năng theo permission.

BR-11
Các thao tác quản trị nhạy cảm phải được audit.

BR-12
Payment failure không được làm mất dữ liệu trip.

23. Các trường hợp ngoại lệ cần phân tích
Đây là phần BA cần đặc biệt chú ý.

Case 1 – Không tìm được tài xế
Booking
 ↓
Search
 ↓
No Driver
 ↓
NO_DRIVER_FOUND
 ↓
Notify Customer

Cần xác định khách hàng có được:

Retry?
Chỉnh điểm đón?
Chờ tiếp?
Tự động search lại?
Case 2 – Driver không phản hồi
Offer Driver A
 ↓
Timeout
 ↓
Driver B

Cần xác định timeout bao nhiêu giây.

Case 3 – Driver từ chối
Tương tự:

Driver A Reject
       ↓
Driver B

Cần xác định driver A có được tiếp tục nhận request khác ngay không.

Case 4 – Hai driver cùng accept
Đây là một concurrency scenario rất quan trọng.

Ví dụ:

Driver A ── Accept ──┐
                     ├── Booking
Driver B ── Accept ──┘

Business rule:

Chỉ một driver được assign thành công.

Cần có cơ chế đảm bảo atomic assignment/concurrency control.

Case 5 – Payment thất bại
Cần xác định:

Retry bao nhiêu lần?
Retry tự động hay thủ công?
Có cho đổi payment method?
Trip có được coi là completed?
Có phát sinh khoản phải thu?
Case 6 – Mất mạng
Ví dụ driver:

Driver đang chạy
      ↓
Mất mạng
      ↓
Không gửi được location

Cần xác định:

Có lưu local state?
Reconnect gửi dữ liệu thế nào?
Bao lâu thì hệ thống đánh dấu driver mất kết nối?
Customer nhìn thấy trạng thái gì?
Case 7 – Driver mất kết nối sau khi accept
Đây là case nghiêm trọng:

Driver Accept
     ↓
Driver Offline

Hệ thống có:

Reassign?
Chờ driver?
Operation can thiệp?
Notify customer?
Cần business policy.

24. Các điểm chưa rõ cần BA xác nhận
Đây có thể xem là Question List gửi khách hàng.

#	Chủ đề	Câu hỏi cần xác nhận
1	Fare	Công thức tính giá cụ thể là gì?
2	Fare	Có giá tối thiểu không?
3	Fare	Có surge pricing không?
4	Fare	Có phụ phí/khuyến mãi không?
5	Driver	Tiêu chí chọn driver là gì?
6	Driver	Khoảng cách có phải tiêu chí chính không?
7	Driver	Có xét rating/accept rate không?
8	Driver	Driver phải phản hồi trong bao lâu?
9	Matching	Có retry bao nhiêu driver?
10	Matching	Có giới hạn thời gian tìm driver không?
11	Cancel	Ai được hủy chuyến?
12	Cancel	Hủy ở từng trạng thái có được không?
13	Cancel	Có phí hủy không?
14	Payment	Payment failure xử lý thế nào?
15	Payment	Retry bao nhiêu lần?
16	Payment	Có refund không?
17	Location	Tần suất cập nhật GPS?
18	Location	Lưu location bao lâu?
19	Notification	Kênh notification đầu tiên là gì?
20	Notification	Có fallback channel không?
21	Network	Mất mạng xử lý thế nào?
22	Data	Dữ liệu lưu trong bao lâu?
23	Security	Có yêu cầu MFA cho Admin không?
24	Report	Báo cáo real-time hay batch?
25	Scale	Peak concurrent users là bao nhiêu?
26	Scale	Số chuyến/ngày dự kiến?
27	Availability	SLA/uptime mục tiêu là bao nhiêu?
28	Deployment	Có yêu cầu zero-downtime không?
29	Service	Giai đoạn 1 có những loại xe nào?
30	Payment	Nhà cung cấp payment nào?

25. Phân loại mức độ ưu tiên
Do thời gian chỉ có 7 tuần, cần ưu tiên rõ ràng.

Có thể dùng MoSCoW:

Must Have – MVP
Đăng ký/đăng nhập.
Customer profile.
Driver profile.
Vehicle.
Tạo booking.
Tìm driver.
Accept/Reject/Timeout.
Trip lifecycle.
Driver location cơ bản.
Fare cơ bản.
Cash payment.
Electronic payment.
Notification cơ bản.
Trip history.
Rating.
Operation portal.
RBAC cơ bản.
Audit log cơ bản.
Báo cáo cơ bản.
Should Have
Advanced driver ranking.
Advanced analytics.
Notification fallback.
Advanced payment retry.
Advanced operational dashboard.
Could Have
Loyalty.
Promotion.
Corporate account.
Scheduled booking.
Multi-stop trip.
Dynamic pricing nâng cao.
Won't Have – Phase 1
Các chức năng chưa phục vụ mục tiêu MVP hoặc chưa có business rule rõ ràng.

26. Ước lượng phạm vi cho 7 tuần
Với thời gian 7 tuần, phạm vi hiện tại khá lớn. BA nên cảnh báo khách hàng rằng không nên triển khai toàn bộ khả năng "nền tảng lâu dài" ngay trong 7 tuần.

Có thể chia:

Tuần 1 – Analysis & Foundation
Requirement clarification.
Process.
Use case.
Business rules.
UX flow.
Architecture baseline.
Tuần 2 – Account & Master Data
Authentication.
Customer.
Driver.
Vehicle.
RBAC.
Tuần 3 – Booking & Matching
Create booking.
Driver availability.
Matching.
Accept/Reject/Timeout.
Tuần 4 – Trip & Location
Trip state.
Driver location.
Tracking.
Driver status.
Tuần 5 – Fare & Payment
Fare calculation.
Cash.
Payment provider.
Payment retry/error handling.
Tuần 6 – Notification & Operation
Notification.
Admin portal.
Trip monitoring.
Transaction lookup.
Audit.
Tuần 7 – Report, Integration, Testing & Deployment
Reports.
Integration testing.
UAT.
Performance/security testing.
Deployment.
Bug fixing.
Tuy nhiên, lịch này chỉ khả thi nếu requirements được chốt rất nhanh và phạm vi MVP được kiểm soát chặt.

27. Các Use Case chính
Có thể lập danh sách Use Case sơ bộ:

Customer
UC01 – Register Account
UC02 – Login
UC03 – Update Profile
UC04 – Create Booking
UC05 – Track Trip
UC06 – Cancel Booking
UC07 – View Trip History
UC08 – View Fare
UC09 – Make Payment
UC10 – Retry Payment
UC11 – Rate Driver
Driver
UC12 – Login
UC13 – Update Profile
UC14 – Manage Vehicle
UC15 – Set Availability
UC16 – Receive Trip Request
UC17 – Accept Trip
UC18 – Reject Trip
UC19 – Update Trip Status
UC20 – Update Location
Operation
UC21 – Manage Customer
UC22 – Manage Driver
UC23 – Manage Vehicle
UC24 – Monitor Trips
UC25 – Handle Trip Exception
UC26 – View Transactions
UC27 – View Reports
Admin
UC28 – Manage Staff
UC29 – Manage Roles/Permissions
UC30 – View Audit Log
UC31 – Configure Business Rules
System/External
UC32 – Calculate Fare
UC33 – Find Driver
UC34 – Send Notification
UC35 – Process Payment
UC36 – Record Driver Location
28. Kiến trúc nghiệp vụ sơ bộ
Từ yêu cầu "có khả năng mở rộng độc lập", có thể suy ra hệ thống nên được phân tách logic thành các domain/service:

                    ┌─────────────────┐
                    │ Customer App    │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │ API / Gateway   │
                    └────────┬────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
        ▼                    ▼                    ▼
   Account Service     Booking Service     Driver Service
                             │                    │
                             ▼                    ▼
                      Matching Service      Location Service
                             │
                ┌────────────┼────────────┐
                ▼            ▼            ▼
           Trip Service  Fare Service  Notification
                                             │
                                             ▼
                                      Notification Provider

                Trip
                  │
                  ▼
           Payment Service
                  │
                  ▼
          Payment Provider

                  │
                  ▼
           Operation Portal

Đây chưa phải kiến trúc kỹ thuật cuối cùng, nhưng là domain decomposition sơ bộ hữu ích cho BA và Architect.

29. Các rủi ro chính của dự án
Rủi ro	Mức độ	Giải pháp
Requirements chưa chốt	Cao	Workshop với stakeholder
Fare chưa rõ	Cao	Chốt fare rules trước development
Matching chưa rõ	Cao	Xác định ranking/timeout
7 tuần quá ngắn	Cao	Chốt MVP
Payment integration phụ thuộc bên ngoài	Cao	Mock/sandbox sớm
Location real-time phức tạp	Cao	Prototype sớm
Concurrency khi assign driver	Cao	Define business rule + technical solution
Network failure	Cao	Define exception flow
Notification failure	Trung bình	Async/retry
Scope creep	Cao	Change request process
Security	Cao	Security requirements ngay từ đầu

30. Kết luận BA sơ khởi
Từ yêu cầu của ABC, có thể kết luận CAB System không chỉ là một ứng dụng đặt xe, mà là một nền tảng quản lý toàn bộ vòng đời chuyến đi, với 5 domain nghiệp vụ cốt lõi:

Booking → Driver Matching → Trip → Payment → Operation

Trong đó, các điểm phức tạp nhất cần BA ưu tiên làm rõ là:

Driver Matching – chọn ai, ưu tiên thế nào, timeout bao lâu, reject xử lý ra sao.
Trip State Machine – các trạng thái hợp lệ và điều kiện chuyển trạng thái.
Fare Calculation – cách tính tiền.
Cancellation Policy – ai được hủy và phí thế nào.
Payment Failure – retry, pending, refund.
Location – tần suất và thời gian lưu.
Network Failure – đặc biệt với driver.
Scalability/SLA – số lượng người dùng, chuyến/giây, peak load.
RBAC & Audit – quyền của Operation vs Admin.
MVP trong 7 tuần – cần cắt scope hợp lý.
Deliverable BA nên tạo tiếp theo
Từ bản phân tích sơ khởi này, bộ tài liệu tiếp theo nên đi theo thứ tự:

1. Stakeholder Analysis
↓
2. Scope & Context Diagram
↓
3. Business Process / BPMN
↓
4. Use Case Diagram + Use Case Specification
↓
5. Functional Requirements (FRD/SRS)
↓
6. Non-functional Requirements
↓
7. Business Rules
↓
8. State Diagram cho Trip
↓
9. Activity/Sequence Flow cho Matching & Payment
↓
10. Acceptance Criteria / User Stories
↓
11. Requirement Traceability Matrix (RTM)
↓
12. UAT Scenarios

Nếu đây là bài BA case study/interview, thì điểm quan trọng nhất không phải chỉ liệt kê chức năng, mà phải thể hiện được rằng BA nhận diện được requirement chưa rõ, không tự ý giả định business rule, phát hiện exception/concurrency, kiểm soát scope 7 tuần và chuyển yêu cầu của khách hàng thành requirement có thể kiểm thử được.

Bước 2: Xác định các stackholder
Dựa trên yêu cầu CAB System, có thể xác định Stakeholder (bên liên quan) theo 3 nhóm: Business – Operational – Technical/External. Một số stakeholder là Actor trực tiếp, một số không trực tiếp sử dụng hệ thống nhưng có quyền lợi hoặc ảnh hưởng đến dự án.

1. Danh sách Stakeholder chính
Stakeholder	Vai trò	Mối quan tâm / Trách nhiệm chính	Mức độ ảnh hưởng
Ban giám đốc / Sponsor	Business Sponsor	Định hướng, ngân sách, mục tiêu kinh doanh, phê duyệt phạm vi	🔴 Rất cao
Product Owner / Business Owner	Đại diện nghiệp vụ	Xác định product vision, ưu tiên backlog, quyết định MVP	🔴 Rất cao
Business Analyst (BA)	Phân tích nghiệp vụ	Thu thập, phân tích, làm rõ và quản lý requirement	🔴 Cao
Operation Staff	Người dùng vận hành	Quản lý tài xế, chuyến, xử lý sự cố, giám sát hoạt động	🔴 Cao
Customer	Người sử dụng dịch vụ	Đặt xe, theo dõi chuyến, thanh toán, đánh giá	🟠 Cao
Driver	Người cung cấp dịch vụ	Nhận chuyến, thực hiện chuyến, cập nhật trạng thái/vị trí	🟠 Cao
Admin	Quản trị hệ thống	Quản lý user, role, permission, cấu hình	🔴 Cao
Finance / Accounting	Quản lý tài chính	Đối soát thanh toán, doanh thu, giao dịch, hoàn tiền	🟠 Cao
Customer Service / Call Center	Hỗ trợ khách hàng	Hỗ trợ booking, xử lý khiếu nại và trip lỗi	🟠 Trung bình-Cao
IT / System Admin	Vận hành hệ thống	Infrastructure, deployment, monitoring, availability	🔴 Cao
Development Team	Xây dựng hệ thống	Implement chức năng theo requirement	🟠 Cao
QA / Tester	Đảm bảo chất lượng	Kiểm thử chức năng, integration, performance, security	🟠 Cao
Security / Compliance	Bảo mật & tuân thủ	Bảo vệ PII, payment data, audit, access control	🔴 Cao
Payment Provider	Đối tác thanh toán	Xử lý giao dịch điện tử	🟠 Cao
Notification Provider	Đối tác thông báo	Push/SMS/Email notification	🟡 Trung bình
Data/BI Team	Phân tích dữ liệu	Báo cáo, dashboard, KPI	🟡 Trung bình

2. Phân tích chi tiết từng Stakeholder
2.1. Ban giám đốc / Sponsor
Vai trò: Người tài trợ và ra quyết định cấp cao.

Quan tâm đến:

Hệ thống có đáp ứng mục tiêu kinh doanh không?
Chi phí dự án.
Thời gian triển khai 7 tuần.
Khả năng mở rộng.
Doanh thu.
Tỷ lệ hoàn thành/hủy chuyến.
Hiệu quả tài xế.
Quyền quyết định:

Phạm vi lớn.
Budget.
Timeline.
Các thay đổi quan trọng.
Go-live.
2.2. Product Owner / Business Owner
Đây là stakeholder rất quan trọng đối với BA.

Vai trò:

Đại diện cho business.
Xác định product vision.
Quyết định ưu tiên tính năng.
Xác định MVP.
Accept/reject requirement.
Ví dụ:

Trong 7 tuần không thể làm tất cả chức năng, Product Owner quyết định "Scheduled Booking" đưa sang Phase 2 nhưng "Real-time Trip Tracking" phải có trong MVP.

2.3. Operation Staff
Vai trò: Người sử dụng hệ thống hàng ngày để vận hành dịch vụ.

Quan tâm:

Xem chuyến đang chạy.
Tìm tài xế.
Theo dõi driver online/offline.
Xử lý trip lỗi.
Hỗ trợ khách hàng.
Tra cứu giao dịch.
Đây là stakeholder mà BA nên phỏng vấn/workshop nhiều, vì họ hiểu các vấn đề thực tế của hệ thống hiện tại.

2.4. Customer
Vai trò: Người sử dụng dịch vụ và tạo doanh thu cho doanh nghiệp.

Cần:

Đăng ký/đăng nhập.
Đặt xe.
Theo dõi tài xế.
Biết ETA.
Thanh toán.
Xem lịch sử.
Đánh giá.
Pain point hiện tại:

Khó biết xe đang ở đâu và chuyến đang ở trạng thái nào.

Do đó real-time trip tracking là một nhu cầu quan trọng.

2.5. Driver
Vai trò: Người trực tiếp cung cấp dịch vụ vận chuyển.

Cần:

Nhận request.
Accept/Reject.
Xem thông tin điểm đón.
Cập nhật trạng thái.
Gửi vị trí.
Quản lý phương tiện.
Driver cũng ảnh hưởng trực tiếp đến:

Tỷ lệ nhận chuyến.
Thời gian tìm xe.
Tỷ lệ hoàn thành.
Chất lượng dịch vụ.
2.6. Admin
Vai trò: Quản trị hệ thống và quyền truy cập.

Ví dụ:

Admin
 ├── Manage Staff
 ├── Manage Roles
 ├── Manage Permissions
 ├── View Audit Logs
 └── System Configuration

Admin cần quyền cao hơn Operation Staff nhưng không nhất thiết có quyền sửa mọi business data.

Đây là điểm cần xác định rõ trong RBAC.

2.7. Finance / Accounting
Stakeholder này không được nêu trực tiếp trong đề bài nhưng nên được xác định vì hệ thống có payment và revenue.

Quan tâm:

Tổng doanh thu.
Giao dịch thành công/thất bại.
Đối soát payment.
Cash collection.
Refund.
Transaction history.
BA nên đưa Finance vào các workshop liên quan đến:

Fare → Payment → Refund → Reconciliation → Reporting

2.8. Customer Service / Call Center
Do hệ thống hiện tại vẫn có tổng đài, nhóm này rất quan trọng.

Họ cần:

Tra cứu booking.
Tra cứu khách hàng.
Xem trạng thái trip.
Hỗ trợ khi không tìm được tài xế.
Xử lý khiếu nại.
Hỗ trợ payment/trip lỗi.
Ví dụ:

Customer gọi tổng đài vì tài xế đã nhận nhưng không đến → CS cần tìm booking và xem trạng thái hiện tại.

2.9. IT / System Administrator
Vai trò: Đảm bảo hệ thống vận hành ổn định.

Quan tâm:

Availability.
Monitoring.
Logging.
Backup.
Deployment.
Scaling.
Incident management.
Đặc biệt yêu cầu:

Payment hoặc Notification lỗi không được làm toàn bộ hệ thống booking ngừng hoạt động.

Đây là requirement mà IT/DevOps cần tham gia xác định.

2.10. Development Team
Bao gồm:

Backend Developer.
Frontend/Mobile Developer.
Integration Developer.
Vai trò:

Phân tích technical feasibility.
Estimate.
Implement.
Technical design.
Integration.
BA cần phối hợp với Dev để đảm bảo requirement đủ rõ và có thể implement.

2.11. QA / Tester
Vai trò: Xác nhận hệ thống đáp ứng requirement.

QA đặc biệt cần tham gia sớm vào các requirement:

Trip state.
Matching.
Timeout.
Payment.
Notification.
Permission.
Exception handling.
Ví dụ requirement tốt:

"Nếu Driver A không phản hồi trong X giây, hệ thống phải chuyển request sang Driver tiếp theo."

Requirement này có thể chuyển thành test case.

2.12. Security / Compliance
Do hệ thống xử lý:

PII.
Location.
Transaction.
Authentication.
Authorization.
Stakeholder này cần xác định:

Password policy.
Access control.
Encryption.
Data retention.
Audit.
Privacy.
Payment security.
3. External Stakeholders
Payment Provider
Ví dụ về vai trò:

CAB
 ↓
Payment API
 ↓
Payment Provider
 ↓
Success / Failed

Cần thống nhất:

API.
Authentication.
Timeout.
Retry.
Webhook/callback.
Transaction status.
Refund.
Notification Provider
Có thể cung cấp:

Push.
SMS.
Email.
Điểm quan trọng trong requirement là ABC muốn có thể thay đổi provider trong tương lai, do đó không nên để business logic phụ thuộc trực tiếp vào một provider duy nhất.

4. Stakeholder Map
Có thể dùng Power–Interest Matrix:

                    HIGH INTEREST
                         │
       Keep Satisfied    │     Manage Closely
                         │
   Finance               │     Sponsor
   Security              │     Product Owner
   IT                    │     Operation
                         │     BA
                         │
─────────────────────────┼──────────────────── HIGH POWER
                         │
       Monitor           │     Keep Informed
                         │
   Notification Provider │     Customer
   Payment Provider      │     Driver
                         │     QA/Dev
                         │
                    LOW INTEREST

Tuy nhiên, với dự án CAB, Customer và Driver có thể có power thấp nhưng business impact rất cao, nên không nên bỏ qua chỉ vì họ không có quyền quyết định.

5. Ai là người BA nên làm việc trực tiếp?
Có thể ưu tiên như sau:

Nhóm 1 – Phải làm việc thường xuyên
Product Owner / Business Owner
Operation Manager
Operation Staff
Customer Service
Finance
IT/Technical Lead
Nhóm 2 – Cần khảo sát/phỏng vấn
Customer
Driver
Security/Compliance
QA
Nhóm 3 – Cần trao đổi khi có integration
Payment Provider
Notification Provider
Infrastructure/Cloud team
6. RACI sơ bộ
Có thể lập RACI cho các hoạt động chính:

Hoạt động	Sponsor	PO	BA	Operation	Finance	IT/Dev	QA
Xác định Business Goal	A	R	C	C	C	I	I
Thu thập Requirement	I	A	R	C	C	C	C
Xác định MVP	A	R	C	C	C	C	I
Fare Rules	A	R	R	C	C	C	C
Driver Matching Rules	I	A	R	R	I	C	C
Payment Rules	I	A	R	C	R	C	C
Security Requirements	I	A	R	I	C	R	C
Development	I	A	C	I	I	R	C
Testing	I	A	C	C	C	C	R
UAT	A	R	R	R	C	C	R
Go-live	A	R	C	C	C	R	R

Chú thích:

R – Responsible: Người trực tiếp thực hiện.
A – Accountable: Người chịu trách nhiệm cuối cùng/phê duyệt.
C – Consulted: Người được tham vấn.
I – Informed: Người cần được thông báo.
Điểm BA cần lưu ý
Trong dự án này, Product Owner/Business Owner nên là người Accountable về requirement, còn BA là Responsible trong việc elicitation, analysis, documentation và validation. BA không nên tự quyết định những vấn đề chưa được khách hàng chốt như fare, driver priority, cancellation policy, payment retry hoặc data retention.

- mermaid syntax code:
%%{init: {
  "quadrantChart": {
    "chartWidth": 900,
    "chartHeight": 650,
    "pointLabelFontSize": 10,
    "pointTextPadding": 3,
    "pointRadius": 4,
    "quadrantLabelFontSize": 14
  }
}}%%
quadrantChart
    title CAB System - Stakeholder Power / Interest Matrix
    x-axis Low Interest --> High Interest
    y-axis Low Power --> High Power

    quadrant-1 Manage Closely
    quadrant-2 Keep Satisfied
    quadrant-3 Monitor
    quadrant-4 Keep Informed

    Finance: [0.28, 0.76]
    Security: [0.42, 0.86]
    IT: [0.58, 0.72]

    Sponsor: [0.78, 0.90]
    Product Owner: [0.94, 0.82]
    Operation: [0.84, 0.68]
    BA: [0.68, 0.58]

    Notification Provider: [0.20, 0.18]
    Payment Provider: [0.42, 0.30]

    Customer: [0.78, 0.30]
    Driver: [0.92, 0.20]
    QA: [0.62, 0.12]
    Dev: [0.48, 0.22]


Bước 3: Xác định business goal
Dựa trên đề bài CAB System, có thể xác định Business Goals ở cấp độ nghiệp vụ như sau. Lưu ý phân biệt Business Goal với System Requirement: goal trả lời “Doanh nghiệp muốn đạt được điều gì?”, còn requirement trả lời “Hệ thống phải làm gì để đạt được điều đó?”.

1. Business Goal tổng thể
Xây dựng một nền tảng CAB System có khả năng số hóa và tự động hóa toàn bộ quy trình đặt xe, từ tiếp nhận yêu cầu, tìm và phân công tài xế, thực hiện chuyến, tính cước, thanh toán đến đánh giá; đồng thời hỗ trợ vận hành hiệu quả, mở rộng quy mô và phát triển thêm dịch vụ trong tương lai.

Có thể chia thành 7 Business Goals chính:

BG01 – Số hóa và tự động hóa quy trình đặt xe
Mục tiêu
Giảm sự phụ thuộc vào tổng đài và các thao tác thủ công trong quy trình đặt xe.

Hiện trạng
Khách hàng có thể phải gọi tổng đài.
Phân công tài xế chủ yếu thủ công.
Thông tin chuyến đi phân tán.
Khó mở rộng khi số lượng chuyến tăng.
Kỳ vọng
Đặt xe
  ↓
Tự động tiếp nhận
  ↓
Tự động tìm tài xế
  ↓
Tự động cập nhật trạng thái
  ↓
Tính cước
  ↓
Thanh toán

Business Value
Giảm workload cho vận hành.
Giảm thời gian xử lý booking.
Giảm sai sót thủ công.
Tăng khả năng phục vụ khách hàng.
BG02 – Nâng cao trải nghiệm khách hàng
Mục tiêu
Cho phép khách hàng chủ động đặt và theo dõi chuyến đi thay vì phải phụ thuộc vào tổng đài.

Kỳ vọng
Khách hàng có thể:

Đặt xe nhanh chóng.
Biết hệ thống đang tìm tài xế.
Biết tài xế nào nhận chuyến.
Biết ETA.
Theo dõi trạng thái chuyến.
Biết số tiền phải trả.
Thanh toán thuận tiện.
Đánh giá tài xế.
Business Value
Tăng customer satisfaction.
Tăng khả năng khách hàng quay lại.
Giảm số lượng yêu cầu hỗ trợ từ Call Center.
BG03 – Tối ưu hóa việc phân công và sử dụng tài xế
Mục tiêu
Tự động hóa quá trình tìm tài xế phù hợp và tối ưu khả năng sử dụng đội ngũ tài xế.

Kỳ vọng
Hệ thống có thể:

Booking
   ↓
Xác định Driver phù hợp
   ↓
Ưu tiên Driver
   ↓
Gửi Request
   ↓
Accept?
 ┌─┴─┐
Yes  No/Timeout
 ↓      ↓
Assign  Driver tiếp theo

Business Value
Giảm thời gian tìm tài xế.
Tăng tỷ lệ booking được nhận.
Giảm thời gian tài xế chạy rỗng.
Tăng hiệu suất đội xe.
BG04 – Quản lý tập trung doanh thu và thanh toán
Mục tiêu
Xây dựng cơ chế quản lý tập trung cho fare, payment và transaction.

Kỳ vọng
Trip Completed
      ↓
Calculate Fare
      ↓
Payment
      ↓
Transaction
      ↓
Reporting / Reconciliation

Hỗ trợ:

Tiền mặt.
Thanh toán điện tử.
Theo dõi giao dịch.
Xử lý payment failure.
Tra cứu lịch sử.
Business Value
Kiểm soát doanh thu tốt hơn.
Giảm sai sót đối soát.
Tăng tính minh bạch giao dịch.
Tạo nền tảng để thêm phương thức thanh toán.
BG05 – Nâng cao hiệu quả vận hành và quản trị
Mục tiêu
Cung cấp cho bộ phận vận hành khả năng giám sát, xử lý và phân tích hoạt động kinh doanh trên một hệ thống tập trung.

Kỳ vọng
Operation có thể:

Theo dõi trip đang diễn ra.
Kiểm tra trạng thái driver.
Quản lý customer.
Quản lý driver.
Quản lý vehicle.
Xử lý trip lỗi.
Tra cứu transaction.
Ban lãnh đạo có thể theo dõi:

Số chuyến.
Doanh thu.
Completion rate.
Cancellation rate.
Driver performance.
Business Value
Data-driven operation & decision making

BG06 – Đảm bảo hệ thống có khả năng mở rộng và hoạt động ổn định
Mục tiêu
Xây dựng nền tảng có thể đáp ứng lượng khách hàng/chuyến xe tăng mà không phải xây dựng lại toàn bộ hệ thống.

Kỳ vọng
Các thành phần có thể mở rộng độc lập:

Booking
   │
   ├── Matching
   ├── Location
   ├── Notification
   └── Payment

Nếu một thành phần gặp sự cố:

Payment Down
     ↓
Booking vẫn hoạt động
     ↓
Payment = Pending/Failed
     ↓
Retry sau

Business Value
Giảm downtime.
Đảm bảo business continuity.
Hỗ trợ peak demand.
Giảm rủi ro khi mở rộng.
BG07 – Tạo nền tảng linh hoạt cho phát triển tương lai
Mục tiêu
CAB không chỉ phục vụ MVP hiện tại mà phải trở thành nền tảng có thể mở rộng.

Trong tương lai có thể:

Thêm loại dịch vụ.
Thêm loại xe.
Thêm payment method.
Thêm payment provider.
Thêm notification provider.
Thay đổi thành phần kỹ thuật.
Bổ sung chức năng mới.
Business Value
Giảm:

Cost của các lần nâng cấp.
Thời gian phát triển tính năng mới.
Rủi ro ảnh hưởng chức năng hiện tại.
2. Business Goal → Business Objective → KPI
Để Business Goal có thể đo lường được, BA nên chuyển tiếp thành Business Objective/KPI.

Business Goal	Business Objective	KPI đề xuất
BG01 – Tự động hóa	Giảm xử lý thủ công	% booking được xử lý tự động
BG02 – Customer Experience	Tăng trải nghiệm	Booking success rate, CSAT
BG03 – Driver Optimization	Tìm driver nhanh hơn	Average driver matching time
BG03 – Driver Optimization	Tăng khả năng nhận chuyến	Driver acceptance rate
BG04 – Payment	Quản lý giao dịch tập trung	Payment success rate
BG05 – Operation	Nâng hiệu quả vận hành	Trips/operator, resolution time
BG05 – Reporting	Tăng khả năng quản trị	Report availability / accuracy
BG06 – Scalability	Đảm bảo hoạt động peak	Availability, response time
BG06 – Resilience	Giảm ảnh hưởng sự cố	Failed dependency isolation rate
BG07 – Extensibility	Dễ thêm tính năng/provider	Time-to-integrate new provider

Các giá trị KPI cụ thể chưa nên tự đặt, vì đề bài chưa cung cấp baseline và target. BA cần xác nhận với Sponsor/PO.

3. Business Goal Hierarchy
Có thể trình bày trong tài liệu BA như sau:

Business VisionXây dựng nền tảng CAB phát triển lâu dài
BG01Số hóa & tự động hóa đặt xe
BG02Nâng cao trải nghiệm khách hàng
BG03Tối ưu phân công tài xế
BG04Quản lý thanh toán & doanh thu
BG05Nâng cao hiệu quả vận hành
BG06Ổn định & mở rộng quy mô
BG07Linh hoạt phát triển tương lai
Giảm thao tác thủ công
Tự động hóa matching
Theo dõi chuyến
Thanh toán thuận tiện
Đánh giá dịch vụ
Giảm thời gian tìm tài xế
Tăng tỷ lệ nhận chuyến
Quản lý transaction
Đối soát doanh thu
Giám sát trip
Báo cáo KPI
Scale độc lập
Không ảnh hưởng toàn hệ thống khi một component lỗi
Thêm dịch vụ
Thêm payment/notification provider
4. Ưu tiên Business Goal trong dự án 7 tuần
Vì thời gian chỉ 7 tuần, tôi đề xuất BA và PO ưu tiên:

P0 – Bắt buộc đạt
BG01 – Số hóa đặt xe
BG03 – Tự động tìm/phân công tài xế
BG02 – Theo dõi chuyến
BG04 – Thanh toán cơ bản
BG05 – Vận hành cơ bản

P1 – Rất quan trọng
BG06 – Stability & Scalability
BG07 – Extensibility

Hai mục này không nhất thiết phải xây dựng hàng loạt chức năng ngay trong MVP, nhưng kiến trúc và requirement ngay từ đầu phải hỗ trợ.

5. Business Goal Statement dùng trong tài liệu BA
Nếu cần viết ngắn gọn trong BRD/Project Charter, có thể dùng:

Business Goal: Xây dựng nền tảng CAB System nhằm số hóa và tự động hóa quy trình đặt xe, tối ưu việc tìm và phân công tài xế, nâng cao trải nghiệm khách hàng, quản lý tập trung thanh toán và hoạt động vận hành, đồng thời đảm bảo hệ thống có khả năng mở rộng, hoạt động ổn định và linh hoạt cho việc bổ sung dịch vụ trong tương lai.

Sau đó từ Business Goal này BA có thể trace xuống:

Business Goal → Business Objective → Stakeholder Need → Business Requirement → Functional Requirement → Use Case/User Story → Acceptance Criteria → Test Case.

Đây là chuỗi traceability rất phù hợp để xây dựng BRD/SRS và RTM cho case CAB System.


Bước 4: Xác định phạm vi yêu cầu (scope)
Dựa trên đề bài CAB System và mốc triển khai 7 tuần, phạm vi nên được xác định theo hướng MVP có thể triển khai được, đồng thời giữ kiến trúc đủ linh hoạt cho Phase 2.

1. Scope tổng thể
Scope Statement
CAB System Phase 1 là nền tảng đặt xe hỗ trợ khách hàng, tài xế và nhân viên vận hành trong toàn bộ quy trình từ tạo yêu cầu đặt xe → tìm/phân công tài xế → thực hiện chuyến → tính cước → thanh toán → thông báo → đánh giá, đồng thời cung cấp chức năng quản trị, báo cáo cơ bản, phân quyền và audit log.

Boundary của hệ thống
flowchart LR
    Customer["Customer"]
    Driver["Driver"]
    Operation["Operation Staff"]
    Admin["Admin"]
    Payment["Payment Provider"]
    Notification["Notification Provider"]

    subgraph CAB["CAB System - Phase 1"]
        Account["Account & Profile"]
        Booking["Booking"]
        Matching["Driver Matching"]
        Trip["Trip Management"]
        Location["Driver Location"]
        Fare["Fare Calculation"]
        Pay["Payment Management"]
        Notify["Notification"]
        Rating["Rating"]
        AdminPortal["Operation & Admin"]
        Report["Reporting"]
        Audit["Audit Log"]
    end

    Customer --> Account
    Customer --> Booking
    Customer --> Trip
    Customer --> Pay
    Customer --> Rating

    Driver --> Account
    Driver --> Matching
    Driver --> Trip
    Driver --> Location

    Operation --> AdminPortal
    Admin --> AdminPortal

    Booking --> Matching
    Matching --> Trip
    Trip --> Fare
    Fare --> Pay
    Pay --> Payment
    Trip --> Notify
    Notify --> Notification

    AdminPortal --> Report
    AdminPortal --> Audit

2. In Scope – Những gì thuộc phạm vi Phase 1
2.1. Quản lý tài khoản
Customer
Đăng ký.
Đăng nhập/đăng xuất.
Cập nhật thông tin cá nhân.
Quản lý thông tin tài khoản.
Driver
Tạo tài khoản/được Operation tạo.
Đăng nhập.
Cập nhật hồ sơ.
Quản lý thông tin phương tiện.
Cập nhật trạng thái hoạt động.
Staff/Admin
Đăng nhập.
Quản lý tài khoản theo quyền.
3. Booking Management
Đây là core scope.

Customer có thể:

Nhập điểm đón.
Nhập điểm đến.
Chọn loại xe/dịch vụ.
Tạo yêu cầu đặt xe.
Nhận thông báo hệ thống đã tiếp nhận.
Hệ thống phải:

Tạo booking/trip.
Sinh mã chuyến.
Lưu thông tin chuyến.
Chuyển sang quy trình tìm tài xế.
4. Driver Matching
In Scope và là chức năng trọng tâm.

Hệ thống:

Tìm driver phù hợp.
Kiểm tra driver available.
Xét vị trí.
Xét loại xe.
Áp dụng tiêu chí ưu tiên đã được doanh nghiệp xác định.
Gửi request cho driver.
Xử lý Accept.
Xử lý Reject.
Xử lý Timeout.
Chuyển sang driver tiếp theo.
Thông báo khi không tìm được driver.
Scope boundary
Có:

Matching theo business rules được xác định cho MVP.

Chưa cam kết:

Thuật toán AI/ML tối ưu matching.

5. Trip Management
Quản lý vòng đời chuyến:

Requested
   ↓
Searching Driver
   ↓
Driver Assigned
   ↓
Driver Arriving
   ↓
Driver Arrived
   ↓
Passenger Picked Up
   ↓
In Progress
   ↓
Completed

Ngoài ra cần xử lý:

Cancelled.
No Driver Found.
Failed.
Customer có thể theo dõi trạng thái.

Driver có thể cập nhật trạng thái chuyến.

Operation có thể xem và hỗ trợ xử lý trip.

6. Driver Location
In Scope
Driver gửi vị trí.
Hệ thống ghi nhận vị trí hiện tại.
Sử dụng vị trí để matching.
Hỗ trợ tính ETA ở mức MVP.
Operation có thể xem vị trí/trạng thái driver theo quyền.
Chưa chốt
Tần suất GPS chính xác bao nhiêu giây.
Lưu lịch sử GPS bao lâu.
Có lưu toàn bộ route hay chỉ latest location.
Đây là requirement cần clarification.

7. Fare Calculation
In Scope
Tính giá sau khi hoàn thành chuyến.
Xác định tổng tiền phải trả.
Lưu fare của chuyến.
Hiển thị số tiền cho customer.
Hỗ trợ cấu hình fare rules cơ bản.
Chưa xác định
Công thức giá/km.
Giá theo thời gian.
Giá tối thiểu.
Surge pricing.
Phụ phí.
Voucher/discount.
Thuế.
Do đó, Fare Engine thuộc scope nhưng Fare Rules cụ thể cần khách hàng xác nhận.

8. Payment
In Scope
Cash:

Ghi nhận thanh toán tiền mặt.
Cập nhật payment status.
Electronic Payment:

Tích hợp payment provider.
Gửi yêu cầu thanh toán.
Nhận kết quả.
Lưu transaction/reference.
Xử lý success/failure.
Cho phép retry theo policy.
Out of Scope
CAB không lưu trực tiếp:

Số thẻ đầy đủ.
CVV.
Sensitive payment credentials.
9. Notification
In Scope
Customer nhận notification khi:

Booking được tiếp nhận.
Driver nhận chuyến.
Driver đến điểm đón.
Trip hoàn thành.
Payment có kết quả.
Driver nhận:

Trip mới.
Thay đổi liên quan đến trip.
Kiến trúc
Notification nên được xây dựng theo abstraction:

CAB
 ↓
Notification Service
 ├── Push
 ├── SMS
 └── Email

MVP
Có thể triển khai một hoặc một số kênh chính, nhưng kiến trúc phải cho phép thêm provider trong tương lai.

10. Rating
In Scope
Sau khi trip completed:

Customer xem rating form.
Đánh giá driver.
Lưu rating.
Operation có thể tra cứu rating.
Chưa xác định
Rating 1–5 hay hệ khác.
Có comment hay không.
Có cho sửa rating không.
Driver có được đánh giá customer không.
11. Operation/Admin Portal
Customer Management
Search.
View profile.
View trip history.
Quản lý trạng thái tài khoản.
Driver Management
Create/update driver.
Active/inactive.
View driver status.
View trip history.
Vehicle Management
Create.
Update.
Active/inactive.
Gắn driver.
Trip Management
Xem trip đang chạy.
Search trip.
Xem trạng thái.
Hỗ trợ trip lỗi.
Transaction
Search transaction.
View payment status.
Tra cứu lịch sử.
12. RBAC & Audit
In Scope
Phân quyền tối thiểu:

Customer
Driver
Operation Staff
Administrator

Hệ thống phải kiểm soát:

Ai được xem dữ liệu.
Ai được thay đổi dữ liệu.
Ai được thực hiện thao tác nhạy cảm.
Audit Log
Ghi nhận các hành động quan trọng:

Login.
Thay đổi quyền.
Thay đổi driver.
Thay đổi trip bởi staff.
Thay đổi payment.
Các thao tác admin nhạy cảm.
13. Reporting
MVP
Báo cáo cơ bản:

Tổng số chuyến.
Completed trips.
Cancelled trips.
Completion rate.
Cancellation rate.
Revenue.
Driver performance cơ bản.
Phase 2
Có thể mở rộng:

Real-time dashboard nâng cao.
Forecasting.
Driver utilization analytics.
Revenue prediction.
Business intelligence nâng cao.
14. Non-functional Scope
Đây không phải chức năng riêng, nhưng phải nằm trong scope của Phase 1.

Performance
Cần xác định:

API response time.
Booking response time.
Matching time.
Concurrent users.
Scalability
Các thành phần có thể scale độc lập.
Hỗ trợ peak demand.
Availability
Một component lỗi không làm toàn bộ hệ thống dừng.
Payment/Notification failure phải được cô lập.
Security
Authentication.
Authorization.
Data protection.
Audit.
Maintainability
Có khả năng thêm service/provider.
Có khả năng triển khai từng phần.
15. Out of Scope – Phase 1
Đây là phần rất quan trọng khi dự án chỉ có 7 tuần.

Có thể tạm đưa ra ngoài MVP:

Chức năng	Phase 1
Scheduled Booking	❌
Multi-stop trip	❌
Corporate Account	❌
Loyalty Program	❌
Voucher/Promotion nâng cao	❌
Dynamic Pricing nâng cao	❌
AI Driver Matching	❌
Advanced BI/Analytics	❌
Multi-country operation	❌
Multi-language nâng cao	❌
Fleet management nâng cao	❌
Driver incentive system	❌
Advanced fraud detection	❌

Lưu ý: Đây là đề xuất scope control, cần PO/Sponsor phê duyệt; không phải các chức năng khách hàng đã chính thức loại bỏ.

16. Scope theo Actor
Scope	Customer	Driver	Operation	Admin
Account	✓	✓	✓	✓
Profile	✓	✓	✓	✓
Booking	✓		✓	✓
Driver Matching		✓	✓	✓
Trip Tracking	✓	✓	✓	✓
Driver Location		✓	✓	✓
Fare	✓	✓	✓	✓
Payment	✓		✓	✓
Notification	✓	✓	✓	✓
Rating	✓		✓	✓
Customer Management			✓	✓
Driver Management			✓	✓
Vehicle Management		✓	✓	✓
Transaction			✓	✓
Reporting			✓	✓
RBAC			hạn chế	✓
Audit Log			hạn chế	✓

17. Scope Boundary
Có thể viết trong BRD như sau:

Inside the System
Account Management, Booking Management, Driver Matching, Trip Management, Driver Location, Fare Calculation, Payment Management, Notification Management, Rating, Operation Portal, Reporting, RBAC và Audit Log.

Outside the System / External Dependencies
Payment Provider, Notification Provider và các dịch vụ bản đồ/GPS bên ngoài nếu được sử dụng.

flowchart LR
    subgraph External["External"]
        Customer["Customer"]
        Driver["Driver"]
        Payment["Payment Provider"]
        Map["Map / Location Provider"]
        Notify["Notification Provider"]
    end

    subgraph CAB["CAB System"]
        Booking["Booking"]
        Matching["Driver Matching"]
        Trip["Trip"]
        Fare["Fare"]
        PaymentSvc["Payment"]
        Notification["Notification"]
        Operation["Operation Portal"]
    end

    Customer --> Booking
    Customer --> Trip
    Driver --> Matching
    Driver --> Trip

    Booking --> Matching
    Matching --> Trip
    Trip --> Fare
    Fare --> PaymentSvc
    PaymentSvc --> Payment

    Matching --> Map
    Trip --> Notification
    Notification --> Notify

    Operation --> Booking
    Operation --> Trip

18. Scope Statement hoàn chỉnh cho BRD
Bạn có thể đưa nguyên đoạn sau vào tài liệu:

Scope của CAB System Phase 1 bao gồm việc xây dựng nền tảng hỗ trợ khách hàng, tài xế và nhân viên vận hành trong toàn bộ quy trình đặt xe, bao gồm quản lý tài khoản, tạo yêu cầu đặt xe, tìm và phân công tài xế, theo dõi và cập nhật trạng thái chuyến, quản lý vị trí tài xế, tính cước, thanh toán, thông báo, đánh giá tài xế, quản trị dữ liệu vận hành, báo cáo, phân quyền và audit log. Hệ thống sẽ tích hợp với các bên thứ ba như nhà cung cấp thanh toán và thông báo nhưng không lưu trực tiếp dữ liệu thanh toán nhạy cảm. Các chức năng mở rộng như đặt xe theo lịch, multi-stop, loyalty, corporate account, AI matching và phân tích nâng cao được xem xét cho các phase tiếp theo. Phạm vi chi tiết của các chức năng chưa được khách hàng chốt sẽ được xác nhận trong giai đoạn phân tích trước khi phát triển.

Một điểm BA cần đặc biệt lưu ý
Scope ≠ danh sách tính năng. Với case này, nên quản lý scope theo 3 lớp:

Business Scope
→ CAB phục vụ toàn bộ vòng đời đặt xe.

System Scope
→ Booking, Matching, Trip, Payment, Notification, Operation...

Release Scope
→ Trong 7 tuần chỉ triển khai MVP ưu tiên, còn các chức năng mở rộng đưa vào Phase 2.

Cách phân chia này sẽ giúp tránh scope creep khi khách hàng tiếp tục đưa thêm yêu cầu trong quá trình phát triển.



Bước 5: Thiết kế Business requirement
Được. Với CAB System, Business Requirement (BR) nên mô tả doanh nghiệp cần đạt được điều gì, chưa đi quá sâu vào cách hệ thống sẽ implement. Từ BR mới trace xuống Functional Requirement, Use Case/User Story.

Dưới đây là bộ Business Requirements sơ bộ có thể dùng cho BRD.

1. Cấu trúc mã Requirement

Đề xuất quy ước:

BR-01 → BR-xx: Business Requirement
FR-01 → FR-xx: Functional Requirement
NFR-01 → NFR-xx: Non-functional Requirement
BRL-01 → BRL-xx: Business Rule

Ví dụ:

BR-03: Doanh nghiệp cần tự động hóa việc tìm và phân công tài xế.

Sau đó:

BR-03 → FR-10 Find Driver → FR-11 Rank Driver → FR-12 Driver Accept/Reject → FR-13 Reassign Driver.

2. Danh sách Business Requirement tổng thể

Có thể chia thành 9 nhóm:

Nhóm	Mã	Business Requirement	Priority
Customer	BR-01	Hỗ trợ khách hàng quản lý tài khoản	Must
Booking	BR-02	Số hóa quy trình đặt xe	Must
Matching	BR-03	Tự động tìm và phân công tài xế	Must
Trip	BR-04	Quản lý toàn bộ vòng đời chuyến	Must
Location	BR-05	Quản lý vị trí tài xế	Must
Fare	BR-06	Tính và quản lý cước chuyến	Must
Payment	BR-07	Hỗ trợ quản lý thanh toán	Must
Notification	BR-08	Cung cấp thông báo kịp thời	Must
Rating	BR-09	Thu thập đánh giá dịch vụ	Should
Operation	BR-10	Hỗ trợ vận hành tập trung	Must
Reporting	BR-11	Cung cấp dữ liệu quản trị	Must
Security	BR-12	Bảo vệ dữ liệu và kiểm soát truy cập	Must
Audit	BR-13	Lưu vết hoạt động quan trọng	Must
Scalability	BR-14	Đảm bảo khả năng mở rộng	Must
Resilience	BR-15	Hạn chế ảnh hưởng khi thành phần lỗi	Must
Extensibility	BR-16	Hỗ trợ mở rộng dịch vụ trong tương lai	Should
3. BR-01 – Quản lý khách hàng
Business Requirement

BR-01: Hệ thống phải hỗ trợ khách hàng quản lý tài khoản và thông tin cá nhân để sử dụng các dịch vụ CAB một cách thuận tiện và an toàn.

Business Need

Khách hàng cần có tài khoản để:

Đặt xe.
Theo dõi chuyến.
Thanh toán.
Xem lịch sử.
Đánh giá.
Expected Business Value
Giảm thao tác thủ công.
Quản lý khách hàng tập trung.
Tạo cơ sở cho việc cung cấp dịch vụ cá nhân hóa.
Priority

Must Have

4. BR-02 – Số hóa quy trình đặt xe
Business Requirement

BR-02: Hệ thống phải cho phép khách hàng tạo yêu cầu đặt xe thông qua nền tảng CAB mà không cần phụ thuộc vào quy trình đặt xe thủ công qua tổng đài.

Business Need

Customer cần:

Điểm đón.
Điểm đến.
Loại xe/dịch vụ.
Gửi yêu cầu.
Business Value
Giảm tải Call Center.
Rút ngắn thời gian đặt xe.
Tăng số lượng booking có thể xử lý.
Priority

Must Have

5. BR-03 – Tự động tìm và phân công tài xế

Đây là Business Requirement quan trọng nhất của hệ thống.

Business Requirement

BR-03: Hệ thống phải tự động xác định và ưu tiên các tài xế phù hợp với yêu cầu đặt xe dựa trên các tiêu chí vận hành của doanh nghiệp, đồng thời có khả năng tiếp tục tìm tài xế khác khi tài xế được đề xuất không nhận chuyến.

Business Need

Quy trình hiện tại còn thủ công.

CAB cần:

Booking
   ↓
Find Candidate
   ↓
Prioritize
   ↓
Offer Trip
   ↓
Accept?
 ┌─┴────────┐
Yes         No/Timeout
 ↓              ↓
Assign       Next Driver

Business Value
Giảm thời gian matching.
Tăng tỷ lệ booking thành công.
Giảm workload Operation.
Tối ưu đội ngũ driver.
Priority

Must Have

6. BR-04 – Quản lý vòng đời chuyến
Business Requirement

BR-04: Hệ thống phải quản lý và cung cấp khả năng theo dõi toàn bộ vòng đời của chuyến đi từ khi khách hàng tạo yêu cầu đến khi chuyến hoàn thành hoặc kết thúc theo một trạng thái ngoại lệ.

Business Need

Các bên phải biết trip đang ở đâu trong quy trình.

Ví dụ:

Requested
    ↓
Searching
    ↓
Assigned
    ↓
Arriving
    ↓
Arrived
    ↓
Picked Up
    ↓
In Progress
    ↓
Completed

Business Value
Minh bạch trạng thái.
Giảm khiếu nại.
Operation dễ xử lý sự cố.
Priority

Must Have

7. BR-05 – Quản lý vị trí tài xế
Business Requirement

BR-05: Hệ thống phải sử dụng thông tin vị trí của tài xế để hỗ trợ việc tìm tài xế phù hợp, theo dõi chuyến và cải thiện khả năng ước tính thời gian tài xế đến.

Business Value
Matching chính xác hơn.
Cải thiện ETA.
Cải thiện trải nghiệm customer.
Hỗ trợ Operation.
Priority

Must Have

8. BR-06 – Tính cước
Business Requirement

BR-06: Hệ thống phải xác định số tiền khách hàng cần thanh toán dựa trên loại dịch vụ và các quy tắc tính cước được doanh nghiệp phê duyệt.

Business Value
Tính giá nhất quán.
Giảm sai sót.
Tự động hóa tính cước.
Hỗ trợ quản lý doanh thu.
Open Issue

Cần xác nhận:

Giá theo km?
Theo thời gian?
Giá tối thiểu?
Phụ phí?
Khuyến mãi?
Surge?
Priority

Must Have

9. BR-07 – Thanh toán
Business Requirement

BR-07: Hệ thống phải hỗ trợ khách hàng thanh toán cho chuyến đi bằng tiền mặt hoặc phương thức điện tử, đồng thời quản lý trạng thái giao dịch và xử lý các trường hợp thanh toán thất bại theo chính sách của doanh nghiệp.

Business Value
Quản lý tập trung.
Giảm sai sót giao dịch.
Tăng lựa chọn thanh toán.
Hỗ trợ đối soát doanh thu.
Constraint

CAB không được lưu trực tiếp thông tin thanh toán nhạy cảm của khách hàng.

Priority

Must Have

10. BR-08 – Notification
Business Requirement

BR-08: Hệ thống phải cung cấp thông báo kịp thời cho khách hàng và tài xế về các sự kiện quan trọng trong vòng đời chuyến đi.

Customer
Booking accepted.
Driver assigned.
Driver arrived.
Trip completed.
Payment result.
Driver
New trip.
Trip changes.
Cancellation.
Business Value
Tăng transparency.
Giảm customer support.
Cải thiện driver responsiveness.
Priority

Must Have

11. BR-09 – Đánh giá dịch vụ
Business Requirement

BR-09: Hệ thống phải cho phép khách hàng đánh giá tài xế sau khi hoàn thành chuyến để doanh nghiệp thu thập phản hồi và đánh giá chất lượng dịch vụ.

Business Value
Đo chất lượng dịch vụ.
Hỗ trợ quản lý driver.
Cải thiện customer experience.
Priority

Should Have

12. BR-10 – Quản lý vận hành tập trung
Business Requirement

BR-10: Hệ thống phải cung cấp giao diện vận hành tập trung cho phép nhân viên quản lý khách hàng, tài xế, phương tiện, chuyến đi và giao dịch.

Operation cần:
Theo dõi trip.
Xem driver status.
Tra cứu customer.
Quản lý driver.
Quản lý vehicle.
Xử lý trip exception.
Tra cứu transaction.
Business Value

Tạo một "single source of truth" cho bộ phận vận hành.

Priority

Must Have

13. BR-11 – Báo cáo quản trị
Business Requirement

BR-11: Hệ thống phải cung cấp dữ liệu và báo cáo hỗ trợ Ban giám đốc đánh giá tình hình hoạt động và hiệu quả kinh doanh của dịch vụ CAB.

Các KPI chính
Total trips.
Completed trips.
Cancelled trips.
Completion rate.
Cancellation rate.
Revenue.
Driver performance.
Business Value

Hỗ trợ:

Data-driven decision making

Priority

Must Have

14. BR-12 – Bảo mật và kiểm soát truy cập
Business Requirement

BR-12: Hệ thống phải đảm bảo chỉ người dùng được xác thực và có quyền phù hợp mới có thể truy cập dữ liệu hoặc thực hiện các chức năng tương ứng.

Business Need

Bảo vệ:

Customer information.
Driver information.
Vehicle information.
Location.
Transaction data.
Priority

Must Have

15. BR-13 – Audit & Traceability
Business Requirement

BR-13: Hệ thống phải lưu vết các thao tác quan trọng để doanh nghiệp có thể truy xuất và kiểm tra khi xảy ra sự cố hoặc cần kiểm toán.

Ví dụ:

Who
What
When
Before
After
Result

Priority

Must Have

16. BR-14 – Khả năng mở rộng
Business Requirement

BR-14: Hệ thống phải có khả năng mở rộng để đáp ứng sự gia tăng về số lượng khách hàng, tài xế và chuyến đi mà không ảnh hưởng đáng kể đến các chức năng đang hoạt động.

Business Value
Hỗ trợ business growth.
Xử lý peak demand.
Giảm chi phí thay đổi hệ thống.
Priority

Must Have

17. BR-15 – Business Continuity / Resilience
Business Requirement

BR-15: Sự cố tại một thành phần hoặc dịch vụ phụ trợ như thanh toán hoặc thông báo không được làm gián đoạn toàn bộ quy trình đặt xe và quản lý chuyến.

Ví dụ:

Payment Provider Down
          ↓
Trip vẫn được lưu
          ↓
Payment = Pending/Failed
          ↓
Retry sau


Notification lỗi:

Booking vẫn thành công
       ↓
Notification retry

Priority

Must Have

18. BR-16 – Khả năng mở rộng nghiệp vụ trong tương lai
Business Requirement

BR-16: Hệ thống phải hỗ trợ doanh nghiệp bổ sung loại dịch vụ, phương thức thanh toán, nhà cung cấp thông báo và các thành phần nghiệp vụ mới mà hạn chế ảnh hưởng đến các chức năng hiện tại.

Ví dụ Phase 2
CAB
 ├── Standard Taxi
 ├── Premium
 ├── Bike
 ├── Delivery
 └── Scheduled Ride


Payment:

Cash
Card
E-Wallet
Banking
...


Notification:

Push
SMS
Email
Future Channel

Priority

Should Have

19. Tổng hợp Business Requirement Matrix
ID	Business Requirement	Stakeholder	Priority	Business Value
BR-01	Quản lý tài khoản	Customer/Driver	Must	Quản lý user
BR-02	Số hóa đặt xe	Customer/Business	Must	Giảm thủ công
BR-03	Tự động matching driver	Customer/Driver/Operation	Must	Tối ưu vận hành
BR-04	Quản lý vòng đời trip	All	Must	Minh bạch
BR-05	Quản lý location	Driver/Customer/Operation	Must	Matching + ETA
BR-06	Tính cước	Customer/Finance	Must	Doanh thu
BR-07	Thanh toán	Customer/Finance	Must	Transaction
BR-08	Notification	Customer/Driver	Must	UX
BR-09	Rating	Customer/Business	Should	Quality
BR-10	Operation Portal	Operation	Must	Operational efficiency
BR-11	Reporting	Management	Must	Decision making
BR-12	Security/RBAC	All	Must	Data protection
BR-13	Audit	Management/IT	Must	Traceability
BR-14	Scalability	Business/IT	Must	Business growth
BR-15	Resilience	Business/IT	Must	Business continuity
BR-16	Extensibility	Business/IT	Should	Future growth
20. Traceability từ Business Goal xuống Requirement

Đây là phần rất nên có trong tài liệu BA.

Business GoalXây dựng nền tảng CAB
BG01Tự động hóa đặt xe
BG02Nâng cao trải nghiệm
BG03Tối ưu driver
BG04Quản lý doanh thu
BG05Nâng cao vận hành
BG06Ổn định & mở rộng
BR-02Số hóa đặt xe
BR-03Tự động matching
BR-04Quản lý trip
BR-08Notification
BR-09Rating
BR-05Driver location
BR-06Fare
BR-07Payment
BR-11Reporting
BR-10Operation
BR-12Security
BR-13Audit
BR-14Scalability
BR-15Resilience
BR-16Extensibility
21. Mẫu Business Requirement chuẩn

Khi viết BRD chính thức, mỗi requirement nên có template:

Field	Nội dung
ID	BR-03
Name	Driver Matching
Description	Doanh nghiệp cần tự động tìm và phân công driver
Business Need	Giảm thao tác thủ công, rút ngắn thời gian tìm xe
Stakeholder	Customer, Driver, Operation
Priority	Must Have
Business Value	Tăng booking success, giảm workload
Business Rule	Driver phải phù hợp và available
Assumption	Location driver khả dụng
Dependency	Location service
Acceptance Criteria	Booking phải tiếp tục tìm driver nếu driver trước reject/timeout
Open Issue	Timeout và ranking chưa được xác định
Traceability	BG03
Điểm quan trọng

Ở tầng Business Requirement, không nên viết:

"System shall use Redis GeoQuery to find drivers within 5 km."

Đó là technical/functional solution.

Nên viết:

"Doanh nghiệp cần hệ thống xác định và ưu tiên các tài xế phù hợp, gần điểm đón và đang sẵn sàng nhận chuyến."

Sau khi business rule được xác nhận, BA mới chuyển xuống Functional Requirement và đội kỹ thuật quyết định giải pháp implement.

Với CAB System, BR-03 (Driver Matching), BR-04 (Trip Lifecycle), BR-06 (Fare), BR-07 (Payment) và BR-15 (Resilience) là các business requirements nên được ưu tiên workshop với khách hàng trước khi bắt đầu thiết kế chi tiết.



Bước 6: xây dựng business process
Dưới đây là Business Process sơ bộ cho CAB System, tập trung vào quy trình nghiệp vụ end-to-end. Với vai trò BA, nên xây dựng Level 0 → Level 1 → Level 2 để từ quy trình tổng thể có thể đi xuống Use Case/Functional Requirement.

1. Business Process tổng thể – Level 0

Quy trình kinh doanh cốt lõi của CAB có thể xác định là:

Đặt xe → Tìm tài xế → Phân công → Thực hiện chuyến → Tính cước → Thanh toán → Đánh giá

1. Tạo yêu cầu đặt xe
2. Tìm tài xế
3. Phân công tài xế
4. Thực hiện chuyến
5. Tính cước
6. Thanh toán
7. Đánh giá

Đây là Core Business Process của hệ thống.

2. Business Process – Level 1

Có thể chia thành 8 process chính:

flowchart TD
    Start(("Customer có nhu cầu đi xe"))

    P1["P1. Quản lý tài khoản"]
    P2["P2. Đặt xe"]
    P3["P3. Tìm & phân công tài xế"]
    P4["P4. Thực hiện & theo dõi chuyến"]
    P5["P5. Tính cước"]
    P6["P6. Thanh toán"]
    P7["P7. Đánh giá"]
    P8["P8. Quản lý & giám sát vận hành"]

    End(("Trip hoàn tất"))

    Start --> P1
    P1 --> P2
    P2 --> P3
    P3 --> P4
    P4 --> P5
    P5 --> P6
    P6 --> P7
    P7 --> End

    P8 -. "Giám sát / hỗ trợ" .-> P2
    P8 -. "Giám sát / hỗ trợ" .-> P3
    P8 -. "Giám sát / xử lý lỗi" .-> P4
    P8 -. "Tra cứu" .-> P6

3. Process P1 – Quản lý tài khoản
Input
Thông tin khách hàng/tài xế.
Username/phone/email.
Thông tin cá nhân.
Process
Đăng ký
   ↓
Validate thông tin
   ↓
Tạo tài khoản
   ↓
Đăng nhập
   ↓
Quản lý Profile

Output
User account.
User profile.
Authentication status.
Actor
Customer.
Driver.
Operation/Admin.
4. Process P2 – Đặt xe

Đây là quy trình bắt đầu một trip.

flowchart TD
    A["Customer đăng nhập"]
    B["Nhập điểm đón"]
    C["Nhập điểm đến"]
    D["Chọn loại xe / dịch vụ"]
    E["Gửi yêu cầu đặt xe"]
    F{"Thông tin hợp lệ?"}
    G["Tạo Booking"]
    H["Thông báo tiếp nhận"]
    I["Yêu cầu nhập lại"]

    A --> B --> C --> D --> E --> F
    F -- "Không" --> I
    I --> B
    F -- "Có" --> G --> H

Business outcome

Booking được tạo thành công và chuyển sang:

SEARCHING_DRIVER

5. Process P3 – Tìm và phân công tài xế

Đây là quy trình nghiệp vụ phức tạp nhất.

flowchart TD
    A["Booking được tạo"]
    B["Xác định Driver phù hợp"]
    C{"Có Driver phù hợp?"}

    D["Xếp hạng Driver"]
    E["Gửi yêu cầu cho Driver"]
    F{"Driver phản hồi?"}

    G["Driver Accept"]
    H["Driver Reject"]
    I["Timeout"]

    J["Assign Driver"]
    K["Tìm Driver tiếp theo"]
    L["Thông báo không tìm được Driver"]

    A --> B --> C

    C -- "Không" --> L
    C -- "Có" --> D --> E --> F

    F -- "Accept" --> G --> J
    F -- "Reject" --> H --> K
    F -- "Timeout" --> I --> K

    K --> C

Business Rule quan trọng
BR-M01

Chỉ driver đáp ứng tiêu chí nghiệp vụ mới được đưa vào danh sách candidate.

BR-M02

Driver phải ở trạng thái sẵn sàng nhận chuyến.

BR-M03

Nếu driver reject hoặc timeout, hệ thống phải tiếp tục tìm driver khác.

BR-M04

Một booking chỉ được assign thành công cho một driver.

BR-M05

Nếu không còn driver phù hợp, booking phải chuyển sang trạng thái:

NO_DRIVER_FOUND

và khách hàng phải được thông báo.

6. Process P4 – Thực hiện chuyến

Sau khi driver accept:

Driver Accept
Driver Assigned
Driver di chuyển đến điểm đón
Driver Arrived
Đón khách
Passenger Picked Up
Di chuyển đến điểm đến
Trip Completed
Trạng thái đề xuất
DRIVER_ASSIGNED
       ↓
DRIVER_ARRIVING
       ↓
DRIVER_ARRIVED
       ↓
PASSENGER_PICKED_UP
       ↓
IN_PROGRESS
       ↓
COMPLETED


Customer trong quá trình này có thể:

Xem driver.
Xem vehicle.
Theo dõi trạng thái.
Xem ETA.

Driver có trách nhiệm:

Cập nhật trạng thái.
Gửi location.
Hoàn thành trip.
7. Process P5 – Tính cước

Khi trip hoàn thành:

Trip Completed
Thu thập thông tin chuyến
Xác định loại dịch vụ
Áp dụng Fare Rules
Tính tổng tiền
Lưu Fare
Thông báo số tiền
Input
Service type.
Trip information.
Distance.
Duration.
Fare rules.
Các phụ phí nếu có.
Output

Final Fare

8. Process P6 – Thanh toán
flowchart TD
    A["Fare đã được xác định"]
    B{"Phương thức thanh toán?"}

    C["Cash"]
    D["Electronic Payment"]

    E["Ghi nhận Cash Payment"]
    F["Gửi yêu cầu Payment Provider"]
    G{"Payment thành công?"}

    H["Payment SUCCESS"]
    I["Payment FAILED"]
    J["Retry / Đổi phương thức"]
    K["Payment PENDING / FAILED"]

    A --> B

    B -- "Cash" --> C --> E --> H

    B -- "Electronic" --> D --> F --> G

    G -- "Có" --> H
    G -- "Không" --> I --> J --> F
    J --> K

Business Rule

Payment failure không được làm mất thông tin trip.

Ví dụ:

Trip = COMPLETED
Payment = FAILED


Hai trạng thái này phải được quản lý độc lập.

9. Process P7 – Đánh giá
flowchart TD
    A["Trip Completed"]
    B["Payment Processed"]
    C["Customer được yêu cầu đánh giá"]
    D{"Customer đánh giá?"}
    E["Lưu Rating"]
    F["Kết thúc"]

    A --> B --> C --> D
    D -- "Có" --> E --> F
    D -- "Không" --> F


Có thể mở rộng trong tương lai thành:

Customer Rating
      +
Driver Rating
      ↓
Service Quality
      ↓
Driver Performance

10. Process P8 – Vận hành và xử lý ngoại lệ

Đây là một supporting business process, chạy song song với core process.

Operation Dashboard
Theo dõi Trip
Theo dõi Driver
Quản lý Customer
Quản lý Vehicle
Tra cứu Transaction
Xử lý Trip Exception

Operation có thể can thiệp khi:

Không tìm được driver.
Driver mất kết nối.
Trip bị stuck.
Payment lỗi.
Customer khiếu nại.
Driver gặp sự cố.
11. End-to-End Business Process với Swimlane

Đây là sơ đồ nên đưa vào BRD, vì thể hiện rõ trách nhiệm của từng bên.

flowchart LR
    subgraph C["Customer"]
        C1["Tạo yêu cầu"]
        C2["Theo dõi Trip"]
        C3["Thanh toán"]
        C4["Đánh giá"]
    end

    subgraph S["CAB System"]
        S1["Tiếp nhận Booking"]
        S2["Tìm Driver"]
        S3["Gửi Request"]
        S4["Quản lý Trip"]
        S5["Tính Fare"]
        S6["Quản lý Payment"]
        S7["Gửi Notification"]
    end

    subgraph D["Driver"]
        D1["Nhận Request"]
        D2["Accept / Reject"]
        D3["Cập nhật Location"]
        D4["Cập nhật Trip Status"]
    end

    subgraph O["Operation"]
        O1["Giám sát"]
        O2["Xử lý Exception"]
    end

    subgraph P["Payment Provider"]
        P1["Process Payment"]
        P2["Return Result"]
    end

    C1 --> S1
    S1 --> S2
    S2 --> S3
    S3 --> D1
    D1 --> D2

    D2 -->|Accept| S4
    D2 -->|Reject/Timeout| S2

    D3 --> S4
    D4 --> S4

    S4 --> C2
    S4 --> S5
    S5 --> S6

    S6 --> P1
    P1 --> P2
    P2 --> S6

    S6 --> C3
    S4 --> S7
    S7 --> C2

    C3 --> C4

    O1 -.-> S4
    O2 -.-> S4
    O2 -.-> S6

12. Business Process có Exception

Một BA tốt không chỉ vẽ Happy Path, mà cần xác định các nhánh ngoại lệ.

Exception E01 – Không tìm được tài xế
Booking
 ↓
Search Driver
 ↓
Không có Driver
 ↓
NO_DRIVER_FOUND
 ↓
Notify Customer
 ↓
Customer quyết định:
  ├── Retry
  └── Cancel

Exception E02 – Driver Reject
Driver A
   ↓
Reject
   ↓
Search Driver
   ↓
Driver B


Customer không cần tạo lại booking.

Exception E03 – Driver Timeout
Send Request
     ↓
Wait Response
     ↓
Timeout
     ↓
Release Driver
     ↓
Next Candidate


Thời gian timeout là Open Issue cần xác nhận.

Exception E04 – Driver mất kết nối
Assigned
   ↓
Driver Offline
   ↓
Operation / System xử lý
   ↓
Reassign / Wait / Cancel


Chính sách cụ thể cần khách hàng xác nhận.

Exception E05 – Payment Failed
Trip Completed
      ↓
Payment Failed
      ↓
Notify Customer
      ↓
Retry / Alternative Method


Trip không được chuyển ngược về trạng thái chưa hoàn thành chỉ vì payment failure.

13. Business Process State của Trip

Có thể biểu diễn thành State Diagram:

stateDiagram-v2
    [*] --> REQUESTED

    REQUESTED --> SEARCHING_DRIVER
    SEARCHING_DRIVER --> DRIVER_ASSIGNED
    SEARCHING_DRIVER --> NO_DRIVER_FOUND

    DRIVER_ASSIGNED --> DRIVER_ARRIVING
    DRIVER_ARRIVING --> DRIVER_ARRIVED
    DRIVER_ARRIVED --> PASSENGER_PICKED_UP
    PASSENGER_PICKED_UP --> IN_PROGRESS
    IN_PROGRESS --> COMPLETED

    REQUESTED --> CANCELLED
    SEARCHING_DRIVER --> CANCELLED
    DRIVER_ASSIGNED --> CANCELLED
    DRIVER_ARRIVING --> CANCELLED

    COMPLETED --> PAYMENT_PENDING
    PAYMENT_PENDING --> PAID
    PAYMENT_PENDING --> PAYMENT_FAILED
    PAYMENT_FAILED --> PAYMENT_PENDING

    NO_DRIVER_FOUND --> [*]
    CANCELLED --> [*]
    PAID --> [*]


Đây là sơ đồ rất quan trọng vì nó giúp BA, Dev và QA thống nhất:

Trip có những trạng thái nào?
Trạng thái nào được chuyển sang trạng thái nào?
Ai được phép chuyển?
Điều kiện chuyển là gì?
Exception xử lý thế nào?
14. Business Process Decomposition

Có thể đưa vào tài liệu BA dưới dạng:

CAB Business Process
│
├── 1. Customer Management
│   ├── Register
│   ├── Login
│   └── Manage Profile
│
├── 2. Booking Management
│   ├── Create Booking
│   ├── Cancel Booking
│   └── Track Booking
│
├── 3. Driver Management
│   ├── Manage Profile
│   ├── Manage Vehicle
│   └── Manage Availability
│
├── 4. Driver Matching
│   ├── Find Candidates
│   ├── Prioritize
│   ├── Offer Trip
│   ├── Accept
│   ├── Reject
│   └── Reassign
│
├── 5. Trip Management
│   ├── Arriving
│   ├── Arrived
│   ├── Pick Up
│   ├── In Progress
│   └── Complete
│
├── 6. Fare & Payment
│   ├── Calculate Fare
│   ├── Cash
│   ├── Electronic Payment
│   └── Payment Retry
│
├── 7. Notification
│   ├── Customer Notification
│   └── Driver Notification
│
├── 8. Rating
│
└── 9. Operation & Reporting
    ├── Monitor
    ├── Exception Handling
    ├── Transaction
    └── Reports

15. Business Process cần chốt với khách hàng

Trong workshop BA, không nên coi các sơ đồ trên là final. Có ít nhất các điểm cần xác nhận:

Process	Cần xác nhận
Booking	Có cho đặt xe ngay hay đặt trước?
Booking	Có được sửa điểm đón/điểm đến không?
Matching	Tiêu chí chọn driver?
Matching	Driver timeout bao lâu?
Matching	Retry bao nhiêu driver?
Matching	Khi nào dừng tìm?
Trip	Ai được cancel?
Trip	Cancel ở trạng thái nào?
Trip	Có phí hủy không?
Location	Tần suất cập nhật GPS?
Fare	Công thức tính tiền?
Payment	Retry bao nhiêu lần?
Payment	Refund thế nào?
Notification	Kênh notification nào?
Exception	Driver mất mạng xử lý ra sao?
Data	Location lưu bao lâu?
16. Process quan trọng nhất để đưa vào BRD

Nếu tài liệu BA cần ưu tiên, tôi đề xuất 5 sơ đồ sau:

End-to-End CAB Business Process
Driver Matching Process
Trip Lifecycle / State Diagram
Payment Process
Exception Handling Process

Trong đó, Driver Matching Process + Trip Lifecycle là hai quy trình cần workshop kỹ nhất, vì chúng chứa phần lớn Business Rules, Exception Cases và các requirement chưa được khách hàng xác định.



Bước 7: Phân rã yêu cầu nghiệp vụ
Phân rã yêu cầu nghiệp vụ – CAB System

Sau khi đã xác định Business Goal → Scope → Business Process → Business Requirement, bước tiếp theo là phân rã Business Requirement thành các yêu cầu nhỏ hơn để có thể chuyển sang Functional Requirement, Use Case/User Story và Acceptance Criteria.

Có thể dùng cấu trúc:

Business Goal
    ↓
Business Requirement (BR)
    ↓
Business Sub-Requirement (BR-S)
    ↓
Functional Requirement (FR)
    ↓
Use Case / User Story
    ↓
Acceptance Criteria

1. Tổng quan phân rã
Business GoalXây dựng nền tảng CAB
BR-01Quản lý tài khoản
BR-02Đặt xe
BR-03Driver Matching
BR-04Trip Management
BR-05Location
BR-06Fare
BR-07Payment
BR-08Notification
BR-09Rating
BR-10Operation
BR-11Reporting
BR-12Security
BR-13Audit
BR-14Scalability
BR-15Resilience
BR-16Extensibility
2. BR-01 – Quản lý tài khoản
Business Requirement

Hệ thống phải hỗ trợ Customer, Driver và Staff quản lý tài khoản và thông tin cá nhân một cách an toàn.

Phân rã
ID	Sub-Requirement
BR-01.01	Customer có thể đăng ký tài khoản
BR-01.02	User có thể đăng nhập/đăng xuất
BR-01.03	User có thể cập nhật thông tin cá nhân
BR-01.04	Driver có thể quản lý hồ sơ
BR-01.05	Driver có thể quản lý thông tin phương tiện
BR-01.06	Operation có thể tạo/quản lý Driver
BR-01.07	Hệ thống phải xác thực người dùng trước khi truy cập chức năng yêu cầu tài khoản
Chuyển thành Functional Requirement
FR-01.01 Register Customer
FR-01.02 Login
FR-01.03 Logout
FR-01.04 Update Profile
FR-01.05 Manage Driver Profile
FR-01.06 Manage Vehicle

3. BR-02 – Đặt xe
Business Requirement

Hệ thống phải cho phép Customer tạo và quản lý yêu cầu đặt xe.

Phân rã
BR-02 Booking
│
├── BR-02.01 Nhập Pickup Location
├── BR-02.02 Nhập Destination
├── BR-02.03 Chọn Service / Vehicle Type
├── BR-02.04 Kiểm tra thông tin Booking
├── BR-02.05 Tạo Booking
├── BR-02.06 Sinh Booking ID
├── BR-02.07 Theo dõi trạng thái Booking
└── BR-02.08 Hủy Booking theo policy

Functional Requirement tương ứng
ID	Functional Requirement
FR-02.01	System shall allow Customer to enter pickup location
FR-02.02	System shall allow Customer to enter destination
FR-02.03	System shall allow Customer to select service type
FR-02.04	System shall validate booking information
FR-02.05	System shall create booking
FR-02.06	System shall generate unique booking ID
FR-02.07	System shall display booking status
FR-02.08	System shall process cancellation according to policy
4. BR-03 – Driver Matching

Đây là requirement nên phân rã sâu nhất.

Business Requirement

Hệ thống phải tự động tìm và phân công Driver phù hợp với Booking.

Phân rã
BR-03 Driver Matching
│
├── BR-03.01 Xác định Driver Candidate
│   ├── Driver Available
│   ├── Vehicle phù hợp
│   ├── Location hợp lệ
│   └── Không đang thực hiện Trip khác
│
├── BR-03.02 Xếp hạng Candidate
│   ├── Khoảng cách
│   ├── Trạng thái
│   └── Business Priority
│
├── BR-03.03 Gửi Trip Request
│
├── BR-03.04 Xử lý Driver Response
│   ├── Accept
│   ├── Reject
│   └── Timeout
│
├── BR-03.05 Reassign
│
├── BR-03.06 Xử lý No Driver Found
│
└── BR-03.07 Thông báo kết quả

Luồng nghiệp vụ
Accept
Reject
Timeout
No Candidate
Booking Created
Find Candidate
Filter Eligible Driver
Rank Driver
Send Request
Response?
Accept
Reject
Timeout
Assign Driver
Next Candidate
No Driver Found
Notify Customer
Open Business Rules

Chưa nên tự quyết định:

Driver phải phản hồi trong bao nhiêu giây?
Bán kính tìm kiếm bao nhiêu km?
Ưu tiên khoảng cách hay rating?
Có ưu tiên Driver lâu chưa nhận chuyến?
Retry tối đa bao nhiêu lần?
Khi nào chuyển NO_DRIVER_FOUND?

Đây là các Business Rules cần xác nhận.

5. BR-04 – Trip Management
Business Requirement

Hệ thống phải quản lý toàn bộ vòng đời của Trip.

Phân rã
BR-04 Trip
│
├── BR-04.01 Requested
├── BR-04.02 Searching Driver
├── BR-04.03 Driver Assigned
├── BR-04.04 Driver Arriving
├── BR-04.05 Driver Arrived
├── BR-04.06 Passenger Picked Up
├── BR-04.07 In Progress
├── BR-04.08 Completed
├── BR-04.09 Cancelled
├── BR-04.10 No Driver Found
└── BR-04.11 Payment Status

Các yêu cầu con
ID	Requirement
BR-04.01	Customer có thể xem trạng thái Trip
BR-04.02	Driver có thể cập nhật trạng thái Trip
BR-04.03	Operation có thể xem Trip đang diễn ra
BR-04.04	Hệ thống phải kiểm soát trạng thái hợp lệ
BR-04.05	Hệ thống phải xử lý trạng thái ngoại lệ
BR-04.06	Trip Completed phải được chuyển sang quy trình Fare
6. BR-05 – Driver Location
Business Requirement

Hệ thống phải sử dụng thông tin vị trí Driver để hỗ trợ Matching, ETA và Operation.

Phân rã
BR-05 Location
│
├── BR-05.01 Driver gửi vị trí
├── BR-05.02 Hệ thống cập nhật vị trí
├── BR-05.03 Sử dụng vị trí cho Matching
├── BR-05.04 Hỗ trợ tính ETA
├── BR-05.05 Operation xem vị trí theo quyền
└── BR-05.06 Bảo vệ dữ liệu vị trí

Open Issues
GPS update frequency?
Location accuracy?
Storage duration?
Có lưu historical location không?
7. BR-06 – Fare Management
Business Requirement

Hệ thống phải xác định số tiền Customer cần trả dựa trên loại dịch vụ và Fare Rules.

Phân rã
BR-06 Fare
│
├── BR-06.01 Xác định Service Type
├── BR-06.02 Thu thập Trip Data
├── BR-06.03 Áp dụng Fare Rules
├── BR-06.04 Tính Base Fare
├── BR-06.05 Tính Additional Charge
├── BR-06.06 Tính Total Fare
├── BR-06.07 Lưu Fare
└── BR-06.08 Hiển thị Fare

Fare Rules cần xác nhận
Base Fare
+ Distance Charge
+ Time Charge
+ Surcharge
- Discount
= Total Fare


Đây là mô hình minh họa, không phải công thức chính thức của ABC.

8. BR-07 – Payment
Business Requirement

Hệ thống phải hỗ trợ Cash và Electronic Payment và quản lý trạng thái giao dịch.

Phân rã
BR-07 Payment
│
├── BR-07.01 Chọn Payment Method
├── BR-07.02 Cash Payment
├── BR-07.03 Electronic Payment
├── BR-07.04 Payment Request
├── BR-07.05 Nhận Payment Result
├── BR-07.06 Payment Success
├── BR-07.07 Payment Failure
├── BR-07.08 Payment Retry
├── BR-07.09 Transaction History
└── BR-07.10 Không lưu Sensitive Payment Data

Payment Flow
Success
Failed
Trip Completed
Calculate Fare
Payment
Result
Success
Failed
Retry
9. BR-08 – Notification
Business Requirement

Hệ thống phải thông báo cho Customer và Driver khi xảy ra các sự kiện quan trọng.

Phân rã
BR-08 Notification
│
├── BR-08.01 Booking Received
├── BR-08.02 Driver Assigned
├── BR-08.03 Driver Arrived
├── BR-08.04 Trip Completed
├── BR-08.05 Payment Result
├── BR-08.06 New Trip for Driver
├── BR-08.07 Trip Changed
└── BR-08.08 Trip Cancelled

Kiến trúc nghiệp vụ
Business Event
      ↓
Notification Service
      ↓
Notification Provider
      ↓
Customer / Driver


Điểm quan trọng:

Notification failure không được làm Booking/Trip failure.

10. BR-09 – Rating
Business Requirement

Customer có thể đánh giá Driver sau khi Trip hoàn thành.

Phân rã
BR-09 Rating
│
├── BR-09.01 Cho phép Rating sau Completed
├── BR-09.02 Chấm điểm
├── BR-09.03 Nhập Comment nếu có
├── BR-09.04 Lưu Rating
├── BR-09.05 Tra cứu Rating
└── BR-09.06 Sử dụng Rating cho Driver Performance

Open Issues
Rating 1–5?
Có comment?
Có sửa rating?
Một trip được rating bao nhiêu lần?
11. BR-10 – Operation Management
Business Requirement

Hệ thống phải cung cấp công cụ tập trung để Operation quản lý và giám sát hoạt động CAB.

Phân rã
BR-10 Operation
│
├── BR-10.01 Customer Management
├── BR-10.02 Driver Management
├── BR-10.03 Vehicle Management
├── BR-10.04 Trip Monitoring
├── BR-10.05 Driver Monitoring
├── BR-10.06 Transaction Search
├── BR-10.07 Exception Handling
└── BR-10.08 Operational Support

Exception

Operation có thể cần hỗ trợ:

No Driver Found.
Driver mất kết nối.
Trip stuck.
Payment failure.
Customer complaint.
12. BR-11 – Reporting
Business Requirement

Hệ thống phải cung cấp dữ liệu và báo cáo giúp doanh nghiệp đánh giá hiệu quả hoạt động.

Phân rã
BR-11 Reporting
│
├── BR-11.01 Trip Volume
├── BR-11.02 Revenue
├── BR-11.03 Completion Rate
├── BR-11.04 Cancellation Rate
├── BR-11.05 Driver Performance
├── BR-11.06 Payment Statistics
└── BR-11.07 Operational Metrics

KPI
KPI	Ý nghĩa
Total Trips	Tổng số chuyến
Completed Trips	Chuyến hoàn thành
Completion Rate	Tỷ lệ hoàn thành
Cancellation Rate	Tỷ lệ hủy
Revenue	Doanh thu
Driver Acceptance Rate	Tỷ lệ Driver nhận chuyến
Matching Time	Thời gian tìm Driver
13. BR-12 – Security & Access Control
Business Requirement

Hệ thống phải bảo vệ dữ liệu và kiểm soát quyền truy cập theo vai trò.

Phân rã
BR-12 Security
│
├── BR-12.01 Authentication
├── BR-12.02 Authorization
├── BR-12.03 Role Management
├── BR-12.04 Personal Data Protection
├── BR-12.05 Vehicle Data Protection
├── BR-12.06 Location Data Protection
├── BR-12.07 Transaction Data Protection
└── BR-12.08 Sensitive Operation Control

Role
Customer
Driver
Operation Staff
Administrator

14. BR-13 – Audit
Business Requirement

Hệ thống phải lưu vết các thao tác quan trọng để phục vụ kiểm tra và điều tra sự cố.

Phân rã
BR-13 Audit
│
├── BR-13.01 Record Login
├── BR-13.02 Record Permission Changes
├── BR-13.03 Record Trip Changes
├── BR-13.04 Record Payment Changes
├── BR-13.05 Record Admin Actions
└── BR-13.06 Audit Search


Audit record tối thiểu nên có:

Actor
Action
Timestamp
Object
Before Value
After Value
Result

15. BR-14 – Scalability
Business Requirement

Hệ thống phải có khả năng mở rộng khi số lượng Customer, Driver và Trip tăng.

Phân rã
BR-14 Scalability
│
├── BR-14.01 Scale Booking
├── BR-14.02 Scale Matching
├── BR-14.03 Scale Notification
├── BR-14.04 Scale Payment
├── BR-14.05 Handle Peak Load
└── BR-14.06 Independent Component Scaling


Đây là Business Requirement nhưng các con số như:

10,000 concurrent users
1,000 requests/sec

chưa nên tự đặt nếu khách hàng chưa cung cấp. Đây là phần cần xác định trong NFR workshop.

16. BR-15 – Resilience
Business Requirement

Sự cố của một thành phần không được làm dừng toàn bộ hoạt động CAB.

Phân rã
BR-15 Resilience
│
├── BR-15.01 Payment Failure Isolation
├── BR-15.02 Notification Failure Isolation
├── BR-15.03 Retry Failed Operations
├── BR-15.04 Preserve Booking Data
├── BR-15.05 Preserve Trip Data
├── BR-15.06 Exception Monitoring
└── BR-15.07 Operational Recovery

Ví dụ
Payment Provider DOWN
        ↓
Payment = FAILED/PENDING
        ↓
Trip vẫn tồn tại
        ↓
Customer được thông báo
        ↓
Retry theo Policy

17. BR-16 – Extensibility
Business Requirement

Hệ thống phải có khả năng bổ sung dịch vụ và thay đổi các thành phần tích hợp mà hạn chế ảnh hưởng đến hệ thống hiện tại.

Phân rã
BR-16 Extensibility
│
├── BR-16.01 Add New Service Type
├── BR-16.02 Add New Vehicle Type
├── BR-16.03 Add Payment Method
├── BR-16.04 Add Payment Provider
├── BR-16.05 Add Notification Provider
├── BR-16.06 Change External Provider
└── BR-16.07 Deploy New Feature Independently

18. Tổng hợp cây phân rã
mindmap
  root((CAB Business Requirements))
    Customer
      BR-01 Account
      BR-02 Booking
      BR-04 Trip
      BR-06 Fare
      BR-07 Payment
      BR-08 Notification
      BR-09 Rating

    Driver
      BR-01 Account
      BR-03 Matching
      BR-04 Trip
      BR-05 Location
      BR-08 Notification

    Operation
      BR-10 Operation
      BR-11 Reporting
      BR-13 Audit

    Business
      BR-06 Fare
      BR-07 Payment
      BR-11 Reporting

    Platform
      BR-12 Security
      BR-14 Scalability
      BR-15 Resilience
      BR-16 Extensibility

19. Traceability Matrix

Đây là phần quan trọng để chứng minh requirement không bị "rơi".

Business Goal	Business Requirement	Sub-Requirement	Functional Area
BG01 Automation	BR-02 Booking	BR-02.01–08	Booking
BG01 Automation	BR-03 Matching	BR-03.01–07	Matching
BG02 Customer Experience	BR-04 Trip	BR-04.01–06	Trip
BG02 Customer Experience	BR-08 Notification	BR-08.01–08	Notification
BG02 Customer Experience	BR-09 Rating	BR-09.01–06	Rating
BG03 Driver Optimization	BR-03 Matching	BR-03.01–07	Matching
BG03 Driver Optimization	BR-05 Location	BR-05.01–06	Location
BG04 Revenue	BR-06 Fare	BR-06.01–08	Fare
BG04 Revenue	BR-07 Payment	BR-07.01–10	Payment
BG05 Operation	BR-10 Operation	BR-10.01–08	Admin Portal
BG05 Operation	BR-11 Reporting	BR-11.01–07	Reporting
BG06 Stability	BR-12 Security	BR-12.01–08	Security
BG06 Stability	BR-13 Audit	BR-13.01–06	Audit
BG06 Stability	BR-14 Scalability	BR-14.01–06	Architecture
BG06 Stability	BR-15 Resilience	BR-15.01–07	Resilience
BG07 Future Growth	BR-16 Extensibility	BR-16.01–07	Platform
20. Từ Business Requirement sang Functional Requirement

Ví dụ với BR-03 Driver Matching:

BR-03
Tự động tìm và phân công Driver
│
├── BR-03.01
│   Xác định Candidate
│   │
│   ├── FR-03.01
│   │   Kiểm tra Driver Available
│   │
│   ├── FR-03.02
│   │   Kiểm tra Vehicle Type
│   │
│   └── FR-03.03
│       Xác định Driver Location
│
├── BR-03.02
│   Ranking Candidate
│   │
│   ├── FR-03.04
│   │   Tính khoảng cách
│   │
│   └── FR-03.05
│       Áp dụng Priority Rule
│
├── BR-03.03
│   Gửi Request
│   │
│   └── FR-03.06
│       Gửi Trip Request cho Driver
│
└── BR-03.04
    Xử lý Response
    │
    ├── FR-03.07 Accept
    ├── FR-03.08 Reject
    └── FR-03.09 Timeout


Sau đó mới tiếp tục:

FR-03.07
    ↓
Use Case: Accept Trip
    ↓
User Story:
"As a Driver, I want to accept a trip..."
    ↓
Acceptance Criteria
    ↓
Test Case

21. Nguyên tắc phân rã requirement

Khi làm BA cho case này, nên kiểm tra mỗi requirement theo 5 câu hỏi:

Who? – Ai cần?
What? – Cần làm gì?
Why? – Vì sao cần?
When/Condition? – Khi nào/điều kiện gì?
Exception? – Nếu xảy ra lỗi thì sao?

Ví dụ:

BR-03: Doanh nghiệp cần hệ thống tự động tìm Driver.

Chưa đủ để phát triển.

Phân rã thành:

BR-03.01: Xác định Driver phù hợp.
BR-03.02: Ưu tiên Driver theo business rules.
BR-03.03: Gửi yêu cầu cho Driver.
BR-03.04: Xử lý Accept/Reject/Timeout.
BR-03.05: Tìm Driver tiếp theo.
BR-03.06: Xử lý trường hợp không có Driver.

Sau bước này mới chuyển sang Functional Requirements.

Deliverable BA tiếp theo

Với CAB System, sau Business Requirement Decomposition, cấu trúc tài liệu hợp lý sẽ là:

Business Goal
      ↓
Scope
      ↓
Business Process
      ↓
Business Requirement
      ↓
Business Requirement Decomposition   ← ĐANG Ở ĐÂY
      ↓
Business Rules
      ↓
Use Case
      ↓
Functional Requirement
      ↓
User Story
      ↓
Acceptance Criteria
      ↓
RTM


Trong đó nên ưu tiên phân rã chi tiết trước cho Booking → Driver Matching → Trip → Fare → Payment, vì đây là core business flow của CAB và cũng là nơi phát sinh nhiều exception nhất.



Bước 8: Thiết kế Business Rules và Acceptance Criteria
Dưới đây là phần Business Rules + Acceptance Criteria (AC) cho CAB System, viết theo dạng Markdown để có thể đưa trực tiếp vào tài liệu BA/BRD.

Lưu ý: Một số chính sách khách hàng chưa chốt như thời gian timeout, công thức tính cước, phí hủy... nên mình đánh dấu [TBD] thay vì tự đặt giá trị.

Business Rules & Acceptance Criteria – CAB System
1. Tổng quan

Business Rules định nghĩa các quy tắc nghiệp vụ mà hệ thống CAB phải tuân thủ.

Acceptance Criteria dùng để xác định điều kiện để một requirement được xem là đã đáp ứng đúng yêu cầu nghiệp vụ.

Business Goal
    ↓
Business Requirement
    ↓
Business Rule
    ↓
Functional Requirement
    ↓
Acceptance Criteria
    ↓
Test Case

2. Quy ước
Business Rule ID
BR-[Module]-[Number]


Ví dụ:

BR-BOOK-001
BR-MATCH-001
BR-TRIP-001

Acceptance Criteria

Sử dụng format Given – When – Then:

Given [điều kiện ban đầu]
When [hành động]
Then [kết quả mong đợi]

3. Account Management
3.1 Business Rules
ID	Business Rule
BR-ACC-001	Người dùng phải đăng nhập trước khi sử dụng các chức năng yêu cầu tài khoản.
BR-ACC-002	Mỗi tài khoản phải có định danh duy nhất.
BR-ACC-003	Customer chỉ được truy cập dữ liệu thuộc tài khoản của mình.
BR-ACC-004	Driver chỉ được cập nhật hồ sơ và phương tiện thuộc tài khoản được cấp.
BR-ACC-005	Operation chỉ được thực hiện chức năng theo quyền được cấp.
BR-ACC-006	Thông tin xác thực phải được bảo vệ và không được lưu dưới dạng plaintext.
3.2 Acceptance Criteria
AC-ACC-001 – Login
Scenario: Đăng nhập thành công
Given User có tài khoản hợp lệ
And thông tin đăng nhập chính xác
When User thực hiện đăng nhập
Then hệ thống xác thực User
And cho phép User truy cập hệ thống

AC-ACC-002 – Login thất bại
Scenario: Đăng nhập với thông tin không hợp lệ
Given User cung cấp thông tin đăng nhập không chính xác
When User thực hiện đăng nhập
Then hệ thống từ chối đăng nhập
And hiển thị thông báo lỗi phù hợp

AC-ACC-003 – Unauthorized Access
Scenario: User chưa đăng nhập truy cập chức năng yêu cầu authentication
Given User chưa đăng nhập
When User truy cập chức năng yêu cầu tài khoản
Then hệ thống từ chối truy cập
And yêu cầu User đăng nhập

4. Booking Management
4.1 Business Rules
ID	Business Rule
BR-BOOK-001	Chỉ Customer đã xác thực mới được tạo Booking.
BR-BOOK-002	Booking phải có Pickup Location.
BR-BOOK-003	Booking phải có Destination.
BR-BOOK-004	Booking phải có Service/Vehicle Type hợp lệ.
BR-BOOK-005	Mỗi Booking phải có một Booking ID duy nhất.
BR-BOOK-006	Booking mới tạo phải có trạng thái xác định.
BR-BOOK-007	Customer chỉ được hủy Booking khi trạng thái hiện tại cho phép hủy.
BR-BOOK-008	Chính sách phí hủy được áp dụng theo policy của doanh nghiệp.
4.2 Acceptance Criteria
AC-BOOK-001 – Create Booking
Scenario: Customer tạo Booking thành công
Given Customer đã đăng nhập
And Pickup Location hợp lệ
And Destination hợp lệ
And Vehicle Type hợp lệ
When Customer gửi yêu cầu đặt xe
Then hệ thống tạo Booking
And sinh Booking ID duy nhất
And Booking có trạng thái REQUESTED
And hệ thống bắt đầu quy trình tìm Driver

AC-BOOK-002 – Missing Pickup
Scenario: Không nhập điểm đón
Given Customer đang tạo Booking
When Customer không cung cấp Pickup Location
Then hệ thống không tạo Booking
And yêu cầu Customer nhập Pickup Location

AC-BOOK-003 – Missing Destination
Scenario: Không nhập điểm đến
Given Customer đang tạo Booking
When Customer không cung cấp Destination
Then hệ thống không tạo Booking
And yêu cầu Customer nhập Destination

AC-BOOK-004 – Track Booking
Scenario: Customer theo dõi Booking
Given Customer có Booking hợp lệ
When Customer xem trạng thái Booking
Then hệ thống hiển thị trạng thái hiện tại
And hiển thị thông tin Driver nếu Driver đã được assign

5. Driver Matching

Đây là phần quan trọng nhất của Business Rules.

5.1 Business Rules
ID	Business Rule
BR-MATCH-001	Chỉ Driver đủ điều kiện mới được đưa vào danh sách Candidate.
BR-MATCH-002	Driver phải ở trạng thái Available để nhận Booking mới.
BR-MATCH-003	Driver phải có Vehicle phù hợp với Service Type của Booking.
BR-MATCH-004	Driver đang thực hiện Trip không được nhận Trip mới.
BR-MATCH-005	Hệ thống phải ưu tiên Candidate theo tiêu chí do Operation quy định.
BR-MATCH-006	Driver được gửi Trip Request phải có khả năng phản hồi.
BR-MATCH-007	Driver Accept thì Booking được assign cho Driver đó.
BR-MATCH-008	Driver Reject thì hệ thống tiếp tục tìm Candidate khác.
BR-MATCH-009	Driver không phản hồi trong thời gian [TBD] được xem là Timeout.
BR-MATCH-010	Sau Reject/Timeout, Customer không phải tạo lại Booking.
BR-MATCH-011	Một Booking chỉ được assign cho một Driver tại một thời điểm.
BR-MATCH-012	Nếu không còn Candidate phù hợp, Booking chuyển sang NO_DRIVER_FOUND.
BR-MATCH-013	Customer phải được thông báo khi không tìm được Driver.
5.2 Acceptance Criteria
AC-MATCH-001 – Find Candidate
Scenario: Tìm Driver phù hợp
Given Booking có trạng thái REQUESTED
When hệ thống bắt đầu Driver Matching
Then hệ thống tìm các Driver đang Available
And Driver phải phù hợp với Vehicle/Service Type
And Driver đang có Trip khác không được chọn

AC-MATCH-002 – Driver Accept
Scenario: Driver chấp nhận Trip
Given Driver đã nhận Trip Request
And Booking chưa được assign cho Driver khác
When Driver Accept Trip
Then hệ thống assign Booking cho Driver
And Booking chuyển sang DRIVER_ASSIGNED
And Customer được thông báo

AC-MATCH-003 – Driver Reject
Scenario: Driver từ chối Trip
Given Driver đã nhận Trip Request
When Driver Reject Trip
Then Booking không được assign cho Driver đó
And hệ thống tìm Candidate tiếp theo
And Customer không phải tạo lại Booking

AC-MATCH-004 – Driver Timeout
Scenario: Driver không phản hồi
Given Driver đã nhận Trip Request
When Driver không phản hồi trong thời gian quy định
Then hệ thống đánh dấu Request là Timeout
And hệ thống tìm Candidate tiếp theo
And Booking vẫn được duy trì

AC-MATCH-005 – No Driver Found
Scenario: Không tìm được Driver
Given hệ thống đã kiểm tra tất cả Candidate phù hợp
And không có Driver nào Accept
When quá trình Matching kết thúc
Then Booking chuyển sang NO_DRIVER_FOUND
And Customer được thông báo

AC-MATCH-006 – Concurrent Accept
Scenario: Hai Driver cùng Accept một Booking
Given nhiều Driver đang nhận cùng một Booking Request
When nhiều Driver gửi Accept gần như đồng thời
Then hệ thống chỉ assign Booking cho một Driver
And các Driver còn lại không được assign Booking đó
And trạng thái Booking phải nhất quán

6. Trip Management
6.1 Business Rules
ID	Business Rule
BR-TRIP-001	Trip được tạo từ Booking hợp lệ.
BR-TRIP-002	Trip phải có trạng thái hợp lệ trong toàn bộ lifecycle.
BR-TRIP-003	Driver chỉ được cập nhật các trạng thái thuộc quyền của Driver.
BR-TRIP-004	Customer được theo dõi trạng thái Trip nhưng không được tự thay đổi trạng thái vận hành.
BR-TRIP-005	Driver phải cập nhật trạng thái theo trình tự nghiệp vụ hợp lệ.
BR-TRIP-006	Trip chỉ được chuyển sang COMPLETED khi điều kiện hoàn thành được đáp ứng.
BR-TRIP-007	Trip đã COMPLETED không được chuyển ngược sang trạng thái đang thực hiện.
BR-TRIP-008	Các thay đổi trạng thái quan trọng phải được audit.
6.2 State Transition
stateDiagram-v2
    [*] --> REQUESTED
    REQUESTED --> SEARCHING_DRIVER
    SEARCHING_DRIVER --> DRIVER_ASSIGNED
    SEARCHING_DRIVER --> NO_DRIVER_FOUND

    DRIVER_ASSIGNED --> DRIVER_ARRIVING
    DRIVER_ARRIVING --> DRIVER_ARRIVED
    DRIVER_ARRIVED --> PASSENGER_PICKED_UP
    PASSENGER_PICKED_UP --> IN_PROGRESS
    IN_PROGRESS --> COMPLETED

    REQUESTED --> CANCELLED
    SEARCHING_DRIVER --> CANCELLED
    DRIVER_ASSIGNED --> CANCELLED
    DRIVER_ARRIVING --> CANCELLED

    COMPLETED --> PAYMENT_PENDING
    PAYMENT_PENDING --> PAID
    PAYMENT_PENDING --> PAYMENT_FAILED
    PAYMENT_FAILED --> PAYMENT_PENDING

    CANCELLED --> [*]
    NO_DRIVER_FOUND --> [*]
    PAID --> [*]

6.3 Acceptance Criteria
AC-TRIP-001
Scenario: Driver cập nhật trạng thái đã đến
Given Trip đang ở trạng thái DRIVER_ARRIVING
When Driver cập nhật trạng thái DRIVER_ARRIVED
Then hệ thống cập nhật trạng thái Trip
And Customer có thể xem trạng thái mới
And hệ thống gửi thông báo cho Customer

AC-TRIP-002
Scenario: Không cho phép chuyển trạng thái không hợp lệ
Given Trip đang ở trạng thái DRIVER_ARRIVING
When Driver cố chuyển Trip trực tiếp sang COMPLETED
Then hệ thống từ chối thao tác
And trạng thái Trip không thay đổi

AC-TRIP-003
Scenario: Hoàn thành Trip
Given Customer đã được đón
And Trip đang ở trạng thái IN_PROGRESS
When Driver hoàn thành Trip
Then Trip chuyển sang COMPLETED
And hệ thống bắt đầu quy trình tính cước

7. Location Management
7.1 Business Rules
ID	Business Rule
BR-LOC-001	Driver đang hoạt động có thể cung cấp vị trí hiện tại.
BR-LOC-002	Location được sử dụng để hỗ trợ Driver Matching.
BR-LOC-003	Location được sử dụng để hỗ trợ ETA.
BR-LOC-004	Chỉ Actor có quyền mới được xem dữ liệu Location.
BR-LOC-005	Location phải được bảo vệ theo chính sách bảo mật.
BR-LOC-006	Thời gian lưu trữ Location là [TBD].
7.2 Acceptance Criteria
Scenario: Cập nhật vị trí Driver
Given Driver đang ở trạng thái Available
When thiết bị gửi Location hợp lệ
Then hệ thống cập nhật vị trí mới nhất của Driver
And Location có thể được sử dụng cho Matching

Scenario: Unauthorized access Location
Given User không có quyền xem Driver Location
When User yêu cầu xem Location
Then hệ thống từ chối truy cập

8. Fare Management
8.1 Business Rules
ID	Business Rule
BR-FARE-001	Fare chỉ được tính khi Trip đáp ứng điều kiện tính cước.
BR-FARE-002	Fare phải dựa trên Service Type và Fare Rules hiện hành.
BR-FARE-003	Hệ thống phải lưu kết quả tính cước.
BR-FARE-004	Customer phải được biết số tiền phải trả.
BR-FARE-005	Các loại phụ phí phải tuân thủ Fare Policy.
BR-FARE-006	Công thức Fare chi tiết là [TBD].
8.2 Acceptance Criteria
Scenario: Tính Fare sau khi Trip hoàn thành
Given Trip đã COMPLETED
And Fare Rules hợp lệ
When hệ thống thực hiện tính cước
Then hệ thống xác định Total Fare
And lưu kết quả Fare
And Customer có thể xem số tiền phải trả

AC-FARE-002 – Invalid Fare Rule
Scenario: Không có Fare Rule phù hợp
Given Trip đã COMPLETED
And hệ thống không tìm thấy Fare Rule phù hợp
When hệ thống tính Fare
Then hệ thống không tạo Fare sai
And ghi nhận lỗi
And Trip vẫn được lưu
And Operation có thể xử lý ngoại lệ

9. Payment
9.1 Business Rules
ID	Business Rule
BR-PAY-001	Customer có thể thanh toán bằng Cash hoặc Electronic Payment nếu phương thức được hỗ trợ.
BR-PAY-002	Electronic Payment phải được xử lý thông qua Payment Provider.
BR-PAY-003	CAB không lưu thông tin thẻ/tài khoản thanh toán nhạy cảm.
BR-PAY-004	Mỗi Payment Transaction phải có định danh duy nhất.
BR-PAY-005	Payment phải có trạng thái rõ ràng.
BR-PAY-006	Payment thất bại không được làm mất Trip.
BR-PAY-007	Payment có thể retry theo Payment Policy.
BR-PAY-008	Payment Result phải được ghi nhận để tra cứu.
9.2 Acceptance Criteria
AC-PAY-001 – Cash
Scenario: Thanh toán tiền mặt
Given Trip đã COMPLETED
And Customer chọn Cash
When hệ thống ghi nhận thanh toán tiền mặt
Then Payment được ghi nhận theo trạng thái phù hợp
And Transaction được lưu

AC-PAY-002 – Electronic Payment Success
Scenario: Thanh toán điện tử thành công
Given Customer chọn Electronic Payment
And Payment Provider hoạt động
When Payment Provider trả về SUCCESS
Then Payment chuyển sang SUCCESS
And Customer được thông báo thanh toán thành công
And Transaction được lưu

AC-PAY-003 – Electronic Payment Failed
Scenario: Thanh toán điện tử thất bại
Given Customer đang thực hiện Electronic Payment
When Payment Provider trả về FAILED
Then Payment chuyển sang PAYMENT_FAILED
And Customer được thông báo
And Customer có thể retry theo policy
And Trip data không bị mất

AC-PAY-004 – Sensitive Data
Scenario: Xử lý thông tin thanh toán
Given Customer thực hiện Electronic Payment
When hệ thống xử lý Payment
Then thông tin nhạy cảm của thẻ/tài khoản không được lưu trực tiếp trong CAB

10. Notification
10.1 Business Rules
ID	Business Rule
BR-NOTI-001	Customer phải nhận thông báo khi Booking được tiếp nhận.
BR-NOTI-002	Customer phải được thông báo khi Driver được assign.
BR-NOTI-003	Customer phải được thông báo khi Driver đến Pickup.
BR-NOTI-004	Customer phải được thông báo khi Trip hoàn thành.
BR-NOTI-005	Customer phải được thông báo về Payment Result.
BR-NOTI-006	Driver phải nhận thông báo về Trip mới.
BR-NOTI-007	Driver phải nhận thông báo về thay đổi liên quan đến Trip.
BR-NOTI-008	Notification Provider có thể thay đổi/mở rộng.
BR-NOTI-009	Notification failure không được làm dừng Booking/Trip.
10.2 Acceptance Criteria
Scenario: Driver được assign
Given Booking đã được Driver Accept
When hệ thống xác nhận Assignment
Then Customer nhận được thông báo Driver đã nhận chuyến
And Driver nhận được thông tin Trip

Scenario: Notification Provider gặp lỗi
Given Booking đã được tạo thành công
When Notification Provider không phản hồi
Then Booking vẫn tồn tại
And Trip workflow không bị rollback
And hệ thống ghi nhận lỗi Notification
And hệ thống có thể retry theo policy

11. Rating
11.1 Business Rules
ID	Business Rule
BR-RATE-001	Customer chỉ được đánh giá Trip đã COMPLETED.
BR-RATE-002	Customer chỉ được đánh giá Trip thuộc tài khoản của mình.
BR-RATE-003	Rating phải tuân thủ thang điểm được doanh nghiệp quy định.
BR-RATE-004	Chính sách chỉnh sửa rating là [TBD].
11.2 Acceptance Criteria
Scenario: Customer đánh giá Driver
Given Trip đã COMPLETED
And Trip thuộc Customer hiện tại
When Customer gửi Rating hợp lệ
Then hệ thống lưu Rating
And Rating được liên kết với Trip và Driver

Scenario: Đánh giá Trip chưa hoàn thành
Given Trip chưa COMPLETED
When Customer gửi Rating
Then hệ thống từ chối Rating
And không lưu dữ liệu Rating

12. Operation Management
12.1 Business Rules
ID	Business Rule
BR-OPS-001	Operation Staff có thể xem các Trip đang hoạt động theo quyền.
BR-OPS-002	Operation Staff có thể tra cứu Customer, Driver, Vehicle.
BR-OPS-003	Operation Staff có thể tra cứu Transaction.
BR-OPS-004	Các thao tác nhạy cảm phải yêu cầu permission phù hợp.
BR-OPS-005	Thao tác quản trị quan trọng phải được audit.
BR-OPS-006	Administrator có quyền cao hơn Operation Staff.
12.2 Acceptance Criteria
Scenario: Operation xem Active Trips
Given Operation Staff đã đăng nhập
And có quyền xem Trip
When Staff mở màn hình Active Trips
Then hệ thống hiển thị các Trip đang hoạt động
And hiển thị trạng thái hiện tại

Scenario: Staff không có quyền thực hiện thao tác nhạy cảm
Given Staff không có permission phù hợp
When Staff thực hiện thao tác nhạy cảm
Then hệ thống từ chối thao tác
And ghi nhận sự kiện nếu thuộc nhóm audit

13. Reporting
13.1 Business Rules
ID	Business Rule
BR-REP-001	Hệ thống phải cung cấp số lượng Trip.
BR-REP-002	Hệ thống phải cung cấp Revenue.
BR-REP-003	Hệ thống phải cung cấp Completion Rate.
BR-REP-004	Hệ thống phải cung cấp Cancellation Rate.
BR-REP-005	Hệ thống phải cung cấp Driver Performance.
BR-REP-006	Chỉ User có quyền mới được xem báo cáo quản trị.
13.2 Acceptance Criteria
Scenario: Xem báo cáo Trip
Given User có quyền xem Reporting
When User chọn khoảng thời gian báo cáo
Then hệ thống hiển thị tổng số Trip
And số Trip Completed
And số Trip Cancelled
And Completion Rate
And Cancellation Rate

14. Security & Authorization
14.1 Business Rules
ID	Business Rule
BR-SEC-001	User phải được authentication trước khi sử dụng chức năng protected.
BR-SEC-002	User chỉ được thực hiện action theo permission.
BR-SEC-003	Customer không được truy cập dữ liệu Customer khác.
BR-SEC-004	Driver không được truy cập dữ liệu quản trị.
BR-SEC-005	Operation Staff không được thực hiện chức năng chỉ dành cho Administrator.
BR-SEC-006	Sensitive data phải được bảo vệ.
BR-SEC-007	Các thao tác quan trọng phải có audit trail.
14.2 Acceptance Criteria
Scenario: Customer truy cập dữ liệu Customer khác
Given Customer A đã đăng nhập
When Customer A yêu cầu dữ liệu của Customer B
Then hệ thống từ chối truy cập
And không trả về dữ liệu của Customer B

Scenario: Operation truy cập chức năng Admin
Given Staff không có Admin Permission
When Staff truy cập chức năng Administrator
Then hệ thống từ chối truy cập

15. Audit
15.1 Business Rules
ID	Business Rule
BR-AUD-001	Login quan trọng phải được ghi nhận theo chính sách audit.
BR-AUD-002	Thay đổi quyền phải được audit.
BR-AUD-003	Thay đổi trạng thái Trip quan trọng phải được audit.
BR-AUD-004	Thao tác Payment quan trọng phải được audit.
BR-AUD-005	Admin Action phải được audit.
BR-AUD-006	Audit Log phải có Actor, Action, Timestamp và Object.
15.2 Acceptance Criteria
Scenario: Admin thay đổi trạng thái Trip
Given Admin có quyền xử lý Trip
When Admin thay đổi trạng thái Trip
Then hệ thống cập nhật trạng thái
And tạo Audit Log
And Audit Log chứa Actor
And Action
And Timestamp
And đối tượng bị thay đổi

16. Exception & Resilience

Đây là nhóm Business Rules rất quan trọng đối với yêu cầu:

"Lỗi thanh toán hoặc thông báo không được làm toàn bộ hệ thống đặt xe ngừng hoạt động."

16.1 Business Rules
ID	Business Rule
BR-RES-001	Payment failure không được làm mất Booking/Trip.
BR-RES-002	Notification failure không được làm mất Booking/Trip.
BR-RES-003	Các operation thất bại có thể retry theo policy.
BR-RES-004	Lỗi phải được ghi nhận để Operation xử lý.
BR-RES-005	Một component failure không được gây cascading failure cho toàn hệ thống.
BR-RES-006	Các exception quan trọng phải có monitoring/audit phù hợp.
16.2 Acceptance Criteria
Scenario: Payment Provider unavailable
Given Trip đã hoàn thành
And Payment Provider không khả dụng
When hệ thống gửi Payment Request
Then Payment được ghi nhận ở trạng thái phù hợp
And Trip data vẫn được giữ nguyên
And Customer được thông báo theo khả năng hiện có
And Operation có thể xử lý/retry

Scenario: Notification Service unavailable
Given Driver đã Accept Trip
When Notification Service gặp lỗi
Then Booking vẫn được assign
And Trip vẫn tiếp tục
And lỗi Notification được ghi nhận

17. Business Rules cho Scalability

Các yêu cầu này chưa thể tạo AC định lượng hoàn chỉnh vì khách hàng chưa đưa KPI kỹ thuật.

Business Rules
ID	Business Rule
BR-SCALE-001	Các thành phần có tải tăng cao phải có khả năng scale độc lập.
BR-SCALE-002	Peak Load không được làm mất Booking đang xử lý.
BR-SCALE-003	Việc mở rộng một component không được yêu cầu downtime toàn hệ thống nếu kiến trúc cho phép.
BR-SCALE-004	Các thành phần mới phải có khả năng được triển khai từng phần.
Acceptance Criteria sơ khởi
Scenario: Traffic tăng cao
Given hệ thống đang ở thời điểm nhu cầu đặt xe cao
When tải của Booking/Matching tăng
Then hệ thống phải tiếp tục tiếp nhận và xử lý Booking
And các component có tải cao có thể được mở rộng độc lập


NFR cần bổ sung: concurrency, TPS/RPS, response time, availability, recovery time... Đây là phần cần workshop với Technical Lead/Architect và khách hàng.

18. Business Rule Matrix tổng hợp
Domain	Rule Count	Priority
Account	6	Must
Booking	8	Must
Driver Matching	13	Critical
Trip	8	Critical
Location	6	Must
Fare	6	Critical
Payment	8	Critical
Notification	9	Must
Rating	4	Should
Operation	6	Must
Reporting	6	Must
Security	7	Critical
Audit	6	Must
Resilience	6	Critical
Scalability	4	Must
Total	107	
19. Acceptance Criteria Matrix

Có thể dùng bảng sau để quản lý traceability:

Requirement	Business Rule	Acceptance Criteria	Priority
Create Booking	BR-BOOK-001 → 008	AC-BOOK-001 → 004	Must
Driver Matching	BR-MATCH-001 → 013	AC-MATCH-001 → 006	Critical
Trip Lifecycle	BR-TRIP-001 → 008	AC-TRIP-001 → 003	Critical
Location	BR-LOC-001 → 006	AC-LOC-001 → 002	Must
Fare	BR-FARE-001 → 006	AC-FARE-001 → 002	Critical
Payment	BR-PAY-001 → 008	AC-PAY-001 → 004	Critical
Notification	BR-NOTI-001 → 009	AC-NOTI-001 → 002	Must
Rating	BR-RATE-001 → 004	AC-RATE-001 → 002	Should
Operation	BR-OPS-001 → 006	AC-OPS-001 → 002	Must
Reporting	BR-REP-001 → 006	AC-REP-001	Must
Security	BR-SEC-001 → 007	AC-SEC-001 → 002	Critical
Audit	BR-AUD-001 → 006	AC-AUD-001	Must
Resilience	BR-RES-001 → 006	AC-RES-001 → 002	Critical
Scalability	BR-SCALE-001 → 004	AC-SCALE-001	Must
20. Những Business Rules cần khách hàng xác nhận

Đây là phần rất quan trọng trong vai trò BA. Không nên tự đưa các giá trị này vào requirement chính thức.

ID	Nội dung cần xác nhận
TBD-01	Driver phải phản hồi trong bao lâu?
TBD-02	Retry tối đa bao nhiêu Driver?
TBD-03	Bán kính tìm Driver là bao nhiêu?
TBD-04	Tiêu chí ranking Driver?
TBD-05	Có ưu tiên Rating/Performance không?
TBD-06	Chính sách Customer cancel?
TBD-07	Cancellation fee?
TBD-08	Công thức tính Fare?
TBD-09	Có minimum fare không?
TBD-10	Có surge pricing không?
TBD-11	Có discount/voucher không?
TBD-12	Payment retry bao nhiêu lần?
TBD-13	Có Refund không?
TBD-14	Notification retry policy?
TBD-15	Location retention period?
TBD-16	Trip/Transaction retention period?
TBD-17	Xử lý Driver mất mạng?
TBD-18	Xử lý Customer mất mạng?
TBD-19	Rating theo thang điểm nào?
TBD-20	Thời gian hiệu lực của Fare Rule?
21. Mẫu chuẩn cho một Requirement hoàn chỉnh

Để tài liệu BA sau này nhất quán, mỗi requirement nên có cấu trúc:

## FR-MATCH-001 – Tìm Driver phù hợp

### Business Requirement

BR-03 – Driver Matching

### Business Rule

- BR-MATCH-001
- BR-MATCH-002
- BR-MATCH-003
- BR-MATCH-004

### Description

Hệ thống phải xác định các Driver phù hợp với Booking dựa
trên trạng thái hoạt động, loại phương tiện và các tiêu chí
vận hành được cấu hình.

### Preconditions

- Booking hợp lệ.
- Customer đã được xác thực.
- Booking đang ở trạng thái REQUESTED.

### Main Flow

1. Hệ thống nhận Booking.
2. Hệ thống lấy danh sách Driver.
3. Lọc Driver không đủ điều kiện.
4. Xếp hạng Driver.
5. Chọn Candidate.
6. Gửi Trip Request.
7. Chờ phản hồi.

### Acceptance Criteria

#### AC-001

Given Booking hợp lệ  
And Booking ở trạng thái REQUESTED  
When hệ thống bắt đầu Matching  
Then hệ thống phải tìm Driver phù hợp.

#### AC-002

Given Driver không Available  
When hệ thống tìm Candidate  
Then Driver không được đưa vào Candidate List.

#### AC-003

Given Driver Accept  
When hệ thống nhận Accept  
Then Booking được assign cho Driver.

#### AC-004

Given Driver Reject hoặc Timeout  
When hệ thống xử lý response  
Then hệ thống phải tiếp tục tìm Driver khác.

### Exceptions

- Không có Driver.
- Driver Reject.
- Driver Timeout.
- Driver mất kết nối.
- Concurrent Accept.

### Open Questions

- Timeout = [TBD]
- Search Radius = [TBD]
- Ranking Rule = [TBD]

22. Phân biệt Business Rule và Acceptance Criteria

Đây là điểm cần giữ rõ trong tài liệu BA:

Business Rule	Acceptance Criteria
Quy định hệ thống phải tuân thủ điều gì	Kiểm tra requirement đã đạt chưa
Mang tính nghiệp vụ	Mang tính kiểm chứng
Thường ổn định hơn	Có thể chi tiết hơn
Ví dụ: Driver Reject → tìm Driver khác	Given Reject → When Reject → Then tìm Driver khác
Có thể áp dụng cho nhiều chức năng	Thường gắn với một requirement

Ví dụ:

Business Rule
BR-MATCH-008:
Driver Reject thì hệ thống phải tiếp tục tìm Driver khác.

              ↓

Acceptance Criteria
AC-MATCH-003:
Given Driver đã nhận Trip Request
When Driver Reject
Then hệ thống tìm Candidate tiếp theo
And Customer không phải tạo lại Booking.


Đây là cấu trúc nên dùng xuyên suốt dự án CAB: BR định nghĩa luật nghiệp vụ, còn AC biến luật đó thành điều kiện có thể kiểm thử/ nghiệm thu.



Bước 9: Data Modeling 
Modeling Data – CAB System

Dưới đây là phần Data Modeling cho CAB System, xây dựng dựa trên các requirement, business rules và business process đã phân tích ở trên. Nội dung có thể đưa trực tiếp vào file .md.

1. Mục tiêu Data Modeling

Hệ thống CAB cần quản lý dữ liệu xuyên suốt vòng đời:

Customer
   ↓
Booking
   ↓
Driver Matching
   ↓
Trip
   ↓
Fare
   ↓
Payment
   ↓
Rating


Đồng thời hệ thống cần quản lý:

Driver ── Vehicle
   │
   └── Location

Staff ── Role ── Permission

Trip ── Notification
Trip ── Audit Log
Payment ── Transaction


Mục tiêu của Data Model:

Đảm bảo dữ liệu nhất quán.
Tránh lưu trùng dữ liệu.
Hỗ trợ truy vấn lịch sử Trip/Payment.
Hỗ trợ Driver Matching.
Hỗ trợ Reporting.
Bảo vệ dữ liệu nhạy cảm.
Có khả năng mở rộng thêm loại dịch vụ, phương thức thanh toán và notification provider.
2. Xác định các Entity chính
Core Entities
Entity	Mục đích
User	Thông tin tài khoản dùng chung
Customer	Thông tin khách hàng
Driver	Thông tin tài xế
Vehicle	Thông tin phương tiện
ServiceType	Loại dịch vụ/loại xe
Booking	Yêu cầu đặt xe
Trip	Chuyến đi thực tế
DriverAssignment	Lịch sử tìm và phân công Driver
DriverLocation	Vị trí Driver
Fare	Thông tin tính cước
Payment	Thông tin thanh toán
PaymentTransaction	Giao dịch với Payment Provider
Rating	Đánh giá Driver
Notification	Thông báo
Staff	Nhân viên vận hành
Role	Vai trò
Permission	Quyền
AuditLog	Nhật ký thao tác
3. ERD tổng quan
erDiagram

    USER ||--o| CUSTOMER : "has"
    USER ||--o| DRIVER : "has"
    USER ||--o| STAFF : "has"

    DRIVER ||--o{ VEHICLE : owns
    SERVICE_TYPE ||--o{ VEHICLE : supports

    CUSTOMER ||--o{ BOOKING : creates
    SERVICE_TYPE ||--o{ BOOKING : selected_by

    BOOKING ||--o| TRIP : creates
    BOOKING ||--o{ DRIVER_ASSIGNMENT : has
    DRIVER ||--o{ DRIVER_ASSIGNMENT : receives

    DRIVER ||--o{ DRIVER_LOCATION : sends

    TRIP ||--o| FARE : has
    TRIP ||--o{ PAYMENT : has
    PAYMENT ||--o{ PAYMENT_TRANSACTION : contains

    TRIP ||--o| RATING : receives
    CUSTOMER ||--o{ RATING : gives
    DRIVER ||--o{ RATING : receives

    TRIP ||--o{ NOTIFICATION : triggers
    USER ||--o{ NOTIFICATION : receives

    ROLE ||--o{ STAFF : assigned
    ROLE ||--o{ ROLE_PERMISSION : has
    PERMISSION ||--o{ ROLE_PERMISSION : grants

    USER ||--o{ AUDIT_LOG : performs

4. User Model

Một tài khoản có thể thuộc một trong các nhóm:

USER
 ├── CUSTOMER
 ├── DRIVER
 └── STAFF

Entity: User
Field	Type	Description
user_id	UUID	PK
username	String	Username
email	String	Email
phone	String	Số điện thoại
password_hash	String	Password đã hash
status	Enum	ACTIVE / INACTIVE / LOCKED
created_at	DateTime	Ngày tạo
updated_at	DateTime	Ngày cập nhật
Business Rule
Một User phải có thông tin authentication hợp lệ.

Password không được lưu plaintext.

5. Customer
Entity: Customer
Field	Type	Description
customer_id	UUID	PK/FK User
full_name	String	Họ tên
date_of_birth	Date	Ngày sinh
profile_image	String	Avatar
created_at	DateTime	Ngày tạo
updated_at	DateTime	Ngày cập nhật

Relationship:

USER 1 ───── 0..1 CUSTOMER


Một User có thể là Customer.

6. Driver
Entity: Driver
Field	Type	Description
driver_id	UUID	PK/FK User
full_name	String	Họ tên
license_number	String	GPLX
driver_status	Enum	OFFLINE / AVAILABLE / BUSY / SUSPENDED
rating_avg	Decimal	Điểm trung bình
created_at	DateTime	Ngày tạo
updated_at	DateTime	Ngày cập nhật
Driver State
stateDiagram-v2
    [*] --> OFFLINE

    OFFLINE --> AVAILABLE
    AVAILABLE --> BUSY
    BUSY --> AVAILABLE

    AVAILABLE --> OFFLINE
    BUSY --> OFFLINE

    AVAILABLE --> SUSPENDED
    OFFLINE --> SUSPENDED

7. Vehicle
Entity: Vehicle
Field	Type	Description
vehicle_id	UUID	PK
driver_id	UUID	FK Driver
service_type_id	UUID	FK ServiceType
license_plate	String	Biển số
brand	String	Hãng xe
model	String	Model
color	String	Màu
status	Enum	ACTIVE / INACTIVE
created_at	DateTime	Ngày tạo

Relationship:

DRIVER 1 ───── N VEHICLE

8. Service Type

Thiết kế ServiceType riêng giúp hệ thống có thể mở rộng.

Ví dụ:

CAR_4_SEAT
CAR_7_SEAT
PREMIUM
MOTORBIKE
...

Entity: ServiceType
Field	Type	Description
service_type_id	UUID	PK
code	String	Unique code
name	String	Tên dịch vụ
description	String	Mô tả
status	Enum	ACTIVE / INACTIVE

Thay vì hard-code:

if vehicle == "4 seat"


nên thiết kế:

Vehicle
    ↓
ServiceType


Điều này hỗ trợ Extensibility.

9. Booking

Booking đại diện cho yêu cầu đặt xe của Customer.

Entity: Booking
Field	Type	Description
booking_id	UUID	PK
customer_id	UUID	FK Customer
service_type_id	UUID	FK ServiceType
pickup_lat	Decimal	Latitude điểm đón
pickup_lng	Decimal	Longitude điểm đón
pickup_address	String	Địa chỉ đón
destination_lat	Decimal	Latitude điểm đến
destination_lng	Decimal	Longitude điểm đến
destination_address	String	Địa chỉ đến
status	Enum	Booking Status
requested_at	DateTime	Thời gian yêu cầu
cancelled_at	DateTime	Thời gian hủy
cancel_reason	String	Lý do hủy
Booking Status
REQUESTED
     ↓
SEARCHING_DRIVER
     ↓
DRIVER_ASSIGNED
     ↓
CANCELLED


Hoặc:

SEARCHING_DRIVER
     ↓
NO_DRIVER_FOUND

10. Driver Assignment

Không nên chỉ lưu driver_id trực tiếp trong Booking.

Nên có entity riêng:

DriverAssignment
Field	Type	Description
assignment_id	UUID	PK
booking_id	UUID	FK Booking
driver_id	UUID	FK Driver
sequence_no	Integer	Thứ tự Candidate
distance	Decimal	Khoảng cách
score	Decimal	Matching Score
status	Enum	OFFERED / ACCEPTED / REJECTED / TIMEOUT
offered_at	DateTime	Thời gian gửi
responded_at	DateTime	Thời gian phản hồi
Tại sao cần entity này?

Ví dụ:

Booking #B001

Driver A → REJECTED
Driver B → TIMEOUT
Driver C → ACCEPTED


Hệ thống phải lưu được lịch sử này.

Nếu chỉ có:

Booking.driver_id = Driver C


thì mất thông tin về quá trình Matching.

11. Trip

Booking và Trip nên tách riêng.

Booking = Customer yêu cầu
Trip    = Chuyến đi thực tế

Entity: Trip
Field	Type	Description
trip_id	UUID	PK
booking_id	UUID	FK Booking
driver_id	UUID	FK Driver
vehicle_id	UUID	FK Vehicle
status	Enum	Trip Status
started_at	DateTime	Bắt đầu
picked_up_at	DateTime	Đón khách
completed_at	DateTime	Hoàn thành
actual_distance	Decimal	Quãng đường
actual_duration	Integer	Thời gian
created_at	DateTime	Ngày tạo
12. Driver Location

Location có thể có lượng dữ liệu rất lớn nên cần thiết kế riêng.

Entity: DriverLocation
Field	Type	Description
location_id	UUID	PK
driver_id	UUID	FK Driver
latitude	Decimal	Latitude
longitude	Decimal	Longitude
accuracy	Decimal	Độ chính xác
recorded_at	DateTime	Thời gian ghi nhận

Relationship:

DRIVER 1 ───── N DRIVER_LOCATION

Lưu ý BA

Cần xác nhận:

Tần suất cập nhật GPS.
Thời gian lưu Location.
Có lưu lịch sử toàn bộ hay chỉ vị trí gần nhất.
Ai được phép xem Location.
13. Fare
Entity: Fare
Field	Type	Description
fare_id	UUID	PK
trip_id	UUID	FK Trip
service_type_id	UUID	FK ServiceType
base_fare	Decimal	Giá cơ bản
distance_fare	Decimal	Phí khoảng cách
time_fare	Decimal	Phí thời gian
surcharge	Decimal	Phụ phí
discount	Decimal	Giảm giá
total_amount	Decimal	Tổng tiền
currency	String	Currency
calculated_at	DateTime	Thời gian tính

Công thức tổng quát:

Total Amount
    =
Base Fare
+ Distance Fare
+ Time Fare
+ Surcharge
- Discount


Công thức thực tế cần được ABC xác nhận.

14. Payment
Entity: Payment
Field	Type	Description
payment_id	UUID	PK
trip_id	UUID	FK Trip
amount	Decimal	Số tiền
method	Enum	CASH / ELECTRONIC
status	Enum	PENDING / SUCCESS / FAILED
paid_at	DateTime	Thời gian thanh toán
created_at	DateTime	Ngày tạo
15. Payment Transaction

Tách Payment và Transaction giúp hỗ trợ retry.

Payment
   │
   ├── Transaction #1 → FAILED
   ├── Transaction #2 → FAILED
   └── Transaction #3 → SUCCESS

Entity: PaymentTransaction
Field	Type	Description
transaction_id	UUID	PK
payment_id	UUID	FK Payment
provider	String	Payment Provider
provider_transaction_id	String	ID từ Provider
amount	Decimal	Amount
status	Enum	INITIATED / SUCCESS / FAILED
failure_code	String	Error Code
created_at	DateTime	Thời gian tạo
completed_at	DateTime	Hoàn thành
Security

Không lưu:

Card Number
CVV
Full Bank Account Credential


CAB chỉ nên lưu thông tin cần thiết để reference transaction.

16. Rating
Entity: Rating
Field	Type	Description
rating_id	UUID	PK
trip_id	UUID	FK Trip
customer_id	UUID	FK Customer
driver_id	UUID	FK Driver
score	Integer	Điểm
comment	String	Nhận xét
created_at	DateTime	Ngày tạo

Business constraint:

Một Trip chỉ được Rating theo policy của doanh nghiệp.

17. Notification
Entity: Notification
Field	Type	Description
notification_id	UUID	PK
user_id	UUID	Người nhận
trip_id	UUID	FK Trip, nullable
type	String	Notification Type
channel	Enum	PUSH / SMS / EMAIL / ...
title	String	Tiêu đề
content	String	Nội dung
status	Enum	PENDING / SENT / FAILED
sent_at	DateTime	Thời gian gửi
created_at	DateTime	Ngày tạo

Thiết kế này cho phép mở rộng:

Notification
     │
     ├── PUSH
     ├── SMS
     ├── EMAIL
     └── Future Channel

18. Staff – Role – Permission
Staff
Field	Type
staff_id	UUID
user_id	UUID
role_id	UUID
status	Enum
Role
Field	Type
role_id	UUID
code	String
name	String
Permission
Field	Type
permission_id	UUID
code	String
name	String
Role Permission
Field	Type
role_id	UUID
permission_id	UUID

Relationship:

ROLE N ───── N PERMISSION
       │
       ↓
 ROLE_PERMISSION


Ví dụ:

OPERATION_STAFF
    ├── VIEW_TRIP
    ├── VIEW_DRIVER
    ├── VIEW_TRANSACTION
    └── HANDLE_EXCEPTION

ADMIN
    ├── All above
    ├── MANAGE_USER
    ├── MANAGE_ROLE
    └── MANAGE_PERMISSION

19. Audit Log
Entity: AuditLog
Field	Type	Description
audit_id	UUID	PK
actor_id	UUID	User thực hiện
action	String	Action
entity_type	String	Loại Entity
entity_id	UUID	Entity bị tác động
old_value	JSON	Giá trị cũ
new_value	JSON	Giá trị mới
result	Enum	SUCCESS / FAILED
ip_address	String	IP
created_at	DateTime	Timestamp
20. Data Model tổng thể
erDiagram

    USER {
        uuid user_id PK
        string username
        string email
        string phone
        string password_hash
        string status
        datetime created_at
        datetime updated_at
    }

    CUSTOMER {
        uuid customer_id PK
        uuid user_id FK
        string full_name
        string profile_image
    }

    DRIVER {
        uuid driver_id PK
        uuid user_id FK
        string license_number
        string driver_status
        decimal rating_avg
    }

    STAFF {
        uuid staff_id PK
        uuid user_id FK
        uuid role_id FK
        string status
    }

    ROLE {
        uuid role_id PK
        string code
        string name
    }

    PERMISSION {
        uuid permission_id PK
        string code
        string name
    }

    ROLE_PERMISSION {
        uuid role_id FK
        uuid permission_id FK
    }

    VEHICLE {
        uuid vehicle_id PK
        uuid driver_id FK
        uuid service_type_id FK
        string license_plate
        string brand
        string model
        string status
    }

    SERVICE_TYPE {
        uuid service_type_id PK
        string code
        string name
        string status
    }

    BOOKING {
        uuid booking_id PK
        uuid customer_id FK
        uuid service_type_id FK
        decimal pickup_lat
        decimal pickup_lng
        string pickup_address
        decimal destination_lat
        decimal destination_lng
        string destination_address
        string status
        datetime requested_at
    }

    DRIVER_ASSIGNMENT {
        uuid assignment_id PK
        uuid booking_id FK
        uuid driver_id FK
        int sequence_no
        decimal distance
        decimal score
        string status
        datetime offered_at
        datetime responded_at
    }

    TRIP {
        uuid trip_id PK
        uuid booking_id FK
        uuid driver_id FK
        uuid vehicle_id FK
        string status
        datetime started_at
        datetime picked_up_at
        datetime completed_at
        decimal actual_distance
        int actual_duration
    }

    DRIVER_LOCATION {
        uuid location_id PK
        uuid driver_id FK
        decimal latitude
        decimal longitude
        decimal accuracy
        datetime recorded_at
    }

    FARE {
        uuid fare_id PK
        uuid trip_id FK
        uuid service_type_id FK
        decimal base_fare
        decimal distance_fare
        decimal time_fare
        decimal surcharge
        decimal discount
        decimal total_amount
        string currency
    }

    PAYMENT {
        uuid payment_id PK
        uuid trip_id FK
        decimal amount
        string method
        string status
        datetime paid_at
    }

    PAYMENT_TRANSACTION {
        uuid transaction_id PK
        uuid payment_id FK
        string provider
        string provider_transaction_id
        decimal amount
        string status
        string failure_code
    }

    RATING {
        uuid rating_id PK
        uuid trip_id FK
        uuid customer_id FK
        uuid driver_id FK
        int score
        string comment
        datetime created_at
    }

    NOTIFICATION {
        uuid notification_id PK
        uuid user_id FK
        uuid trip_id FK
        string type
        string channel
        string status
        datetime sent_at
    }

    AUDIT_LOG {
        uuid audit_id PK
        uuid actor_id FK
        string action
        string entity_type
        uuid entity_id
        json old_value
        json new_value
        datetime created_at
    }

    USER ||--o| CUSTOMER : has
    USER ||--o| DRIVER : has
    USER ||--o| STAFF : has

    DRIVER ||--o{ VEHICLE : owns
    SERVICE_TYPE ||--o{ VEHICLE : supports

    CUSTOMER ||--o{ BOOKING : creates
    SERVICE_TYPE ||--o{ BOOKING : selected

    BOOKING ||--o| TRIP : generates
    BOOKING ||--o{ DRIVER_ASSIGNMENT : has
    DRIVER ||--o{ DRIVER_ASSIGNMENT : receives

    DRIVER ||--o{ DRIVER_LOCATION : sends

    DRIVER ||--o{ TRIP : performs
    VEHICLE ||--o{ TRIP : used_in

    TRIP ||--o| FARE : has
    TRIP ||--o| PAYMENT : has
    PAYMENT ||--o{ PAYMENT_TRANSACTION : contains

    TRIP ||--o| RATING : receives
    CUSTOMER ||--o{ RATING : gives
    DRIVER ||--o{ RATING : receives

    USER ||--o{ NOTIFICATION : receives
    TRIP ||--o{ NOTIFICATION : triggers

    ROLE ||--o{ STAFF : assigned
    ROLE ||--o{ ROLE_PERMISSION : contains
    PERMISSION ||--o{ ROLE_PERMISSION : grants

    USER ||--o{ AUDIT_LOG : performs

21. Core Data Flow
Customer
Booking
Driver Matching
Driver Assignment
Trip
Fare
Payment
Rating

Đây là core domain model của CAB.

22. Các Entity cần đặc biệt chú ý

Trong phạm vi 7 tuần, không nên xem tất cả entity có mức độ quan trọng giống nhau.

Core
Customer
Driver
Vehicle
Booking
DriverAssignment
Trip
Fare
Payment

Supporting
ServiceType
DriverLocation
Notification
Rating

Administration
Staff
Role
Permission
AuditLog

23. Một số quyết định Modeling quan trọng
23.1 Booking ≠ Trip

Không nên gộp hai entity.

Booking
= yêu cầu của Customer

Trip
= chuyến thực tế được thực hiện


Điều này giúp xử lý:

Booking bị cancel.
Booking không tìm được Driver.
Matching nhiều lần.
Trip được tạo sau khi Driver Accept.
23.2 DriverAssignment là entity riêng

Đây là quyết định quan trọng nhất cho Matching.

Booking
  │
  ├── Driver A → REJECTED
  ├── Driver B → TIMEOUT
  └── Driver C → ACCEPTED


Cho phép hệ thống phân tích:

Acceptance Rate.
Rejection Rate.
Matching Time.
Driver Performance.
23.3 Payment và PaymentTransaction tách riêng
Payment
   │
   ├── Transaction 1 → FAILED
   ├── Transaction 2 → FAILED
   └── Transaction 3 → SUCCESS


Hỗ trợ retry và audit tốt hơn.

23.4 ServiceType là master data

Không hard-code loại xe.

ServiceType
   ├── 4-SEAT
   ├── 7-SEAT
   ├── PREMIUM
   └── FUTURE SERVICE


Điều này đáp ứng requirement:

Có thể bổ sung loại dịch vụ mới mà không phải xây dựng lại toàn bộ ứng dụng.

24. Data Modeling Questions cần BA xác nhận

Trước khi chốt Logical Data Model, cần đưa các câu hỏi sau cho khách hàng/Architect:

#	Question	Impact
1	Customer có thể có nhiều số điện thoại không?	User
2	Driver có thể sở hữu nhiều Vehicle không?	Driver/Vehicle
3	Một Vehicle có thể được nhiều Driver sử dụng không?	Vehicle
4	Driver có thể chuyển Vehicle trong ngày không?	Trip
5	Location lưu bao lâu?	Location
6	Có lưu toàn bộ lịch sử GPS không?	Location/Storage
7	Một Booking có thể retry Matching bao nhiêu lần?	Assignment
8	Một Booking có thể có nhiều Trip không?	Booking/Trip
9	Fare có thể được điều chỉnh thủ công không?	Fare/Audit
10	Payment có hỗ trợ refund không?	Payment
11	Một Trip được retry payment bao nhiêu lần?	Transaction
12	Rating có được sửa/xóa không?	Rating
13	Notification có cần lưu lịch sử không?	Notification
14	Audit Log lưu trong bao lâu?	Audit
15	Transaction lưu trong bao lâu?	Payment
16	Có soft delete không?	Tất cả entity
17	Dữ liệu nào thuộc nhóm sensitive data?	Security
18	Có yêu cầu data residency không?	Architecture
25. Kết quả đầu ra của Data Modeling

Sau bước này, BA có thể chuyển sang các deliverable tiếp theo:

Business Requirement
        ↓
Business Rules
        ↓
Acceptance Criteria
        ↓
Data Modeling          ← ĐANG Ở ĐÂY
        ↓
Entity / Relationship
        ↓
Use Case Modeling
        ↓
Functional Requirement
        ↓
API / System Requirement
        ↓
Database Design


Lưu ý: Data Model ở giai đoạn BA nên dừng ở Conceptual/Logical Data Model. Chưa cần quyết định ngay PostgreSQL, MongoDB, Redis, partition, index cụ thể... vì đó là phần Physical Data Model / Solution Architecture do BA phối hợp với SA/Tech Lead xác định sau.




Bước 10: Xác định non-functional requirements
Non-Functional Requirements (NFR) – CAB System

Dựa trên yêu cầu của ABC và phạm vi hệ thống đã phân tích, NFR của CAB nên tập trung vào: Performance, Availability, Scalability, Security, Reliability, Maintainability, Observability, Usability, Compatibility và Compliance/Data Retention.

Lưu ý BA: Các con số như 99.9%, 2 giây, 1.000 request/second... chưa được khách hàng cung cấp. Vì vậy nên đánh dấu [TBD] và đưa vào danh sách cần xác nhận, thay vì tự coi đó là yêu cầu chính thức.

1. Tổng quan NFR
mindmap
  root((CAB NFR))
    Performance
      Response Time
      Throughput
      Driver Matching
      Peak Load

    Availability
      System Uptime
      Fault Isolation
      Graceful Degradation

    Scalability
      Horizontal Scaling
      Independent Scaling
      Peak Traffic

    Security
      Authentication
      Authorization
      Data Protection
      Payment Security

    Reliability
      Retry
      Idempotency
      Data Consistency
      Recovery

    Maintainability
      Modular Design
      Logging
      Monitoring
      Testing

    Observability
      Logging
      Metrics
      Tracing
      Alerting

    Usability
      Responsive UI
      Clear Error Message
      Accessibility

    Data
      Backup
      Retention
      Integrity

    Extensibility
      New Service
      Payment Provider
      Notification Channel

    Deployment
      Independent Deployment
      Rollback
      CI/CD

2. NFR – Performance
NFR-PERF-001 – Response Time

Requirement:

Hệ thống phải phản hồi các thao tác nghiệp vụ chính trong thời gian phù hợp với trải nghiệm người dùng.

Acceptance Criteria
Scenario: Customer tạo Booking
Given Customer đã nhập đầy đủ thông tin hợp lệ
When Customer gửi Booking
Then hệ thống phải phản hồi kết quả trong thời gian SLA được xác nhận


Target: [TBD] ms/seconds

NFR-PERF-002 – Booking Response

Các thao tác:

Login
View Booking
Create Booking
View Trip
View Payment
View History

phải có response time đáp ứng SLA.

Operation	Target
Login	[TBD]
Create Booking	[TBD]
View Trip	[TBD]
View History	[TBD]
Payment Status	[TBD]
NFR-PERF-003 – Driver Matching

Hệ thống phải bắt đầu quá trình Driver Matching ngay sau khi Booking được tạo thành công.

Booking Created
      ↓
Matching Started
      ↓
Candidate Search
      ↓
Driver Request


Các thông số cần xác định:

Matching initiation time.
Candidate search latency.
Driver response timeout.
Maximum retry duration.
3. NFR – Scalability

Đây là NFR rất quan trọng vì khách hàng yêu cầu hệ thống phục vụ số lượng lớn Customer và Driver.

NFR-SCALE-001 – Horizontal Scalability

Các component có tải cao phải có khả năng scale theo chiều ngang.

Ví dụ:

              Load Balancer
                    |
        +-----------+-----------+
        |           |           |
    Booking-1   Booking-2   Booking-3

NFR-SCALE-002 – Independent Scaling

Các component sau phải có khả năng scale độc lập:

Booking
Matching
Notification
Payment
Reporting


Ví dụ:

Khi Notification tăng tải, không cần scale toàn bộ Booking System.

NFR-SCALE-003 – Peak Load

Hệ thống phải hoạt động ổn định trong thời gian nhu cầu tăng cao.

Cần xác nhận:

Parameter	Value
Concurrent Customers	[TBD]
Concurrent Drivers	[TBD]
Booking/minute	[TBD]
Peak TPS	[TBD]
Peak Duration	[TBD]
4. NFR – Availability
NFR-AVL-001

CAB phải duy trì khả năng cung cấp dịch vụ trong thời gian vận hành.

Target:

Availability = [TBD]


Ví dụ mục tiêu thường được thảo luận trong SLA:

99.9%
99.95%
99.99%


Nhưng không được tự chọn nếu ABC chưa xác nhận.

NFR-AVL-002 – Fault Isolation

Một component bị lỗi không được làm toàn bộ CAB ngừng hoạt động.

Failure
Failure
Booking
Matching
Notification
Payment
Booking vẫn hoạt động
Trip vẫn tồn tại

Ví dụ:

Notification DOWN
        ↓
Booking vẫn hoạt động
        ↓
Trip vẫn hoạt động
        ↓
Notification retry sau

5. NFR – Reliability
NFR-REL-001 – Retry

Các operation có thể retry phải có retry policy.

Áp dụng cho:

Payment.
Notification.
External API.
Driver Matching.

Các thông số:

Maximum Retry = [TBD]
Retry Interval = [TBD]
Backoff Strategy = [TBD]

NFR-REL-002 – Idempotency

Các operation quan trọng không được tạo duplicate data khi request được gửi nhiều lần.

Ví dụ:

Customer
   ↓
Create Booking
   ↓
Network timeout
   ↓
Customer retry


Hệ thống không được tạo:

Booking A
Booking B


nếu cả hai request thực chất là cùng một request.

NFR-REL-003 – Payment Idempotency

Đặc biệt quan trọng.

Payment Request
      ↓
Timeout
      ↓
Retry


Không được dẫn đến:

Charge #1 = SUCCESS
Charge #2 = SUCCESS


cho cùng một payment intent nếu policy không cho phép.

6. NFR – Data Consistency
NFR-DATA-001

Dữ liệu nghiệp vụ quan trọng phải đảm bảo consistency.

Đặc biệt:

Booking
Trip
Fare
Payment
Driver Assignment


Ví dụ:

Booking = DRIVER_ASSIGNED


thì không thể đồng thời có:

Trip = NO_DRIVER_FOUND


nếu không có business rule đặc biệt.

7. NFR – Security
NFR-SEC-001 – Authentication

Customer, Driver và Staff phải được authentication trước khi truy cập chức năng protected.

NFR-SEC-002 – Authorization

Hệ thống phải kiểm soát quyền dựa trên role/permission.

Customer
   ↓
Customer permissions

Driver
   ↓
Driver permissions

Operation
   ↓
Operation permissions

Administrator
   ↓
Administrative permissions

NFR-SEC-003 – Data Encryption

Thông tin nhạy cảm phải được bảo vệ:

Personal Data
Vehicle Data
Location Data
Transaction Data
Authentication Data


Cần xác định:

Encryption in transit.
Encryption at rest.
Key management.
NFR-SEC-004 – Payment Data

CAB không được lưu trực tiếp dữ liệu nhạy cảm của thẻ/tài khoản thanh toán nếu không cần thiết.

Payment data nên được xử lý thông qua Payment Provider.

Customer
CAB
Payment Provider
NFR-SEC-005 – Audit

Các thao tác nhạy cảm phải được audit:

Login.
Permission change.
Driver status change.
Trip intervention.
Payment intervention.
Admin action.
8. NFR – Privacy
NFR-PRIV-001

Chỉ những Actor có quyền mới được truy cập dữ liệu cá nhân.

NFR-PRIV-002

Driver Location phải được kiểm soát quyền truy cập.

NFR-PRIV-003

Dữ liệu cá nhân phải có chính sách retention và deletion.

Các giá trị cần xác nhận:

Personal Data Retention = [TBD]
Location Retention = [TBD]
Payment Data Retention = [TBD]
Audit Retention = [TBD]

9. NFR – Backup & Recovery
NFR-REC-001 – Backup

Dữ liệu quan trọng phải được backup.

Bao gồm:

Customer
Driver
Booking
Trip
Fare
Payment
Audit

NFR-REC-002 – Recovery Point Objective

Xác định lượng dữ liệu tối đa được phép mất khi xảy ra sự cố.

RPO = [TBD]

NFR-REC-003 – Recovery Time Objective

Xác định thời gian tối đa để khôi phục dịch vụ.

RTO = [TBD]

10. NFR – Availability & Disaster Recovery

Cần có chiến lược xử lý khi:

Database Failure
Payment Provider Failure
Notification Provider Failure
Network Failure
Application Failure
Infrastructure Failure


Acceptance Criteria:

Scenario: Payment Provider unavailable
Given CAB đang hoạt động
And Payment Provider bị unavailable
When Customer thực hiện payment
Then Booking và Trip không bị mất
And Payment được ghi nhận trạng thái phù hợp
And hệ thống có thể retry/recover theo policy

11. NFR – Maintainability
NFR-MAIN-001

Hệ thống phải được thiết kế theo các module/component có trách nhiệm rõ ràng.

Các domain chính:

Identity
Booking
Matching
Trip
Location
Fare
Payment
Notification
Rating
Operation
Reporting

NFR-MAIN-002 – Loose Coupling

Các module tích hợp bên ngoài không nên phụ thuộc chặt vào implementation cụ thể.

Ví dụ:

        Payment Interface
               |
      +--------+--------+
      |                 |
 Provider A         Provider B


Tương tự:

Notification Interface
       |
 +-----+-----+------+
 |           |      |
Push        SMS    Email


Điều này đáp ứng requirement:

Có thể thay đổi Payment Provider hoặc Notification Provider mà không phải xây dựng lại toàn bộ hệ thống.

12. NFR – Extensibility
NFR-EXT-001

Hệ thống phải cho phép bổ sung Service Type mới.

Current:
Car 4 seats
Car 7 seats

Future:
Premium
Motorbike
Airport Transfer
...


Không nên yêu cầu thay đổi toàn bộ core booking flow.

NFR-EXT-002

Hệ thống phải hỗ trợ thêm Payment Provider.

Payment
   ↓
Payment Interface
   ├── Provider A
   ├── Provider B
   └── Provider C

NFR-EXT-003

Hệ thống phải hỗ trợ thêm Notification Channel.

Notification
   ├── Push
   ├── SMS
   ├── Email
   └── Future Channel

13. NFR – Observability

Đây là requirement cần thiết để Operation có thể xử lý sự cố.

NFR-OBS-001 – Logging

Hệ thống phải ghi log cho các event quan trọng:

Login
Booking Created
Driver Assigned
Trip Status Changed
Payment
Notification
System Error

NFR-OBS-002 – Monitoring

Phải có monitoring cho:

API response time.
Error rate.
Booking volume.
Matching success rate.
Payment failure rate.
Notification failure rate.
Driver online/offline.
System resource usage.
NFR-OBS-003 – Alerting

Hệ thống phải cảnh báo khi các metric vượt ngưỡng.

Ví dụ:

Payment Failure Rate > [TBD]
       ↓
     Alert
       ↓
Operation / Technical Team

14. NFR – Usability
NFR-USE-001

Customer phải có thể thực hiện Booking bằng quy trình đơn giản và dễ hiểu.

Core flow:

Pickup
  ↓
Destination
  ↓
Service Type
  ↓
Confirm
  ↓
Matching

NFR-USE-002 – Error Message

Thông báo lỗi phải:

Dễ hiểu.
Nêu được vấn đề.
Hướng dẫn hành động tiếp theo nếu có thể.

Không nên:

Error 500


Nên:

Không thể tìm thấy tài xế phù hợp.
Vui lòng thử lại sau.

15. NFR – Compatibility

Hệ thống cần xác định các platform được hỗ trợ:

Platform	Requirement
Customer App	[TBD]
Driver App	[TBD]
Operation Web	[TBD]
iOS	[TBD]
Android	[TBD]
Browser	[TBD]

Cần xác định:

OS version.
Browser version.
Mobile device support.
API compatibility.
16. NFR – Deployment
NFR-DEP-001

Các chức năng mới phải có khả năng triển khai từng phần với ảnh hưởng tối thiểu đến hệ thống đang hoạt động.

Ví dụ:

Version 1
   ↓
Booking + Matching
   ↓
Deploy

Version 2
   ↓
Payment
   ↓
Deploy

Version 3
   ↓
Rating
   ↓
Deploy

NFR-DEP-002 – Rollback

Khi deployment gây lỗi nghiêm trọng:

New Version
     ↓
Error
     ↓
Health Check Failed
     ↓
Rollback
     ↓
Previous Stable Version


Thời gian rollback:

Rollback Time = [TBD]

17. NFR – API

Nếu CAB cung cấp API cho Mobile App/Admin:

NFR-API-001

API phải:

Có authentication.
Có authorization.
Validate input.
Có consistent error response.
Có versioning.

Ví dụ:

/api/v1/bookings
/api/v1/trips
/api/v1/drivers
/api/v1/payments

18. NFR Matrix tổng hợp
ID	Category	Requirement	Priority
NFR-PERF-001	Performance	Response Time	Must
NFR-PERF-002	Performance	API Performance	Must
NFR-PERF-003	Performance	Matching Latency	Critical
NFR-SCALE-001	Scalability	Horizontal Scaling	Critical
NFR-SCALE-002	Scalability	Independent Scaling	Critical
NFR-SCALE-003	Scalability	Peak Load	Critical
NFR-AVL-001	Availability	System Availability	Critical
NFR-AVL-002	Availability	Fault Isolation	Critical
NFR-REL-001	Reliability	Retry	Must
NFR-REL-002	Reliability	Idempotency	Critical
NFR-REL-003	Reliability	Payment Idempotency	Critical
NFR-DATA-001	Data	Consistency	Critical
NFR-SEC-001	Security	Authentication	Critical
NFR-SEC-002	Security	Authorization	Critical
NFR-SEC-003	Security	Encryption	Critical
NFR-SEC-004	Security	Payment Data Protection	Critical
NFR-SEC-005	Security	Audit	Must
NFR-PRIV-001	Privacy	Personal Data Protection	Critical
NFR-REC-001	Recovery	Backup	Must
NFR-REC-002	Recovery	RPO	Must
NFR-REC-003	Recovery	RTO	Must
NFR-MAIN-001	Maintainability	Modular Architecture	Must
NFR-MAIN-002	Maintainability	Loose Coupling	Must
NFR-EXT-001	Extensibility	New Service	Must
NFR-EXT-002	Extensibility	New Payment Provider	Must
NFR-EXT-003	Extensibility	New Notification Channel	Must
NFR-OBS-001	Observability	Logging	Must
NFR-OBS-002	Observability	Monitoring	Must
NFR-OBS-003	Observability	Alerting	Must
NFR-USE-001	Usability	Booking Usability	Should
NFR-USE-002	Usability	Error Message	Should
NFR-DEP-001	Deployment	Independent Deployment	Must
NFR-DEP-002	Deployment	Rollback	Must
19. NFR cần xác nhận với khách hàng

Đây là phần không nên bỏ qua trong BA. Từ đề bài hiện tại, ABC mới mô tả mong muốn ở mức business, chưa có các SLA/KPI kỹ thuật cụ thể.

#	Question	Giá trị cần xác nhận
1	System Availability?	[TBD]%
2	Concurrent Customers?	[TBD]
3	Concurrent Drivers?	[TBD]
4	Peak Booking/min?	[TBD]
5	API Response Time?	[TBD] ms
6	Driver Matching Time?	[TBD] sec
7	Driver Response Timeout?	[TBD] sec
8	Maximum Retry?	[TBD]
9	RPO?	[TBD]
10	RTO?	[TBD]
11	Data Retention?	[TBD]
12	Location Retention?	[TBD]
13	Audit Retention?	[TBD]
14	Supported OS?	[TBD]
15	Supported Browser?	[TBD]
16	Security/Compliance Standard?	[TBD]
17	Maintenance Window?	[TBD]
18	Deployment Downtime?	[TBD]
20. Traceability với Business Goal
Business Goal
Performance
Scalability
Availability
Security
Reliability
Extensibility
Operation
Fast Booking / Matching
Handle Peak Load
System Always Available
Protect Customer / Driver / Payment Data
Payment / Notification Failure Isolation
Add Service / Payment / Notification
Monitoring / Logging / Reporting
21. Ưu tiên NFR cho MVP 7 tuần

Vì thời gian triển khai chỉ 7 tuần, nên không nên cố triển khai tất cả NFR ở cùng mức độ.

P0 – Bắt buộc
Authentication / Authorization
Booking Performance
Driver Matching Performance
Payment Security
Data Consistency
Fault Isolation
Basic Availability
Logging
Monitoring
Backup

P1 – Nên có
Horizontal Scaling
Retry
Idempotency
Audit
Alerting
Independent Deployment
Rollback
Extensible Payment/Notification

P2 – Có thể phát triển sau MVP
Advanced Analytics
Advanced Observability
Advanced Auto Scaling
Advanced Disaster Recovery
Advanced Reporting

22. NFR tổng hợp cho CAB

Có thể đưa đoạn này vào SRS/BRD dưới dạng summary:

## Non-Functional Requirements

CAB System phải đáp ứng các yêu cầu phi chức năng sau:

1. Performance
   - Đáp ứng SLA về response time.
   - Driver Matching phải bắt đầu và xử lý trong SLA được xác định.

2. Scalability
   - Có khả năng phục vụ lượng lớn Customer và Driver.
   - Các component có thể scale độc lập.
   - Hệ thống phải chịu được peak load.

3. Availability
   - Hệ thống phải duy trì khả năng phục vụ theo SLA.
   - Failure của Payment/Notification không được làm dừng Booking/Trip.

4. Reliability
   - Hỗ trợ retry đối với operation phù hợp.
   - Đảm bảo idempotency cho các transaction quan trọng.
   - Đảm bảo consistency của dữ liệu Booking, Trip và Payment.

5. Security
   - Authentication và Authorization.
   - RBAC cho Operation.
   - Bảo vệ Personal Data, Location Data và Transaction Data.
   - Không lưu trực tiếp sensitive payment data.

6. Audit
   - Lưu vết các thao tác quan trọng.

7. Maintainability
   - Thiết kế module/component có coupling thấp.
   - Có logging, monitoring và error handling.

8. Extensibility
   - Hỗ trợ thêm Service Type.
   - Hỗ trợ thêm Payment Provider.
   - Hỗ trợ thêm Notification Channel.

9. Recovery
   - Có Backup.
   - Có RPO/RTO theo SLA.
   - Có khả năng phục hồi khi xảy ra sự cố.

10. Deployment
    - Hỗ trợ triển khai từng phần.
    - Hạn chế downtime.
    - Có khả năng rollback khi deployment thất bại.


Điểm BA cần nhấn mạnh: ở thời điểm hiện tại, phần NFR của CAB mới xác định được "hệ thống phải tốt như thế nào" ở cấp nguyên tắc. Các giá trị định lượng như response time, TPS, concurrent users, availability, RPO/RTO, retention period và SLA phải được đưa thành NFR Acceptance Criteria sau khi BA tổ chức workshop với Business Owner, Operation và Technical/Infrastructure team.

Bước 11: Xác định, vẽ use case
flowchart LR

    C["👤 Customer"]
    D["🚗 Driver"]
    O["👨‍💼 Operation Staff"]
    A["🔐 Admin"]
    PP["💳 Payment Provider"]
    NP["🔔 Notification Provider"]
    MAP["🗺️ Map / Location Service"]

    subgraph CAB["CAB SYSTEM"]

        subgraph CUSTOMER["Customer Management"]
            C1(("Register / Login"))
            C2(("Manage Profile"))
            C3(("Book Vehicle"))
            C4(("Track Trip"))
            C5(("View Trip History"))
            C6(("Cancel Booking"))
            C7(("Make Payment"))
            C8(("Rate Driver"))
        end

        subgraph DRIVER["Driver Operations"]
            D1(("Manage Driver Profile"))
            D2(("Manage Vehicle"))
            D3(("Set Availability"))
            D4(("Receive Trip Request"))
            D5(("Accept / Reject Trip"))
            D6(("Update Trip Status"))
            D7(("Send Location"))
            D8(("View Trip History"))
        end

        subgraph MATCHING["Booking & Matching"]
            M1(("Driver Matching"))
            M2(("Rank Candidates"))
            M3(("Assign Driver"))
        end

        subgraph TRIP["Trip Management"]
            T1(("Calculate Fare"))
            T2(("Manage Trip"))
            T3(("Handle Trip Exception"))
        end

        subgraph PAYMENT["Payment"]
            P1(("Process Payment"))
            P2(("Retry Payment"))
        end

        subgraph NOTIFICATION["Notification"]
            N1(("Send Notification"))
        end

        subgraph OPERATION["Operation"]
            O1(("Manage Customer"))
            O2(("Manage Driver"))
            O3(("Manage Vehicle"))
            O4(("Monitor Active Trips"))
            O5(("View Transaction"))
            O6(("View Reports"))
        end

        subgraph ADMIN["Administration"]
            A1(("Manage Staff"))
            A2(("Manage Role / Permission"))
            A3(("View Audit Log"))
        end
    end

    C --> C1
    C --> C2
    C --> C3
    C --> C4
    C --> C5
    C --> C6
    C --> C7
    C --> C8

    D --> D1
    D --> D2
    D --> D3
    D --> D4
    D --> D5
    D --> D6
    D --> D7
    D --> D8

    O --> O1
    O --> O2
    O --> O3
    O --> O4
    O --> O5
    O --> O6

    A --> A1
    A --> A2
    A --> A3

    C3 --> M1
    M1 --> M2
    M2 --> M3
    D4 --> D5
    D5 --> M1

    D6 --> T2
    T2 --> T1

    C7 --> P1
    P1 --> P2

    C3 --> N1
    D4 --> N1
    D6 --> N1
    P1 --> N1

    P1 --> PP
    N1 --> NP
    M1 --> MAP
    T1 --> MAP



Bước 12: Đặc tả use case
Đặc tả Use Case – CAB System

Dựa trên Use Case Diagram ở trên, nên đặc tả trước các Use Case core của hệ thống CAB. Format dưới đây có thể copy trực tiếp vào Markdown/BRD/SRS.

1. UC-01 – Đăng ký tài khoản
Thuộc tính	Nội dung
Use Case ID	UC-01
Tên	Đăng ký tài khoản
Actor	Customer
Mục tiêu	Tạo tài khoản để sử dụng dịch vụ CAB
Priority	Must Have
Trigger	Customer chọn chức năng Đăng ký
Preconditions
Customer chưa có tài khoản.
Hệ thống đang hoạt động.
Customer có thông tin đăng ký hợp lệ.
Main Flow
Customer chọn Đăng ký.
Hệ thống hiển thị form đăng ký.
Customer nhập thông tin cá nhân.
Customer gửi thông tin.
Hệ thống validate dữ liệu.
Hệ thống kiểm tra tài khoản đã tồn tại hay chưa.
Hệ thống tạo Customer Account.
Hệ thống thông báo đăng ký thành công.
Alternative / Exception Flow
Code	Trường hợp	Xử lý
E01	Email/SĐT đã tồn tại	Thông báo tài khoản đã tồn tại
E02	Dữ liệu không hợp lệ	Hiển thị lỗi tương ứng
E03	Xác thực OTP thất bại	Cho phép xác thực lại theo policy
E04	System error	Không tạo duplicate account và thông báo lỗi
Postconditions
Customer Account được tạo thành công.
Customer có thể Login.
2. UC-02 – Đăng nhập
Thuộc tính	Nội dung
Use Case ID	UC-02
Tên	Đăng nhập
Actor	Customer / Driver / Staff
Priority	Must Have
Preconditions
User đã có tài khoản.
Account đang ở trạng thái cho phép đăng nhập.
Main Flow
User nhập username/phone/email.
User nhập password.
Hệ thống xác thực thông tin.
Hệ thống kiểm tra trạng thái account.
Hệ thống tạo authentication session/token.
Hệ thống cho phép truy cập chức năng tương ứng role.
Exception
E01 – Sai credential
→ Thông báo đăng nhập thất bại.

E02 – Account bị khóa
→ Từ chối đăng nhập.

E03 – Account inactive
→ Từ chối đăng nhập.

E04 – Authentication service lỗi
→ Thông báo system error.

Postconditions
User authenticated
        ↓
Role identified
        ↓
Access granted

3. UC-03 – Đặt xe

Đây là Use Case nghiệp vụ quan trọng nhất.

Thuộc tính	Nội dung
Use Case ID	UC-03
Tên	Đặt xe
Actor	Customer
Supporting Actor	Map/Location Service, Notification Service
Priority	Critical
Trigger	Customer muốn đặt một chuyến xe
Preconditions
Customer đã đăng nhập.
Customer account đang hoạt động.
Location hợp lệ.
Có ít nhất một Service Type đang active.
Main Flow
Customer chọn chức năng Đặt xe.
Hệ thống yêu cầu nhập điểm đón.
Customer nhập điểm đón.
Hệ thống yêu cầu nhập điểm đến.
Customer nhập điểm đến.
Hệ thống xác định thông tin địa điểm.
Customer chọn loại dịch vụ/loại xe.
Hệ thống hiển thị thông tin Booking.
Customer xác nhận đặt xe.
Hệ thống validate Booking.
Hệ thống tạo Booking.
Hệ thống chuyển Booking sang trạng thái SEARCHING_DRIVER.
Hệ thống bắt đầu Driver Matching.
Hệ thống gửi thông báo Booking đã được tiếp nhận.
Hệ thống tiếp tục xử lý theo UC-04 – Driver Matching.
Alternative Flow
A01 – Location không hợp lệ
6a. Map Service không xác định được location.
→ Hệ thống yêu cầu Customer nhập/chọn lại location.

A02 – Customer hủy trước khi confirm
9a. Customer chọn Cancel.
→ Booking không được tạo.
→ Kết thúc Use Case.

Exception Flow
E01 – System không thể tạo Booking
→ Không tạo duplicate Booking.
→ Hiển thị lỗi.

E02 – Map Service unavailable
→ Không thể xác nhận location.
→ Thông báo Customer.

E03 – Không có Service Type khả dụng
→ Thông báo Customer.

Postconditions

Success:

Booking = SEARCHING_DRIVER


Failure:

Booking không được tạo

4. UC-04 – Tìm và phân công tài xế
Thuộc tính	Nội dung
Use Case ID	UC-04
Tên	Driver Matching
Actor chính	CAB System
Supporting Actor	Driver, Map Service, Notification Service
Priority	Critical
Trigger	Booking ở trạng thái SEARCHING_DRIVER
Preconditions
Booking tồn tại.
Booking hợp lệ.
Booking chưa bị cancel.
Có thông tin Pickup Location.
Main Flow
Hệ thống nhận Booking cần tìm Driver.
Hệ thống tìm các Driver đang AVAILABLE.
Hệ thống lọc Driver theo các tiêu chí vận hành.
Hệ thống xác định khoảng cách giữa Driver và Pickup.
Hệ thống xếp hạng Candidate Driver.
Hệ thống chọn Driver phù hợp nhất.
Hệ thống gửi Trip Request cho Driver.
Hệ thống chờ phản hồi.
Driver chấp nhận chuyến.
Hệ thống assign Driver cho Booking.
Booking chuyển sang DRIVER_ASSIGNED.
Hệ thống thông báo Customer.
Hệ thống thông báo Driver.
Kết thúc Matching.
Alternative Flow
A01 – Driver từ chối
9a. Driver Reject.
→ Driver được đánh dấu không nhận Booking này.
→ Hệ thống quay lại bước 5.
→ Chọn Candidate tiếp theo.

A02 – Driver không phản hồi
8a. Timeout.
→ Hệ thống coi request là expired.
→ Chọn Driver khác.

A03 – Không tìm thấy Driver
5a. Không có Candidate phù hợp.
→ Booking = NO_DRIVER_FOUND.
→ Thông báo Customer.
→ Kết thúc Matching.

Business Rules
BR-01:
Chỉ Driver AVAILABLE mới được tham gia Matching.

BR-02:
Driver đã được assign không được đồng thời nhận Booking khác.

BR-03:
Driver Reject/Timeout không được yêu cầu Customer tạo lại Booking.

BR-04:
Hệ thống phải tiếp tục tìm Driver khác.

BR-05:
Tiêu chí Ranking Driver phải được xác nhận với Business.

Postconditions

Success:

Booking = DRIVER_ASSIGNED
Driver = ASSIGNED/BUSY


Failure:

Booking = NO_DRIVER_FOUND

5. UC-05 – Nhận và phản hồi yêu cầu chuyến
Thuộc tính	Nội dung
Use Case ID	UC-05
Tên	Accept / Reject Trip
Actor	Driver
Priority	Critical
Preconditions
Driver đã Login.
Driver đang AVAILABLE.
Driver nhận được Trip Request.
Main Flow – Accept
Driver nhận notification.
Driver mở Trip Request.
Hệ thống hiển thị thông tin chuyến.
Driver chọn Accept.
Hệ thống kiểm tra Booking còn available.
Hệ thống assign Driver.
Driver chuyển sang BUSY.
Customer nhận notification.
Kết thúc Use Case.
Alternative – Reject
Driver chọn Reject.
Hệ thống ghi nhận Driver reject.
Driver trở lại trạng thái AVAILABLE.
Hệ thống kích hoạt Matching tiếp tục.
Tìm Driver tiếp theo.
Exception
E01 – Booking đã được Driver khác nhận
→ Hiển thị Booking không còn khả dụng.

E02 – Driver không còn AVAILABLE
→ Không cho Accept.

E03 – Network failure
→ Không được tạo duplicate assignment.

6. UC-06 – Theo dõi chuyến đi
Thuộc tính	Nội dung
Use Case ID	UC-06
Tên	Track Trip
Actor	Customer
Supporting Actor	Driver, Location Service
Priority	Critical
Main Flow
Customer mở Booking.
Hệ thống lấy Trip Status.
Hệ thống lấy Driver Information.
Hệ thống lấy Driver Location.
Hệ thống lấy ETA.
Hệ thống hiển thị thông tin cho Customer.
Hệ thống cập nhật khi Trip Status/Location thay đổi.
Customer có thể thấy
Driver Name
Vehicle
Vehicle Plate
Driver Location
ETA
Trip Status

Exception
E01 – Driver mất kết nối
→ Hiển thị Last Known Location.

E02 – Location Service unavailable
→ Không hiển thị location mới.
→ Vẫn hiển thị Trip Status.

E03 – Trip không tồn tại
→ Thông báo lỗi.

7. UC-07 – Cập nhật trạng thái chuyến
Thuộc tính	Nội dung
Use Case ID	UC-07
Tên	Update Trip Status
Actor	Driver
Priority	Critical
State Flow
stateDiagram-v2
    [*] --> ASSIGNED
    ASSIGNED --> ARRIVED
    ARRIVED --> PICKED_UP
    PICKED_UP --> IN_PROGRESS
    IN_PROGRESS --> COMPLETED

    ASSIGNED --> CANCELLED
    ARRIVED --> CANCELLED
    PICKED_UP --> CANCELLED

Main Flow
Driver mở Trip.
Driver chọn trạng thái mới.
Hệ thống kiểm tra transition hợp lệ.
Hệ thống cập nhật Trip.
Hệ thống ghi Audit Log.
Hệ thống gửi Notification.
Customer nhận trạng thái mới.
Business Rules
BR-01:
Chỉ được chuyển trạng thái theo transition hợp lệ.

BR-02:
Trip COMPLETED không được quay lại IN_PROGRESS.

BR-03:
Mọi thay đổi trạng thái phải được audit.

BR-04:
Sau khi COMPLETED, hệ thống kích hoạt Calculate Fare.

8. UC-08 – Tính cước
Thuộc tính	Nội dung
Use Case ID	UC-08
Tên	Calculate Fare
Actor	CAB System
Priority	Critical
Preconditions
Trip đã COMPLETED.
Có thông tin Trip hợp lệ.
Service Type tồn tại.
Main Flow
Hệ thống nhận sự kiện Trip Completed.
Xác định Service Type.
Lấy thông tin quãng đường/thời gian.
Áp dụng Fare Rule.
Tính tổng tiền.
Lưu Fare.
Hiển thị số tiền Customer phải trả.
Kích hoạt Payment.
Công thức tổng quát
Fare
= Base Fare
+ Distance Charge
+ Time Charge
+ Additional Fee
- Discount


Công thức thực tế cần được Business xác nhận vì đề bài chưa chốt cách tính cước.

Exception
E01 – Không xác định được Distance
→ Chuyển sang exception handling.

E02 – Fare Rule không tồn tại
→ Không tự động hoàn tất Payment.
→ Ghi nhận lỗi để Operation xử lý.

9. UC-09 – Thanh toán
Thuộc tính	Nội dung
Use Case ID	UC-09
Tên	Make Payment
Actor	Customer
Supporting Actor	Payment Provider
Priority	Critical
Main Flow – Electronic Payment
Trip hoàn thành.
Hệ thống hiển thị Fare.
Customer chọn phương thức thanh toán điện tử.
CAB tạo Payment Request.
CAB gửi request đến Payment Provider.
Payment Provider xử lý.
Payment Provider trả kết quả.
CAB cập nhật Payment Status.
CAB thông báo Customer.
Payment hoàn tất.
Alternative – Cash
3a. Customer chọn Cash.
→ Hệ thống ghi nhận Payment Method = CASH.
→ Driver/Operation xử lý thu tiền theo policy.

Exception
E01 – Payment Failed
→ Payment = FAILED.
→ Thông báo Customer.
→ Cho phép Retry theo policy.

E02 – Payment Provider timeout
→ Payment = PENDING/UNKNOWN.
→ Không tạo transaction duplicate.

E03 – Payment Provider unavailable
→ Không làm Trip/Booking bị mất.

Security Rule
CAB không lưu trực tiếp thông tin nhạy cảm
của thẻ/tài khoản thanh toán.

10. UC-10 – Gửi thông báo
Thuộc tính	Nội dung
Use Case ID	UC-10
Tên	Send Notification
Actor	CAB System
Supporting Actor	Notification Provider
Priority	Critical
Trigger

Các event:

Booking Created
Driver Assigned
Driver Arrived
Trip Completed
Payment Success
Payment Failed
Trip Cancelled
New Trip Request

Main Flow
CAB phát sinh Business Event.
Notification Service nhận Event.
Xác định Recipient.
Xác định Notification Template.
Chọn Channel.
Gửi Notification Provider.
Provider trả Delivery Result.
Hệ thống lưu Notification Status.
Exception
E01 – Provider lỗi
→ Retry theo policy.

E02 – Notification không gửi được
→ Ghi log.
→ Không làm Booking/Trip fail.

E03 – Channel unavailable
→ Có thể chuyển sang channel khác nếu policy cho phép.

11. UC-11 – Đánh giá tài xế
Thuộc tính	Nội dung
Use Case ID	UC-11
Tên	Rate Driver
Actor	Customer
Priority	Should
Preconditions
Trip.status = COMPLETED
Customer là chủ Trip
Customer chưa đánh giá Trip

Main Flow
Customer mở Trip History.
Chọn Trip đã hoàn thành.
Chọn Rate Driver.
Nhập Rating.
Nhập Comment nếu có.
Submit.
Hệ thống validate.
Lưu Rating.
Cập nhật Driver Rating.
Business Rules
BR-01:
Mỗi Trip chỉ được Rating một lần.

BR-02:
Chỉ Customer của Trip mới được Rating.

BR-03:
Rating chỉ được thực hiện sau khi Trip COMPLETED.

12. UC-12 – Operation xử lý Trip Exception
Thuộc tính	Nội dung
Use Case ID	UC-12
Tên	Handle Trip Exception
Actor	Operation Staff
Priority	Must
Main Flow
Staff mở danh sách Active Trip.
Chọn Trip cần xử lý.
Xem thông tin Trip.
Xác định nguyên nhân.
Thực hiện action được phép.
Hệ thống cập nhật Trip.
Hệ thống ghi Audit Log.
Hệ thống thông báo các bên liên quan.
Exception Cases
Driver không đến
Driver mất kết nối
Customer không liên lạc được
Trip bị stuck
Payment Failed
Incorrect Trip Status
Driver/Customer dispute

13. UC-13 – Quản lý Driver
Thuộc tính	Nội dung
Use Case ID	UC-13
Tên	Manage Driver
Actor	Operation Staff
Priority	Must
Chức năng
Create Driver
View Driver
Update Driver
Activate Driver
Deactivate Driver
View Driver Status
View Driver Trip History

Business Rules
Driver phải có hồ sơ hợp lệ trước khi được AVAILABLE.

Driver phải có Vehicle hợp lệ.

Driver bị inactive không được nhận Booking.

14. UC-14 – Quản lý Customer
Thuộc tính	Nội dung
Use Case ID	UC-14
Tên	Manage Customer
Actor	Operation Staff
Priority	Must
Chức năng
Search Customer
View Customer
Update Customer
Activate/Deactivate Account
View Trip History
View Payment History


Các thao tác nhạy cảm phải được phân quyền và audit.

15. UC-15 – Báo cáo vận hành
Thuộc tính	Nội dung
Use Case ID	UC-15
Tên	View Reports
Actor	Operation Staff / Admin
Priority	Should
Báo cáo
Total Trips
Revenue
Completed Trip Rate
Cancellation Rate
Driver Performance
Payment Success Rate
Matching Success Rate

Filter
Date
Service Type
Driver
Area
Trip Status
Payment Status

16. Ma trận Actor – Use Case
Customer
Driver
Operation Staff
Admin
Payment Provider
Notification Provider
Map Service
Register / Login
Manage Profile
Book Vehicle
Track Trip
Cancel Trip
Payment
Rate Driver
Trip History
Driver Profile
Vehicle
Availability
Accept / Reject Trip
Update Trip
Driver Location
Manage Customer
Manage Driver
Manage Vehicle
Monitor Trip
Handle Exception
View Transaction
Reports
Manage Staff
Role / Permission
Audit
Notification
Location / ETA
17. Traceability Use Case → Business Requirement
Business Requirement	Use Case
Customer đăng ký	UC-01
Customer đăng nhập	UC-02
Customer đặt xe	UC-03
Tìm Driver	UC-04
Driver nhận/từ chối	UC-05
Theo dõi chuyến	UC-06
Cập nhật trạng thái	UC-07
Tính cước	UC-08
Thanh toán	UC-09
Notification	UC-10
Đánh giá	UC-11
Xử lý sự cố	UC-12
Quản lý Driver	UC-13
Quản lý Customer	UC-14
Báo cáo	UC-15
18. Các điểm TBD cần xác nhận trong Use Case

Khi đặc tả Use Case, không nên tự giả định những nghiệp vụ khách hàng chưa chốt. Cần đưa vào TBD:

1. Driver response timeout là bao lâu?
2. Matching retry tối đa bao nhiêu lần?
3. Tiêu chí ranking Driver?
4. Phạm vi bán kính tìm Driver?
5. Khi nào Booking tự động expire?
6. Chính sách Customer Cancel?
7. Chính sách Driver Cancel?
8. Fare được tính theo distance, time hay cả hai?
9. Surge pricing có áp dụng không?
10. Payment retry bao nhiêu lần?
11. Payment timeout xử lý thế nào?
12. Cash payment do Driver hay Operation xác nhận?
13. Driver Location update frequency?
14. Khi mất mạng thì Trip Status xử lý thế nào?
15. Rating range là 1–5 hay giá trị khác?
16. Có cho sửa/xóa Rating không?
17. Thời gian lưu Trip/Location/Payment/Audit?
18. Staff role nào được phép can thiệp Trip?

Core Use Case Flow

Toàn bộ nghiệp vụ cốt lõi của CAB có thể tóm tắt thành:

Yes
No / Timeout
Customer Login
Book Vehicle
Driver Matching
Driver Accept?
Assign Driver
Continue Matching
Track Trip
Driver Update Status
Trip Completed
Calculate Fare
Payment
Payment Result
Rate Driver

Đây nên được xem là baseline Use Case Model của CAB MVP. Từ đây có thể chuyển trực tiếp sang Sequence Diagram cho 3 luồng quan trọng nhất: Book Vehicle → Driver Matching, Trip Execution, và Payment, đồng thời viết Acceptance Criteria dạng Given/When/Then cho từng Use Case.




Bước 13: Acceptance Criteria
Acceptance Criteria – CAB System

Dưới đây là bộ Acceptance Criteria (AC) cho các Use Case chính của CAB System. Có thể copy trực tiếp vào BRD/SRS/Backlog/Jira.

Quy ước: Given = điều kiện đầu vào, When = hành động, Then = kết quả mong đợi.

1. UC-01 – Đăng ký tài khoản
AC-01.01 – Đăng ký thành công
Scenario: Customer đăng ký tài khoản hợp lệ
Given Customer chưa có tài khoản
And Customer cung cấp đầy đủ thông tin hợp lệ
When Customer chọn "Đăng ký"
Then hệ thống tạo tài khoản Customer
And tài khoản được lưu với trạng thái phù hợp
And hệ thống thông báo đăng ký thành công

AC-01.02 – Tài khoản đã tồn tại
Scenario: Customer đăng ký bằng thông tin đã tồn tại
Given số điện thoại hoặc email đã được sử dụng
When Customer thực hiện đăng ký
Then hệ thống không tạo tài khoản mới
And hiển thị thông báo tài khoản đã tồn tại

AC-01.03 – Dữ liệu không hợp lệ
Scenario: Customer nhập thông tin không hợp lệ
Given Customer đang ở màn hình đăng ký
When Customer nhập dữ liệu không hợp lệ
Then hệ thống từ chối đăng ký
And hiển thị lỗi tương ứng cho trường dữ liệu

2. UC-02 – Đăng nhập
AC-02.01 – Login thành công
Scenario: User đăng nhập bằng credential hợp lệ
Given User đã có tài khoản đang hoạt động
When User nhập đúng credential
Then hệ thống xác thực User thành công
And tạo session/token
And User được truy cập các chức năng theo role

AC-02.02 – Sai mật khẩu
Scenario: User nhập sai password
Given User đã tồn tại
When User nhập password không chính xác
Then hệ thống từ chối đăng nhập
And hiển thị thông báo lỗi

AC-02.03 – Account bị khóa
Scenario: User có account bị khóa
Given Account đang ở trạng thái LOCKED
When User thực hiện Login
Then hệ thống từ chối đăng nhập
And thông báo tài khoản không thể sử dụng

3. UC-03 – Book Vehicle

Đây là nhóm Acceptance Criteria Critical.

AC-03.01 – Tạo Booking thành công
Scenario: Customer tạo Booking hợp lệ
Given Customer đã đăng nhập
And Customer cung cấp Pickup Location hợp lệ
And Customer cung cấp Destination hợp lệ
And Service Type đang hoạt động
When Customer xác nhận đặt xe
Then hệ thống tạo Booking
And Booking có trạng thái SEARCHING_DRIVER
And hệ thống bắt đầu Driver Matching
And Customer nhận được thông báo Booking đã được tiếp nhận

AC-03.02 – Thiếu Pickup
Scenario: Customer không nhập Pickup Location
Given Customer đang tạo Booking
When Customer xác nhận Booking
Then hệ thống không tạo Booking
And hiển thị yêu cầu nhập Pickup Location

AC-03.03 – Thiếu Destination
Scenario: Customer không nhập Destination
Given Customer đang tạo Booking
When Customer xác nhận Booking
Then hệ thống không tạo Booking
And yêu cầu Customer nhập Destination

AC-03.04 – Service Type không khả dụng
Scenario: Service Type không còn hoạt động
Given Customer đã chọn một Service Type
And Service Type đã bị inactive
When Customer xác nhận Booking
Then hệ thống không tạo Booking
And thông báo Service Type không khả dụng

AC-03.05 – Duplicate Request
Scenario: Customer gửi cùng một Booking request nhiều lần
Given Customer đã gửi một Booking request
When cùng request được gửi lại
Then hệ thống không tạo duplicate Booking
And trả về kết quả của Booking hiện tại

4. UC-04 – Driver Matching
AC-04.01 – Tìm thấy Driver
Scenario: Có Driver phù hợp
Given Booking đang ở trạng thái SEARCHING_DRIVER
And có Driver đang AVAILABLE
And Driver đáp ứng các tiêu chí Matching
When hệ thống thực hiện Driver Matching
Then hệ thống lựa chọn Candidate Driver phù hợp
And gửi Trip Request đến Driver
And Booking tiếp tục ở trạng thái chờ Driver response

AC-04.02 – Driver Accept
Scenario: Driver chấp nhận Booking
Given Driver đã nhận Trip Request
And Booking vẫn còn available
When Driver chọn Accept
Then hệ thống assign Driver cho Booking
And Booking chuyển sang DRIVER_ASSIGNED
And Driver chuyển sang trạng thái BUSY
And Customer nhận notification

AC-04.03 – Driver Reject
Scenario: Driver từ chối Booking
Given Driver đã nhận Trip Request
When Driver chọn Reject
Then hệ thống ghi nhận Driver đã từ chối
And Driver vẫn có thể nhận các Booking khác nếu AVAILABLE
And hệ thống tiếp tục tìm Driver khác
And Customer không cần tạo lại Booking

AC-04.04 – Driver Timeout
Scenario: Driver không phản hồi
Given Trip Request đã được gửi đến Driver
When thời gian phản hồi vượt quá timeout được cấu hình
Then Trip Request được đánh dấu expired
And hệ thống tiếp tục tìm Driver khác
And Customer không cần tạo lại Booking

AC-04.05 – Không tìm được Driver
Scenario: Không có Driver phù hợp
Given Booking đang SEARCHING_DRIVER
And hệ thống đã thực hiện Matching theo policy
And không còn Candidate phù hợp
When Matching kết thúc
Then Booking chuyển sang NO_DRIVER_FOUND
And Customer nhận thông báo không tìm được Driver

5. UC-05 – Driver Accept / Reject Trip
AC-05.01 – Driver nhận được Trip Request
Scenario: Driver đủ điều kiện nhận Trip Request
Given Driver đang AVAILABLE
And có Booking phù hợp
When hệ thống gửi Trip Request
Then Driver nhận được thông báo
And Driver có thể xem thông tin Trip Request

AC-05.02 – Hai Driver cùng Accept
Scenario: Hai Driver cùng cố gắng Accept một Booking
Given Booking chỉ được assign cho một Driver
And Driver A và Driver B đều nhận được Trip Request
When cả hai cùng thực hiện Accept
Then chỉ một Driver được assign thành công
And Driver còn lại nhận thông báo Booking không còn khả dụng
And hệ thống không tạo duplicate assignment

6. UC-06 – Track Trip
AC-06.01 – Customer xem trạng thái Trip
Scenario: Customer theo dõi Trip
Given Customer có một Booking hợp lệ
When Customer mở màn hình Track Trip
Then hệ thống hiển thị Trip Status
And hiển thị thông tin Driver nếu đã được assign
And hiển thị ETA nếu có dữ liệu

AC-06.02 – Driver Location thay đổi
Scenario: Driver cập nhật vị trí
Given Trip đang được thực hiện
When Driver gửi Location mới
Then hệ thống cập nhật Last Known Location
And Customer có thể xem vị trí mới theo chính sách cập nhật

AC-06.03 – Location Service lỗi
Scenario: Location Service unavailable
Given Trip đang diễn ra
When Location Service không phản hồi
Then Trip không bị chuyển sang FAILED
And hệ thống giữ Last Known Location nếu có
And Customer vẫn có thể xem Trip Status

7. UC-07 – Update Trip Status
AC-07.01 – Driver đến điểm đón
Scenario: Driver đến Pickup
Given Trip đang ở trạng thái DRIVER_ASSIGNED
When Driver xác nhận đã đến Pickup
Then Trip chuyển sang ARRIVED
And Customer nhận notification

AC-07.02 – Driver đón Customer
Scenario: Driver đã đón Customer
Given Trip đang ở trạng thái ARRIVED
When Driver xác nhận đã đón Customer
Then Trip chuyển sang PICKED_UP
And Customer nhận notification

AC-07.03 – Bắt đầu chuyến
Scenario: Driver bắt đầu di chuyển
Given Trip đang ở trạng thái PICKED_UP
When Driver bắt đầu Trip
Then Trip chuyển sang IN_PROGRESS

AC-07.04 – Hoàn thành chuyến
Scenario: Driver hoàn thành Trip
Given Trip đang ở trạng thái IN_PROGRESS
When Driver xác nhận hoàn thành
Then Trip chuyển sang COMPLETED
And hệ thống ghi nhận thời gian hoàn thành
And hệ thống bắt đầu Calculate Fare
And Customer nhận notification

AC-07.05 – Invalid Status Transition
Scenario: Driver thực hiện Status Transition không hợp lệ
Given Trip đang ở trạng thái COMPLETED
When Driver cố chuyển Trip về IN_PROGRESS
Then hệ thống từ chối thao tác
And Trip vẫn giữ trạng thái COMPLETED
And hành động được ghi log nếu cần

8. UC-08 – Calculate Fare
AC-08.01 – Tính cước thành công
Scenario: Trip hoàn thành và có đầy đủ dữ liệu tính cước
Given Trip đã COMPLETED
And Service Type có Fare Rule hợp lệ
And dữ liệu Trip cần thiết tồn tại
When hệ thống Calculate Fare
Then hệ thống tính được Fare
And lưu Fare vào Trip
And Customer có thể xem số tiền phải trả

AC-08.02 – Fare Rule không tồn tại
Scenario: Không có Fare Rule
Given Trip đã COMPLETED
And Service Type không có Fare Rule hợp lệ
When hệ thống Calculate Fare
Then hệ thống không tạo Fare sai
And ghi nhận lỗi
And Trip không bị mất
And Operation có thể xử lý exception

9. UC-09 – Payment
AC-09.01 – Cash Payment
Scenario: Customer thanh toán tiền mặt
Given Trip đã COMPLETED
And Fare đã được xác định
When Customer chọn Cash
Then hệ thống ghi nhận Payment Method là CASH
And Payment được xử lý theo Cash Payment Policy

AC-09.02 – Electronic Payment thành công
Scenario: Electronic Payment thành công
Given Trip đã COMPLETED
And Fare đã được xác định
And Customer chọn Electronic Payment
When Payment Provider trả SUCCESS
Then hệ thống cập nhật Payment Status = SUCCESS
And Customer nhận thông báo thanh toán thành công

AC-09.03 – Payment thất bại
Scenario: Electronic Payment thất bại
Given Customer thực hiện Electronic Payment
When Payment Provider trả FAILED
Then hệ thống cập nhật Payment Status = FAILED
And Customer nhận thông báo thanh toán thất bại
And hệ thống cho phép Retry nếu policy cho phép

AC-09.04 – Payment Provider Timeout
Scenario: Payment Provider không phản hồi
Given Customer đang thực hiện Payment
When Payment Provider timeout
Then hệ thống không tạo duplicate Payment
And Payment được đánh dấu PENDING hoặc UNKNOWN theo policy
And Customer được thông báo trạng thái phù hợp

AC-09.05 – Không lưu Payment Sensitive Data
Scenario: Customer thực hiện Electronic Payment
Given Customer nhập thông tin thanh toán
When CAB gửi yêu cầu đến Payment Provider
Then CAB không lưu trực tiếp thông tin nhạy cảm của thẻ/tài khoản
And CAB chỉ lưu thông tin transaction cần thiết

10. UC-10 – Notification
AC-10.01 – Booking Created
Scenario: Booking được tạo
Given Customer tạo Booking thành công
When Booking chuyển sang SEARCHING_DRIVER
Then hệ thống tạo Notification Event
And Customer nhận được thông báo Booking đã được tiếp nhận

AC-10.02 – Driver Assigned
Scenario: Driver nhận Trip
Given Driver đã được assign
When Booking chuyển sang DRIVER_ASSIGNED
Then Customer nhận notification
And Driver nhận notification xác nhận chuyến

AC-10.03 – Driver Arrived
Scenario: Driver đến Pickup
Given Trip chuyển sang ARRIVED
When trạng thái được cập nhật
Then Customer nhận notification Driver đã đến

AC-10.04 – Notification Provider Failure
Scenario: Notification Provider bị lỗi
Given CAB phát sinh Notification Event
When Notification Provider không khả dụng
Then Booking/Trip vẫn tiếp tục hoạt động
And Notification được retry theo policy
And lỗi được ghi log

11. UC-11 – Rate Driver
AC-11.01 – Rating thành công
Scenario: Customer đánh giá Trip
Given Trip đã COMPLETED
And Customer là chủ Trip
And Customer chưa đánh giá Trip
When Customer gửi Rating hợp lệ
Then hệ thống lưu Rating
And Rating được gắn với Driver và Trip

AC-11.02 – Rating lần thứ hai
Scenario: Customer đã đánh giá Trip
Given Trip đã có Rating
When Customer gửi Rating lần nữa
Then hệ thống từ chối Rating mới
And không tạo duplicate Rating

12. UC-12 – Operation xử lý Exception
AC-12.01 – Xem Active Trip
Scenario: Operation Staff xem các Trip đang diễn ra
Given Staff đã đăng nhập
And Staff có quyền xem Trip
When Staff mở Active Trips
Then hệ thống hiển thị các Trip đang hoạt động
And hiển thị Status, Customer, Driver và thông tin liên quan

AC-12.02 – Can thiệp Trip
Scenario: Staff xử lý Trip Exception
Given Trip đang gặp lỗi
And Staff có permission phù hợp
When Staff thực hiện action được phép
Then hệ thống cập nhật Trip
And ghi Audit Log
And thông báo các bên liên quan nếu cần

AC-12.03 – Không đủ quyền
Scenario: Staff không có permission
Given Staff không có quyền thực hiện một thao tác nhạy cảm
When Staff cố thực hiện thao tác
Then hệ thống từ chối
And không thay đổi dữ liệu
And ghi nhận security event nếu cần

13. UC-13 – Manage Driver
AC-13.01 – Tạo Driver
Scenario: Operation tạo Driver
Given Staff có quyền Manage Driver
When Staff nhập thông tin Driver hợp lệ
Then hệ thống tạo Driver Account
And Driver có thể sử dụng account theo trạng thái được cấp

AC-13.02 – Driver không đủ điều kiện
Scenario: Driver chưa đủ thông tin/điều kiện
Given Driver Profile chưa hoàn tất
When Staff cố chuyển Driver sang AVAILABLE
Then hệ thống từ chối
And hiển thị các điều kiện còn thiếu

14. UC-14 – Manage Vehicle
AC-14.01 – Thêm Vehicle
Scenario: Staff thêm Vehicle hợp lệ
Given Staff có quyền Manage Vehicle
When Staff nhập thông tin Vehicle hợp lệ
Then hệ thống tạo Vehicle
And Vehicle được liên kết với Driver phù hợp

AC-14.02 – Vehicle không hợp lệ
Scenario: Vehicle không đủ điều kiện hoạt động
Given Vehicle đang inactive hoặc không hợp lệ
When Staff/Driver cố sử dụng Vehicle cho Trip
Then hệ thống từ chối
And Vehicle không được sử dụng để Matching

15. UC-15 – Reports
AC-15.01 – Xem báo cáo chuyến
Scenario: Staff xem báo cáo Trip
Given Staff có quyền xem Report
When Staff chọn khoảng thời gian
Then hệ thống hiển thị tổng số Trip
And số Trip Completed
And số Trip Cancelled
And tỷ lệ tương ứng

AC-15.02 – Báo cáo doanh thu
Scenario: Staff xem Revenue Report
Given dữ liệu Payment tồn tại
When Staff chọn khoảng thời gian
Then hệ thống hiển thị Revenue
And phân loại theo Payment Status

16. Acceptance Criteria cho toàn bộ Booking Flow

Đây là bộ End-to-End Acceptance Criteria quan trọng nhất để nghiệm thu MVP.

AC-E2E-01 – Happy Path
Scenario: Customer đặt xe thành công từ đầu đến cuối
Given Customer đã đăng nhập
And Customer có Pickup và Destination hợp lệ
And có Service Type hoạt động
And có Driver AVAILABLE phù hợp

When Customer tạo Booking
Then Booking được tạo với trạng thái SEARCHING_DRIVER

When hệ thống tìm thấy Driver
Then Trip Request được gửi đến Driver

When Driver Accept
Then Driver được assign vào Booking
And Booking chuyển sang DRIVER_ASSIGNED
And Customer nhận notification

When Driver đến Pickup
Then Trip chuyển sang ARRIVED
And Customer nhận notification

When Driver đón Customer
Then Trip chuyển sang PICKED_UP

When Driver bắt đầu di chuyển
Then Trip chuyển sang IN_PROGRESS

When Driver hoàn thành chuyến
Then Trip chuyển sang COMPLETED
And hệ thống Calculate Fare

When Customer thanh toán thành công
Then Payment chuyển sang SUCCESS
And Customer nhận Payment notification

When Customer gửi Rating
Then Rating được lưu thành công

17. Acceptance Criteria cho Failure Flow
AC-E2E-02 – Driver Reject
Scenario: Driver đầu tiên từ chối
Given Customer đã tạo Booking
And Driver A nhận Trip Request

When Driver A Reject
Then hệ thống không yêu cầu Customer tạo Booking mới
And hệ thống tìm Driver B
And Booking vẫn tồn tại

AC-E2E-03 – Driver Timeout
Scenario: Driver không phản hồi
Given Driver A nhận Trip Request
When Driver A không phản hồi trong thời gian timeout
Then Request của Driver A hết hạn
And hệ thống tiếp tục Matching
And Booking vẫn được giữ nguyên

AC-E2E-04 – No Driver
Scenario: Không tìm được Driver
Given Customer đã tạo Booking
And hệ thống không tìm được Driver phù hợp
When Matching kết thúc theo policy
Then Booking chuyển sang NO_DRIVER_FOUND
And Customer nhận thông báo rõ ràng

AC-E2E-05 – Payment Failed
Scenario: Payment thất bại
Given Trip đã COMPLETED
And Fare đã được tính
When Payment Provider trả FAILED
Then Payment chuyển sang FAILED
And Customer nhận thông báo
And Customer có thể Retry theo policy
And Trip vẫn giữ dữ liệu COMPLETED

AC-E2E-06 – Notification Failed
Scenario: Notification Provider bị lỗi
Given Trip đã được cập nhật trạng thái
When Notification Provider không khả dụng
Then Trip Status vẫn được cập nhật thành công
And hệ thống retry Notification theo policy
And lỗi được ghi log

18. Acceptance Criteria về Security
AC-SEC-01 – Authentication
Scenario: Truy cập chức năng protected
Given User chưa đăng nhập
When User truy cập chức năng yêu cầu authentication
Then hệ thống từ chối truy cập
And yêu cầu User Login

AC-SEC-02 – Authorization
Scenario: Staff không có quyền
Given Staff không có permission thực hiện thao tác
When Staff gọi chức năng protected
Then hệ thống trả về Access Denied
And dữ liệu không bị thay đổi

AC-SEC-03 – Audit
Scenario: Thực hiện thao tác nhạy cảm
Given User có quyền thực hiện thao tác
When User thay đổi dữ liệu nhạy cảm
Then hệ thống ghi Audit Log
And Audit Log chứa Actor, Action, Timestamp và Target

19. Acceptance Criteria về Reliability
AC-REL-01 – Duplicate Booking
Scenario: Customer retry Create Booking
Given request trước đó đã được xử lý
When Customer gửi lại cùng request
Then hệ thống không tạo duplicate Booking

AC-REL-02 – Duplicate Payment
Scenario: Payment request được retry
Given Payment transaction đã SUCCESS
When cùng transaction được gửi lại
Then hệ thống không charge Customer lần thứ hai
And trả về trạng thái Payment hiện tại

AC-REL-03 – Component Failure
Scenario: Payment Service unavailable
Given Customer đã có Booking/Trip hợp lệ
When Payment Service gặp lỗi
Then Booking và Trip vẫn tồn tại
And các chức năng không phụ thuộc Payment vẫn hoạt động

20. Acceptance Criteria về Scalability & Performance

Các giá trị cụ thể cần được ABC xác nhận, nhưng có thể đặt format AC như sau:

Scenario: System xử lý Peak Load
Given hệ thống đang ở mức tải cao theo SLA
When số lượng request đạt Peak Load được xác định
Then hệ thống vẫn đáp ứng SLA về Response Time
And tỷ lệ lỗi không vượt quá ngưỡng cho phép
And các component có thể scale độc lập


Các parameter cần chốt:

Metric	Target
API Response Time	[TBD]
Driver Matching Time	[TBD]
Concurrent Customer	[TBD]
Concurrent Driver	[TBD]
Peak TPS	[TBD]
Availability	[TBD]%
RTO	[TBD]
RPO	[TBD]
21. Acceptance Criteria Matrix
UC	Happy Path	Validation	Exception	Security	Priority
Register	✅	✅	✅	✅	Must
Login	✅	✅	✅	✅	Must
Book Vehicle	✅	✅	✅	✅	Critical
Driver Matching	✅	✅	✅	—	Critical
Accept/Reject	✅	✅	✅	—	Critical
Track Trip	✅	✅	✅	✅	Critical
Update Trip	✅	✅	✅	✅	Critical
Calculate Fare	✅	✅	✅	—	Critical
Payment	✅	✅	✅	✅	Critical
Notification	✅	—	✅	—	Critical
Rating	✅	✅	✅	—	Should
Manage Driver	✅	✅	✅	✅	Must
Manage Customer	✅	✅	✅	✅	Must
Manage Vehicle	✅	✅	✅	✅	Must
Exception Handling	✅	✅	✅	✅	Must
Reports	✅	✅	—	✅	Should
22. Definition of Done cho một Use Case

Một Use Case có thể coi là đủ điều kiện nghiệm thu khi:

Use Case
   │
   ├── Main Flow đã xác định
   ├── Alternative Flow đã xác định
   ├── Exception Flow đã xác định
   ├── Business Rules đã xác định
   ├── Preconditions đã xác định
   ├── Postconditions đã xác định
   ├── Acceptance Criteria đã viết
   ├── Permission đã xác định
   ├── Audit requirement đã xác định
   └── TBD đã được Business xác nhận


Đặc biệt với CAB, 6 nhóm AC cần ưu tiên trong Sprint/MVP là:

Booking → Driver Matching → Driver Accept/Reject → Trip Status → Fare → Payment

vì đây là chuỗi nghiệp vụ tạo nên core value flow của toàn bộ nền tảng CAB.



Bước 14: Truy xuất nguồn gốc yêu cầu
Truy xuất nguồn gốc yêu cầu – Requirements Traceability

Đối với dự án CAB System, truy xuất nguồn gốc yêu cầu (Requirements Traceability) nhằm đảm bảo mỗi yêu cầu nghiệp vụ đều có thể theo dõi xuyên suốt từ:

Business Goal
    ↓
Business Requirement
    ↓
Functional Requirement
    ↓
Use Case
    ↓
Acceptance Criteria
    ↓
Test Case
    ↓
Implementation


Mục tiêu là tránh tình trạng yêu cầu bị bỏ sót, phát triển chức năng không có căn cứ hoặc thay đổi yêu cầu nhưng không biết ảnh hưởng đến đâu.

1. Traceability Matrix tổng thể
Business Goal
Business Requirement
Functional Requirement
Use Case
Acceptance Criteria
Test Case
Implementation
2. Business Goal → Business Requirement
BG ID	Business Goal	BR ID	Business Requirement
BG-01	Tăng khả năng phục vụ khách hàng	BR-01	Cho phép Customer đăng ký, đăng nhập và đặt xe
BG-01	Tăng khả năng phục vụ khách hàng	BR-02	Customer có thể theo dõi trạng thái chuyến
BG-02	Tự động hóa phân công tài xế	BR-03	Hệ thống tự động tìm và phân công Driver
BG-02	Tự động hóa phân công tài xế	BR-04	Hệ thống xử lý Driver Reject/Timeout
BG-03	Quản lý tập trung thanh toán	BR-05	Hệ thống hỗ trợ tính cước và thanh toán
BG-04	Nâng cao trải nghiệm	BR-06	Hệ thống gửi Notification theo sự kiện
BG-05	Nâng cao hiệu quả vận hành	BR-07	Operation có giao diện quản trị
BG-05	Nâng cao hiệu quả vận hành	BR-08	Hệ thống cung cấp báo cáo vận hành
BG-06	Phát triển nền tảng lâu dài	BR-09	Hệ thống có khả năng mở rộng độc lập
BG-07	Đảm bảo an toàn hệ thống	BR-10	Hệ thống hỗ trợ Authentication, Authorization và Audit
3. Business Requirement → Functional Requirement
BR ID	Functional Requirement
BR-01	FR-01: Customer đăng ký tài khoản
BR-01	FR-02: Customer đăng nhập
BR-01	FR-03: Customer quản lý Profile
BR-01	FR-04: Customer tạo Booking
BR-02	FR-05: Customer xem Trip Status
BR-02	FR-06: Customer xem Driver Location
BR-03	FR-07: Hệ thống tìm Candidate Driver
BR-03	FR-08: Hệ thống ranking Driver
BR-03	FR-09: Hệ thống assign Driver
BR-04	FR-10: Xử lý Driver Reject
BR-04	FR-11: Xử lý Driver Timeout
BR-04	FR-12: Tiếp tục Matching Driver khác
BR-05	FR-13: Calculate Fare
BR-05	FR-14: Cash Payment
BR-05	FR-15: Electronic Payment
BR-05	FR-16: Payment Retry
BR-06	FR-17: Gửi Notification
BR-06	FR-18: Retry Notification
BR-07	FR-19: Manage Customer
BR-07	FR-20: Manage Driver
BR-07	FR-21: Manage Vehicle
BR-07	FR-22: Monitor Trip
BR-08	FR-23: Trip Report
BR-08	FR-24: Revenue Report
BR-08	FR-25: Driver Performance Report
BR-10	FR-26: Authentication
BR-10	FR-27: Role & Permission
BR-10	FR-28: Audit Log
4. Functional Requirement → Use Case
FR ID	Requirement	Use Case
FR-01	Register	UC-01
FR-02	Login	UC-02
FR-03	Manage Profile	UC-02 / UC-13 / UC-14
FR-04	Create Booking	UC-03
FR-05	Track Trip	UC-06
FR-06	View Driver Location	UC-06
FR-07	Find Candidate Driver	UC-04
FR-08	Ranking Driver	UC-04
FR-09	Assign Driver	UC-04
FR-10	Driver Reject	UC-05
FR-11	Driver Timeout	UC-04 / UC-05
FR-12	Continue Matching	UC-04
FR-13	Calculate Fare	UC-08
FR-14	Cash Payment	UC-09
FR-15	Electronic Payment	UC-09
FR-16	Payment Retry	UC-09
FR-17	Notification	UC-10
FR-18	Notification Retry	UC-10
FR-19	Manage Customer	UC-14
FR-20	Manage Driver	UC-13
FR-21	Manage Vehicle	UC-14
FR-22	Monitor Trip	UC-12
FR-23	Trip Report	UC-15
FR-24	Revenue Report	UC-15
FR-25	Driver Performance	UC-15
FR-26	Authentication	UC-02
FR-27	Authorization	UC-12 / UC-13 / UC-14 / UC-15
FR-28	Audit Log	UC-12 / UC-13 / UC-14
5. Use Case → Acceptance Criteria
UC	Use Case	AC ID	Acceptance Criteria
UC-01	Register	AC-01.01	Đăng ký thành công với dữ liệu hợp lệ
UC-01	Register	AC-01.02	Không cho đăng ký account đã tồn tại
UC-01	Register	AC-01.03	Validate dữ liệu không hợp lệ
UC-02	Login	AC-02.01	Login thành công
UC-02	Login	AC-02.02	Từ chối credential sai
UC-03	Book Vehicle	AC-03.01	Tạo Booking thành công
UC-03	Book Vehicle	AC-03.02	Không cho Booking thiếu Pickup
UC-03	Book Vehicle	AC-03.03	Không cho Booking thiếu Destination
UC-04	Driver Matching	AC-04.01	Tìm Driver phù hợp
UC-04	Driver Matching	AC-04.02	Driver Accept → Assign
UC-04	Driver Matching	AC-04.03	Driver Reject → tìm Driver khác
UC-04	Driver Matching	AC-04.04	Driver Timeout → tìm Driver khác
UC-04	Driver Matching	AC-04.05	Không tìm được Driver → thông báo Customer
UC-05	Accept/Reject	AC-05.02	Không duplicate Assignment
UC-06	Track Trip	AC-06.01	Customer xem Trip Status
UC-07	Update Trip	AC-07.01	Driver chuyển ARRIVED
UC-07	Update Trip	AC-07.02	Driver chuyển PICKED_UP
UC-07	Update Trip	AC-07.03	Driver chuyển IN_PROGRESS
UC-07	Update Trip	AC-07.04	Driver chuyển COMPLETED
UC-08	Calculate Fare	AC-08.01	Calculate Fare thành công
UC-09	Payment	AC-09.01	Cash Payment
UC-09	Payment	AC-09.02	Electronic Payment Success
UC-09	Payment	AC-09.03	Electronic Payment Failed
UC-09	Payment	AC-09.04	Payment Timeout
UC-10	Notification	AC-10.01	Booking Notification
UC-10	Notification	AC-10.02	Driver Assigned Notification
UC-10	Notification	AC-10.03	Driver Arrived Notification
UC-10	Notification	AC-10.04	Notification Provider Failure
UC-11	Rating	AC-11.01	Customer đánh giá thành công
UC-11	Rating	AC-11.02	Không cho đánh giá 2 lần
UC-12	Exception	AC-12.02	Staff xử lý Exception
UC-13	Driver	AC-13.01	Tạo Driver
UC-14	Vehicle/Customer	AC-14.01	Quản lý Vehicle
UC-15	Report	AC-15.01	Xem Trip Report
UC-15	Report	AC-15.02	Xem Revenue Report
6. End-to-End Traceability

Ví dụ với requirement quan trọng nhất: Customer đặt xe và được phân công Driver.

BG-02
Tự động hóa phân công Driver
        ↓
BR-03
Hệ thống tự động tìm và phân công Driver
        ↓
FR-07
Tìm Candidate Driver
        ↓
FR-08
Ranking Driver
        ↓
FR-09
Assign Driver
        ↓
UC-04
Driver Matching
        ↓
AC-04.01
Tìm thấy Driver phù hợp
        ↓
AC-04.02
Driver Accept → Assign
        ↓
TC-04-01
Test Matching thành công
        ↓
Implementation
Driver Matching Service


Một trường hợp khác:

BG-03
Quản lý tập trung thanh toán
        ↓
BR-05
Hỗ trợ tính cước và thanh toán
        ↓
FR-13
Calculate Fare
        ↓
FR-15
Electronic Payment
        ↓
UC-08 + UC-09
        ↓
AC-08.01
Calculate Fare thành công
        ↓
AC-09.02
Payment Success
        ↓
TC-08-01 + TC-09-01
        ↓
Fare Service + Payment Service

7. Traceability Matrix đầy đủ

Đây là bảng có thể dùng làm RTM – Requirements Traceability Matrix chính thức.

BR	FR	UC	AC	Test	Status
BR-01	FR-01	UC-01	AC-01.01	TC-REG-01	TBD
BR-01	FR-02	UC-02	AC-02.01	TC-LOGIN-01	TBD
BR-01	FR-04	UC-03	AC-03.01	TC-BOOK-01	TBD
BR-02	FR-05	UC-06	AC-06.01	TC-TRACK-01	TBD
BR-02	FR-06	UC-06	AC-06.02	TC-TRACK-02	TBD
BR-03	FR-07	UC-04	AC-04.01	TC-MATCH-01	TBD
BR-03	FR-08	UC-04	AC-04.01	TC-MATCH-02	TBD
BR-03	FR-09	UC-04	AC-04.02	TC-MATCH-03	TBD
BR-04	FR-10	UC-05	AC-04.03	TC-MATCH-04	TBD
BR-04	FR-11	UC-04	AC-04.04	TC-MATCH-05	TBD
BR-04	FR-12	UC-04	AC-04.05	TC-MATCH-06	TBD
BR-05	FR-13	UC-08	AC-08.01	TC-FARE-01	TBD
BR-05	FR-14	UC-09	AC-09.01	TC-PAY-01	TBD
BR-05	FR-15	UC-09	AC-09.02	TC-PAY-02	TBD
BR-05	FR-16	UC-09	AC-09.03	TC-PAY-03	TBD
BR-06	FR-17	UC-10	AC-10.01	TC-NOTI-01	TBD
BR-06	FR-18	UC-10	AC-10.04	TC-NOTI-02	TBD
BR-07	FR-19	UC-14	AC-12.01	TC-OPS-01	TBD
BR-07	FR-20	UC-13	AC-13.01	TC-DRV-01	TBD
BR-07	FR-22	UC-12	AC-12.02	TC-OPS-02	TBD
BR-08	FR-23	UC-15	AC-15.01	TC-RPT-01	TBD
BR-08	FR-24	UC-15	AC-15.02	TC-RPT-02	TBD
BR-10	FR-26	UC-02	AC-02.01	TC-SEC-01	TBD
BR-10	FR-27	UC-12	AC-12.03	TC-SEC-02	TBD
BR-10	FR-28	UC-12	AC-12.02	TC-SEC-03	TBD
8. Traceability theo chiều ngược

RTM không chỉ truy từ Business → Test, mà còn phải truy ngược.

Ví dụ:

TC-MATCH-04
    ↓
AC-04.03
    ↓
UC-04
    ↓
FR-10
    ↓
BR-04
    ↓
BG-02


Điều này giúp trả lời câu hỏi:

"Tại sao chúng ta phải xây chức năng này?"

Ví dụ Driver Reject không phải một chức năng phát sinh tùy ý. Nó bắt nguồn từ:

Business Goal
→ Tự động hóa Driver Assignment

Business Requirement
→ Không yêu cầu Customer tạo lại Booking khi Driver từ chối

Functional Requirement
→ Hệ thống phải tiếp tục Matching

Use Case
→ UC-04 Driver Matching

Acceptance Criteria
→ AC-04.03 Driver Reject

Test Case
→ TC-MATCH-04

9. Change Traceability

Khi Business thay đổi requirement, BA có thể dùng RTM để đánh giá Impact Analysis.

Ví dụ khách hàng yêu cầu:

"Thời gian Driver phải phản hồi giảm từ 30 giây xuống 15 giây."

Có thể truy ra:

BR-04
Driver Reject / Timeout
        ↓
FR-11
Driver Timeout
        ↓
UC-04
Driver Matching
        ↓
AC-04.04
Driver Timeout
        ↓
TC-MATCH-05
        ↓
Matching Service
        ↓
Configuration / Timeout Policy

Impact Analysis
Thành phần	Có ảnh hưởng?
Business Rule	✅
Use Case	✅
Acceptance Criteria	✅
Test Case	✅
Driver App	Có thể
Customer App	Không đáng kể
Matching Service	✅
Notification Service	Có thể
Database	Không nhất thiết

Đây chính là giá trị quan trọng của Requirements Traceability: khi một requirement thay đổi, team biết cần sửa gì, test lại gì và phần nào không bị ảnh hưởng.

10. Traceability Checklist cho BA

Trước khi chuyển yêu cầu sang Development, BA nên kiểm tra:

[ ] Mỗi Business Goal có Business Requirement
[ ] Mỗi Business Requirement có Functional Requirement
[ ] Mỗi Functional Requirement có Use Case
[ ] Mỗi Use Case có Acceptance Criteria
[ ] Mỗi Acceptance Criteria có Test Case
[ ] Mỗi Requirement đều có ID duy nhất
[ ] Không có Requirement bị orphan
[ ] Không có Use Case không có Business Requirement
[ ] Không có Test Case không có Requirement
[ ] Các Exception quan trọng đã được trace
[ ] Security Requirement đã được trace
[ ] Non-functional Requirement đã được trace
[ ] TBD đã được đánh dấu
[ ] Requirement Change có Impact Analysis

Chuỗi traceability chuẩn cho CAB
BGBusiness Goal
BRBusiness Requirement
FRFunctional Requirement
UCUse Case
ACAcceptance Criteria
TCTest Case
Implementation
Change Request

Kết luận: Với CAB System, nên quản lý RTM như một artifact riêng của BA, đặc biệt đối với 6 luồng Critical: Booking → Matching → Assignment → Trip → Fare → Payment. Điều này giúp chứng minh rằng yêu cầu của khách hàng đã được chuyển thành chức năng, Acceptance Criteria và Test Case một cách đầy đủ và có thể kiểm chứng.
