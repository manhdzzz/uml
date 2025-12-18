
| Ký hiệu                        | Ý nghĩa                                                                                                         |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| **○ (hình tròn rỗng)**         | **Public** visibility — có thể truy cập từ bên ngoài lớp                                                        |
| **● (hình tròn đầy / bullet)** | Thường không xuất hiện trong sơ đồ này để chỉ private, nhưng có thể tác giả dùng bullet để biểu thị **private** |
| **▬ (không ký hiệu)**          | Mặc định visibility (package / default) nếu sơ đồ không ghi rõ                                                  |
| Chữ *italic*, gạch chân        | Biểu thị static / abstract tuỳ quy ước                                                                          |

👉 Trong sơ đồ của bạn thì:

* **○ trước thuộc tính / method = public**
* Không có **− (minus)** hay **# (protected)** nên dùng bullet nhỏ hay không show thường là **private / internal** tuỳ tool vẽ.

---

## ✅ **2. Boundary – Control – Entity (BCE)**

Đây là quy ước phân loại lớp trong kiến trúc use-case:

![Image](https://www.cs.sjsu.edu/~pearce/modules/patterns/enterprise/ecb/ecb_files/image009.jpg?utm_source=chatgpt.com)

![Image](https://www.cs.sjsu.edu/~pearce/modules/lectures/ooa/analysis/ecb_files/image014.jpg?utm_source=chatgpt.com)

![Image](https://circle.visual-paradigm.com/wp-content/uploads/2017/06/Class-Diagram-Analysis-Stereotypes.png?utm_source=chatgpt.com)

| Loại lớp     | Ký hiệu stereotype («…»)    | Vai trò                                                                    |
| ------------ | --------------------------- | -------------------------------------------------------------------------- |
| **Boundary** | «boundary»                  | Giao diện cho người dùng (UI Form), nhận dữ liệu từ user, show lỗi, submit |
| **Control**  | «control»                   | Điều phối use case / logic nghiệp vụ, gọi repo / adapter                   |
| **Entity**   | (không luôn cần stereotype) | Đối tượng dữ liệu cốt lõi (Invoice, Payment, User…)                        |

👉 Nhìn sơ đồ của bạn:

* **LoginForm, InvoiceForm, ReportUI** — là **Boundary**
* **AuthenticationController, InvoiceController, ReconcileController** — là **Control**
* **User, Invoice, Payment, Report, ReportLine** — là **Entity**

---

## ✅ **3. Quan hệ giữa các lớp & mũi tên**

### 🔹 **Mũi tên đơn hướng (→)**

Biểu thị **dependency / navigation / dùng – gọi**:

* VD: `LoginForm → AuthenticationController`

  * UI Form gọi controller để xử lý login.

---

### 🔹 **Mối quan hệ kết cấu giữa Class (Association)**

Trong sơ đồ Invoice ↔ Payment bạn có:

```
Invoice 1 ——— 0..* Payment
```

| Ký hiệu          | Ý nghĩa                                                  |
| ---------------- | -------------------------------------------------------- |
| **1**            | Một đối tượng                                            |
| **0..***         | Không hoặc nhiều                                         |
| **—**            | Quan hệ *association* (liên kết trực tiếp)               |
| Không có mũi tên | Liên kết 2 chiều implicity (không bắt buộc 2 chiều code) |

➡ Ý nghĩa:
**Một Invoice có thể có 0 hoặc nhiều Payment**.

---

### 🔹 **Aggregation (hình thoi rỗng)**

Trong quan hệ Report → ReportLine:

```
Report (◇)——many ReportLine
```

| Ký hiệu                | Ý nghĩa                                                              |
| ---------------------- | -------------------------------------------------------------------- |
| **◇ (hình thoi rỗng)** | **Aggregation** — tập hợp tổng quát, các phần có thể tồn tại độc lập |
| trái tim đầy (◆)       | **Composition** — phần không thể tồn tại nếu tổng thể mất            |

➡ Ở đây:

* **Report chứa nhiều ReportLine**, nhưng **ReportLine vẫn có thể tồn tại độc lập về mặt dữ liệu logic** (không bị huỷ khi Report bị xoá).

---

### 🔹 **Dependency (gạch đứt)**

(Trong sơ đồ này không thấy)

* **---▷** thể hiện controller phụ thuộc repo/adapters (chỉ cần biết nó *gọi*, không tồn tại trực tiếp).

---

## ✅ **4. Thành phần trong class**

### Các dòng trong class UML

```
ClassName
--------------------
+ attribute
--------------------
+ method()
```

Giải thích:

* Phần 1: Tên lớp
* Phần 2: **Thuộc tính (attributes)**
* Phần 3: **Phương thức / hành vi (operations)**
* Dòng chia ngăn là cách UML tách phần attribute và method.

---

## ✅ **5. Các ký hiệu visibility (quy ước UML)**

| Ký hiệu | UML Standard      | Ý nghĩa                     |
| ------- | ----------------- | --------------------------- |
| **+**   | Public            | Có thể truy cập ngoài class |
| **−**   | Private           | Chỉ nội bộ class            |
| **#**   | Protected         | Cho subclass                |
| **~**   | Package (default) | Trong cùng package/module   |

👉 Trong sơ đồ bạn:

* Không thấy dấu “+, −, #” rõ → tác giả thay bằng **○ = public**.

---

## ✅ **6. Các ký hiệu khác trong sơ đồ**

| Ký hiệu                  | Ý nghĩa                                   |
| ------------------------ | ----------------------------------------- |
| **enum{…}**              | Liệt kê giá trị hợp lệ                    |
| **: type**               | Kiểu dữ liệu                              |
| **aggregation (◊)**      | Thuộc kiểu “has-a” nhưng *weak ownership* |
| **dependency arrow (→)** | Chỉ *sử dụng / gọi đến*                   |

---

## 📌 Ví dụ liên hệ lại với sơ đồ của bạn

✔ `InvoiceController`

* *Control class* dùng:

  * invoiceRepo
  * paymentRepo
  * gatewayAdapter
  * posAdapter

→ `InvoiceController` **không chứa trực tiếp data, nhưng điều phối tạo Invoice/Payments**

✔ `Invoice`

* *Entity class*
* Trạng thái = enum
* Các method như markPaid() trực tiếp thay đổi state của đối tượng

✔ `Report → ReportLine`

* Aggregation: Report “tập hợp nhiều” ReportLine

---

## 🧠 Mấu chốt để đọc UML nhanh

| Bạn muốn biết                | Nhìn vào                                   |
| ---------------------------- | ------------------------------------------ |
| UI form                      | «boundary»                                 |
| Logic xử lý                  | «control»                                  |
| Dữ liệu domain               | Entities (Invoice, Payment…)               |
| Có quan hệ “1 to many”       | association với multiplicities (1, 0..*,…) |
| Báo cáo chứa các dòng nhỏ    | aggregation                                |
| Controller dùng repo/adapter | dependency                                 |
