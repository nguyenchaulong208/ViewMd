Project MyShop 2025

A. Yêu cầu chung

A1. Tóm tắt yêu cầu

Tạo ra ứng dụng hỗ trợ chủ cửa hàng bán hàng.

A2. Người dùng của hệ thống

Hệ thống chỉ có một người dùng duy nhất là người chủ cửa hàng nhỏ.

A3. Kiến trúc chương trình

Chương trình có kiến trúc client - server, sử dụng database tùy chọn (demo Postgres).

A4. Luồng màn hình chính

graph TD
    LoginScreen[LoginScreen: Màn hình đăng nhập] --> ConfigScreen[ConfigScreen: Cấu hình địa chỉ kết nối]
    LoginScreen -- Nhập thông tin --> Auth{Đăng nhập thành công?}
    Auth -- Thành công --> MainApp[MainApp]
    MainApp -- Đăng xuất --> LoginScreen
    
    subgraph Các màn hình chính
        MainApp --> Dashboard[Dashboard: Tổng quan hệ thống]
        MainApp --> ProductsScreen[ProductsScreen: Quản lý loại & sản phẩm]
        MainApp --> OrdersScreen[OrdersScreen: Quản lý đơn hàng]
        MainApp --> ReportScreen[ReportScreen: Báo cáo kinh doanh]
        MainApp --> SettingsScreen[SettingsScreen: Cấu hình hệ thống]
    end


LoginScreen: Màn hình đăng nhập.

ConfigScreen: Cấu hình địa chỉ server để kết nối.

Dashboard: Cho biết tổng quan về hệ thống.

ProductsScreen: Màn hình quản lý loại sản phẩm và sản phẩm.

OrdersScreen: Màn hình quản lý các đơn hàng.

ReportScreen: Màn hình báo cáo tình hình kinh doanh của hệ thống.

SettingsScreen: Màn hình cấu hình cho hoạt động của chương trình.

A5. Lược đồ CSDL

Lược đồ CSDL gợi ý tổng quan

