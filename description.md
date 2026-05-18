* [cite_start]**LoginScreen**: Màn hình đăng nhập [cite: 21]
* [cite_start]**ConfigScreen**: Cấu hình địa chỉ server để kết nối [cite: 23]
* [cite_start]**Dashboard**: Cho biết tổng quan về hệ thống [cite: 24]
* [cite_start]**ProductsScreen**: Màn hình quản lí loại sản phẩm và sản phẩm [cite: 25]
* [cite_start]**OrdersScreen**: Màn hình quản lí các đơn hàng [cite: 26]
* [cite_start]**ReportScreen**: Màn hình báo cáo tình hình kinh doanh của hệ thống [cite: 27]
* [cite_start]**SettingsScreen**: Màn hình cấu hình cho hoạt động của chương trình [cite: 28]

---

### A5. [cite_start]Lược đồ CSDL [cite: 29]

#### [cite_start]Lược đồ CSDL gợi ý tổng quan [cite: 30]

```mermaid
erDiagram
    CATEGORY ||--o{ PRODUCT : "belongs to"
    ORDER ||--o{ ORDER-ITEM : "includes"
    PRODUCT ||--o{ ORDER-ITEM : "ordered in"
[cite_start]