erDiagram
    CATEGORY ||--o{ PRODUCT : "belongs to"
    ORDER ||--o{ ORDER-ITEM : "includes"
    PRODUCT ||--o{ ORDER-ITEM : "ordered in"


Lược đồ CSDL gợi ý chi tiết

erDiagram
    CATEGORY {
        int category_id PK
        string name
        string description
    }
    
    PRODUCT {
        int product_id PK
        string sku
        string name
        int import_price
        int count
        string description
    }
    
    ORDER {
        int order_id PK
        DateTime created_Time
        int final_price
    }
    
    ORDER-ITEM {
        int order_item_id PK
        int quantity
        float unit_sale_price
        int total_price
    }

    CATEGORY ||--o{ PRODUCT : "belongs to"
    ORDER ||--o{ ORDER-ITEM : "includes"
    PRODUCT ||--o{ ORDER-ITEM : "ordered in"


Một số lưu ý về CSDL:

Thiết kế CSDL chỉ là gợi ý, học viên có thể tùy biến nếu thấy thích hợp. Nên trao đổi với giáo viên trước để được duyệt.

Giá sản phẩm không nhất thiết phải dùng tới kiểu dữ liệu tiền tệ chuyên biệt. Do đặc thù ở Việt Nam nên chỉ cần dùng số nguyên integer là quá đủ (max 4 tỉ).

B. Các chức năng cơ sở (5 điểm)

B1. Đăng nhập (0.25 điểm)

[ ] Nếu có thông tin đăng nhập lưu từ lần trước thì tự động đăng nhập và đi vào màn hình chính luôn.

[ ] Thông tin đăng nhập cần phải được mã hóa.

[ ] Màn hình đăng nhập cần hiển thị thông tin phiên bản của chương trình.

[ ] Cho phép cấu hình thông tin server từ màn hình Config.

Giao diện đăng nhập gợi ý:

Phần bên trái: Có thể thay thế bằng Logo và tên của ứng dụng (Hiển thị thông điệp "Welcome Back!").

Phần bên phải: Giao diện đăng nhập gồm tiêu đề "Login", các trường nhập liệu Username (mặc định hiển thị placeholder username@gmail.com), Password (ẩn mật khẩu dạng dấu *), tùy chọn "Remember Me", liên kết "Forgot Password?", nút hành động "Login", và dòng đăng ký nhanh "New User? Signup".

B2. Dashboard tổng quan hệ thống (0.5 điểm)

Mục tiêu của dashboard là nhằm cung cấp cái nhìn tổng quan của hệ thống. Các thông tin cơ bản bao gồm:

Tổng số sản phẩm.

Top 5 sản phẩm sắp hết hàng (số lượng < 5).

Top 5 sản phẩm bán chạy.

Tổng số đơn hàng trong ngày.

Tổng doanh thu trong ngày.

Chi tiết 3 đơn hàng gần nhất.

Biểu đồ doanh thu theo ngày trong tháng hiện tại.

B3. Quản lý sản phẩm - Products (Master data) (1.25 điểm)

Cho phép xem danh sách sản phẩm theo loại.

Xem chi tiết -> Xóa / Sửa.

Có hỗ trợ phân trang.

Cho phép sắp xếp theo một loại tiêu chí.

Cho phép lọc lại theo khoảng giá.

Cho phép tìm kiếm dựa theo từ khóa trong tên sản phẩm.

Thêm mới loại sản phẩm & Thêm mới sản phẩm.

Cho phép nhập (import) dữ liệu từ tập tin Excel hoặc Access.

Yêu cầu tối thiểu về dữ liệu mẫu:

Loại sản phẩm: có ít nhất 3 loại.

Sản phẩm: Mỗi loại sản phẩm có tối thiểu 22 sản phẩm.

Mỗi sản phẩm có tối thiểu 3 hình ảnh minh họa.

Dữ liệu mẫu không cần phải là thật nhưng nên trông giống thật.

B4. Quản lý đơn hàng - Orders (Transaction data) (1.5 điểm)

[ ] Tạo mới các đơn hàng.

[ ] Cho phép xóa một đơn hàng, cập nhật một đơn hàng.

[ ] Cho phép xem danh sách các đơn hàng có phân trang, xem chi tiết một đơn hàng.

[ ] Tìm kiếm các đơn hàng trong khoảng thời gian (từ ngày... đến ngày...).

stateDiagram-v2
    [*] --> Created : Khởi tạo đơn hàng
    Created --> Paid : Đã thanh toán
    Created --> Cancelled : Đã hủy
    Paid --> [*]
    Cancelled --> [*]


B5. Báo cáo thống kê - Report (1 điểm)

Mục tiêu chính của báo cáo là giúp người chủ:

Biết được tình trạng hệ thống hiện tại về sản phẩm & đơn hàng.

Nắm bắt được tình hình kinh doanh đang tiến triển theo chiều hướng nào.

Xem các sản phẩm và số lượng bán theo khoảng thời gian từ ngày đến ngày, theo tuần, theo tháng, hoặc theo năm (yêu cầu vẽ biểu đồ đường).

Báo cáo chi tiết doanh thu và lợi nhuận theo khoảng thời gian từ ngày đến ngày, theo tuần, theo tháng, hoặc theo năm (yêu cầu vẽ biểu đồ cột hoặc biểu đồ bánh/tròn).

B6. Cấu hình chương trình (0.25 điểm)

Cho phép hiệu chỉnh số lượng sản phẩm hiển thị trên mỗi trang khi thực hiện phân trang (Ví dụ các mốc: 5 / 10 / 15 / 20).

Lưu lại chức năng chính của lần cuối cùng mở ứng dụng.

Ví dụ: Nếu ở phiên làm việc trước, người dùng tắt ứng dụng khi đang ở màn hình Products thì ở lần đăng nhập tiếp theo, thay vì tự động chuyển đến màn hình mặc định là Dashboard, hệ thống sẽ tự động đưa người dùng thẳng vào màn hình làm việc trước đó là màn hình Products.

B7. Đóng gói thành file cài đặt (0.25 điểm)

Cần đóng gói toàn bộ chương trình thành file cài đặt dạng .exe để người dùng có thể dễ dàng tự cài đặt phần mềm vào hệ thống Windows.

C. Các chức năng tự chọn (5 điểm)

Học viên có thể lựa chọn các chức năng dưới đây để tích lũy thêm điểm (Tổng điểm tối đa phần này là 5 điểm):

[ ] Auto save (0.25 điểm): Tự động sao lưu tạm thời/lưu tự động khi tạo đơn hàng hoặc thêm mới sản phẩm.

[ ] Responsive layout (0.5 điểm): Tự động thay đổi, sắp xếp một cách hợp lý các thành phần giao diện theo độ rộng của màn hình làm việc.

[ ] Kiến trúc Plugin (1.0 điểm): Thiết kế chương trình có khả năng mở rộng một cách linh hoạt theo kiến trúc plugin.

[ ] Chương trình khuyến mãi (1.0 điểm): Bổ sung chức năng áp dụng các chương trình khuyến mãi, giảm giá trên hóa đơn hoặc sản phẩm.

[ ] Chống dịch ngược (0.25 điểm): Thực hiện làm rối mã nguồn (obfuscator) nhằm bảo mật sản phẩm và chống dịch ngược mã nguồn.

[ ] Chế độ dùng thử (0.5 điểm): Thiết lập chế độ dùng thử giới hạn thời gian (Cho phép trải nghiệm toàn bộ tính năng của phần mềm trong vòng 15 ngày, hết hạn sẽ yêu cầu mã kích hoạt hoặc đăng ký bản quyền).

[ ] Backup / Restore Database (0.25 điểm): Hỗ trợ chức năng sao lưu dự phòng và khôi phục dữ liệu hệ thống.

[ ] Sử dụng GraphQL (1.0 điểm): Thiết kế hệ thống sử dụng GraphQL API để truyền tải dữ liệu thay thế cho REST thông thường.

[ ] Kiến trúc MVVM (0.5 điểm): Phát triển chương trình áp dụng chuẩn mô hình kiến trúc Model-View-ViewModel.

[ ] Dependency Injection (0.5 điểm): Áp dụng kỹ thuật Dependency Injection vào kiến trúc hệ thống để quản lý và giảm thiểu sự phụ thuộc giữa các tầng lớp.

[ ] Phân quyền truy cập tài khoản (0.5 điểm): Phân chia rõ ràng quyền hạn giữa tài khoản quản trị (Admin) và nhân viên điều hành/bán hàng (Moderator / Sale).

Ví dụ: Nhân viên Sale chỉ được phép nhìn thấy giá bán lẻ của sản phẩm, trong khi Admin có thể theo dõi cả giá gốc nhập kho; hoặc nhân viên bán hàng A chỉ quản lý và nhìn thấy đơn hàng do chính mình tạo ra trong ngày mà không thể truy cập đơn hàng của nhân viên B.

[ ] Tính hoa hồng bán hàng (0.25 điểm): Hỗ trợ tính toán và chi trả thêm hoa hồng bán hàng cho nhân viên dựa trên tổng doanh số đạt được (KPI).

[ ] Quản lý khách hàng (0.5 điểm): Quản lý hồ sơ, thông tin liên lạc và lịch sử giao dịch mua hàng của khách hàng.

[ ] Viết Test Case (0.5 điểm): Thiết lập các test case tự động kiểm thử chức năng và giao diện của phần mềm.

[ ] In đơn hàng (0.5 điểm): Tính năng kết xuất hóa đơn. Lưu ý: Khi kiểm thử (test) không cần thiết phải kết nối máy in thật, có thể chọn lưu và in ra dưới dạng tệp tin PDF hoặc XPS là đạt yêu cầu.

[ ] Sắp xếp nâng cao (0.5 điểm): Hỗ trợ sắp xếp khi hiển thị danh sách theo đồng thời nhiều tiêu chí khác nhau, có khả năng tùy biến chiều sắp xếp tăng dần hay giảm dần một cách linh hoạt.

[ ] Tìm kiếm nâng cao (1.0 điểm): Phát triển bộ lọc và chức năng tìm kiếm nâng cao theo nhiều thuộc tính lồng ghép nhau.

[ ] Hỗ trợ Onboarding (0.5 điểm): Cung cấp tài liệu hướng dẫn nhanh, các tooltip hoặc quy trình trải nghiệm trực quan từng bước ngay khi người dùng mở phần mềm lần đầu tiên.

D. Hướng dẫn nộp bài

(Học viên lưu ý tuân thủ đúng quy chế và thời hạn nộp bài được ghi nhận trên hệ thống học tập).
