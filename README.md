# CHƯƠNG 1

# TỔNG QUAN VỀ HỆ THỐNG QUẢN LÝ THANH TOÁN VÀ THỐNG KÊ DOANH THU SIÊU THỊ

## 1.1 KHẢO SÁT HIỆN TRẠNG

### 1.1.1 Giới thiệu tổng quan về hệ thống

**Phát biểu bài toán:** Siêu thị cần một hệ thống **kế toán** (tập trung vào **quản lý hóa đơn** và **ghi nhận thanh toán** theo 2 phương thức: **online**, **tiền mặt**) cùng với **đối soát và thống kê doanh thu**. Mục tiêu là giảm chi phí, triển khai nhanh, hỗ trợ nhiều phương thức, thuận tiện cho nhân viên và chủ cửa hàng (CCH) khi kiểm soát dòng tiền và báo cáo. Yêu cầu cốt lõi gồm: quản lý hóa đơn hợp nhất, ghi nhận thanh toán chính xác, **đối soát** với sao kê/statement từ ngân hàng/đối tác và **thống kê doanh thu** theo ca/ngày/kênh. 

**Bối cảnh & tầm quan trọng:** Việt Nam hiện nay đang phát triển cực nhanh với các hệ thống siêu thị, tập chung vào đa nền tảng và đa hàng hóa. Các siêu thị hiện đại thường có **đa kênh thanh toán** (online qua cổng thanh toán, tiền mặt tại quầy...); tuy nhiên, nếu không áp dụng hệ thống kế toán sẽ khiến việc quản lý và đối soát trở nên vô cùng khó khăn và phức tạp. Việc xây dựng 1 hệ thống kế toán quản trị tập trung giúp: (i) giảm sai sót ghi nhận, (ii) tăng tốc đóng ca, (iii) hỗ trợ **đối soát chính xác** dựa trên chuẩn **ISO 20022 camt.053** (Bank-to-Customer Statement) khi nhập sao kê từ ngân hàng/đối tác; từ đó nâng độ tin cậy và tốc độ báo cáo.

**Các vấn đề đặc trưng cần giải quyết.**

1.  **Hóa đơn & thanh toán hợp nhất** (tránh mỗi kênh một hệ).
    
2.  **Đối soát – bù trừ – hạch toán** với bank statement/API.
    
3.  **An toàn thông tin** với các chuẩn an toàn thông tin quốc tế.
    

### 1.1.2 Khảo sát quy trình hiện tại

-   **Nhân viên (NV):** tạo hóa đơn; nhận thanh toán theo 1 trong 2 phương thức (online/tiền mặt).
    
-   **Khách hàng:** thanh toán theo phương thức đã chọn. Với **online**, kết quả thường được hệ thống nhận qua **webhook/callback** từ cổng thanh toán/NGÂN HÀNG; sau đó đợi nhân viên xác nhận và thực hiện lấy hóa đơn.
    
-   **Chủ cửa hàng (CCH):** cấu hình phương thức, xem báo cáo, chạy **đối soát** định kỳ, nhập sao kê.
    

## 1.2 THU THẬP YÊU CẦU

### 1.2.1 Yêu cầu chức năng (quy trình)

-   **Quản lý hóa đơn thanh toán:** tạo/sửa; chọn **phương thức** (online/tiền mặt); ghi nhận thanh toán; in/xuất hóa đơn chứng từ.
    
-   **Xử lý thanh toán:**
    
    -   **Online:** tạo yêu cầu thanh toán, nhận **callback/webhook** xác nhận thành công/thất bại.
        
    -   **Tiền mặt:** NV xác nhận, ghi nhận thu.
        
-   **Đối soát & báo cáo doanh thu:** tìm kiếm, tra cứu, đối soát đơn hàng, doanh thu.
    
-   **Quản trị & hệ thống:** người dùng–vai trò (QTV/CCH/NV), nhật ký/audit; cấu hình tích hợp.
    

### 1.2.2 Yêu cầu phi chức năng

-   **Hiệu năng:** tạo hóa đơn + khởi tạo yêu cầu thanh toán online ≤ 5s; phản hồi GUI dưới 300–500ms ở thao tác đơn.
    
-   **Tính sẵn sàng:** vận hành 24/7; tự động hoàn toàn.
    
-   **An toàn & tuân thủ:** Bảo mật mọi kênh qua TLS; Tránh spam, chặn kết nối không hợp lệ, xác thực SSO/OIDC; kiểm soát RBAC; ghi audit.
    
-   **Mở rộng & bảo trì:** kiến trúc mô-đun; lớp tích hợp chuẩn hóa (Payment Gateway, Bank Statement Adapter).
    

## 1.3 MỤC TIÊU VÀ PHẠM VI

### 1.3.1 Mục tiêu

-   **Mục tiêu chung:** nền tảng quản lý **hóa đơn & thanh toán** đa phương thức; **đối soát chính xác** với sao kê; **thống kê doanh thu** theo thời gian (ca/ngày/điểm bán/kênh).
    
-   **Mục tiêu cụ thể:**
    
    -   100% hóa đơn có **tham chiếu thanh toán**;
        
    -   Đối soát bán tự động với statement/API;
        
    -   Cảnh báo sai lệch; báo cáo chi tiết, xuất CSV/Excel.
        

### 1.3.2 Phạm vi

-   **Không gian:** áp dụng cho cửa hàng siêu thị tại Việt Nam, tích hợp đa ngân hàng/đối tác.
    
-   **Đối tượng:** nghiệp vụ **quản lý thanh toán** (hóa đơn & ghi nhận) và **thống kê doanh thu** (không mở rộng sang quản trị kho/sản phẩm).
    

----------

# CHƯƠNG 2

# PHÂN TÍCH THIẾT KẾ HỆ THỐNG

## 2.1 Phân tích yêu cầu hệ thống

### Ánh xạ yêu cầu ↔ Use Cases (đề tài mới)

-   **UC01** Đăng nhập/SSO (RBAC).
    
-   **UC02** Tạo **hóa đơn thanh toán** & ghi nhận thanh toán (online/tiền mặt).
    
-   **UC03** Nhận kết quả thanh toán online (webhook/callback).
    
-   **UC04** Đối soát – Sao kê – Báo cáo doanh thu.
    
-   **UC05** Quản lý người dùng/quyền (QTV/CCH/NV).
    
-   **UC06** Nhật ký/audit & cảnh báo.
    

**Độ ưu tiên:**  
P0: UC02, UC03, UC04 (lõi hóa đơn–thanh toán–đối soát)  
P1: UC01, UC05, UC06 (đăng nhập & quản trị & giám sát)

**Phụ thuộc:** UC02/UC04/UC05/UC06 phụ thuộc đăng nhập (UC01). UC04 dựa trên dữ liệu từ UC02–UC03 + statement (camt.053/CSV/API).

## 2.2 Xác định Actors và vai trò

-   **QTV (Quản trị viên):** quản trị người dùng, cấu hình, nhật ký/audit.
    
-   **CCH (Chủ cửa hàng):** xem báo cáo, chạy đối soát, cấu hình phương thức.
    
-   **NV (Nhân viên):** tạo hóa đơn, nhận thanh toán.
    
-   **Payment Gateway/Ngân hàng (bên ngoài):** gửi **webhook** xác nhận giao dịch online; xuất **statement camt.053**/CSV/API cho đối soát. 
    

## 2.3 Xác định và mô tả Use Cases

### 2.3.1 Danh sách Use Cases

UC01–UC06 như mục 2.1.

### 2.3.2 Biểu đồ Use Case tổng quát: 

img1

### 2.3.3 Mô tả chi tiết Use Cases:

**UC01 – Đăng nhập/SSO.** : Mục đích: xác thực, tạo phiên, hiển thị menu theo vai trò. 
- Tiền ĐK: TK hợp lệ.  Hậu ĐK: phiên hợp lệ (cookie an toàn). 
- Luồng chính: mở form → nhập thông tin/SSO → xác thực → tạo session & điều hướng. 
- Ngoại lệ: sai thông tin, TK khóa. (Khuyến nghị SSO/OIDC; cookie **HttpOnly/Secure/SameSite**; audit.)

**UC02 – Tạo hóa đơn & ghi nhận thanh toán.** : Mục đích: lập **hóa đơn** và ghi nhận theo **online/tiền mặt**. 
- Tiền ĐK: đăng nhập. _Hậu ĐK:_ hóa đơn có trạng thái & thông tin thanh toán tương ứng. 
- Luồng chính:_ nhập dữ liệu hóa đơn → chọn phương thức:

	-   **Online:** tạo request, hiển thị hướng dẫn; sau đó chờ UC03 (kết quả).
	    
	-   **Tiền mặt:** NV xác nhận thu & đánh dấu “paid”.  
	    _Ngoại lệ:_ thiếu dữ liệu, giao dịch lỗi, timeout.
    

**UC03 – Nhận kết quả thanh toán online (webhook).** 
_Mục đích:_ xác minh chữ ký & cập nhật thanh toán **idempotent**; ánh xạ về hóa đơn. 
_Hậu ĐK:_ hóa đơn chuyển “đã thanh toán” hoặc “thất bại”, ghi log. 

**UC04 – Đối soát – Sao kê – Báo cáo.** 
_Mục đích:_ nhập **statement camt.053/CSV/API**, chuẩn hóa–so khớp với ledger nội bộ, phân loại; xuất **CSV/Excel** & dashboard KPI.

----------

## 2.4 Phân tích thiết kế chi tiết từng Use Case:


### 2.4.1 UC01 — **Đăng nhập**

**(A) Use Case chức năng đăng nhập (UC01)**

img2

**(B) Sequence — Đăng nhập (Biểu đồ tuần tự)**

img3

**(C) Collaboration — Đăng nhập (Biểu đồ cộng tác)**

img4

**(D) Activity — Đăng nhập (Biểu đồ hoạt động)**

img5

**(E) State — Vòng đời đăng nhập (Biểu đồ trạng thái)**

img6

----------

### 2.4.2 UC02 — **Tạo hóa đơn & ghi nhận thanh toán**

**(A) Use Case 02**

img7

**(B) Sequence diagram**

img8

**(C) Collaboration diagram **

img9

**(D) Activity diagram**

img10

**(E) State diagram (Vòng đời hóa đơn)**

img11

----------

### 2.4.3 UC04 — **Đối soát – Sao kê – Báo cáo (Thống kê doanh thu)**

**(A) Use Case diagram**

img12

**(B) Sequence diagram — Đối soát & Báo cáo**

img13

**(C) Collaboration diagram — Đối soát**

img14

**(D) Activity diagram— Quy trình đối soát**

img15

**(E) State diagram — Vòng đời Báo cáo & đối soát**

img16

----------

## 2.5 Phân tích thiết kế **biểu đồ lớp**

### 2.5.1 Mô tả chi tiết các lớp

#### 2.5.1.1 **Chức năng đăng nhập**

**Entity — `User`**: `id, username, passwordHash, role(QTV|CCH|NV), status, createdAt`; methods: `checkRole()`, `isActive()`, `lock()`, `unlock()`.  
**Boundary — `LoginForm`**: `username, password, rememberMe`; methods: `submit()`, `showError()`.  
**Control — `AuthenticationController`**: attrs `sessionManager, userRepo`; methods: `login()`, `logout()`, `authorize(role)`.  


#### 2.5.1.2 **Chức năng tạo hóa đơn & ghi nhận thanh toán**

**Entity**

-   `Invoice`: `invoiceId, amount, currency, note, status(Pending|Paid|Failed|Canceled), createdAt, paidAt, method(ONLINE|CARD|CASH)`; methods: `markPending()`, `markPaid()`, `markFailed()`, `cancel()`.
    
-   `Payment`: `paymentId, invoiceId, method, amount, providerRef/bankRef, authCode, status, createdAt`; methods: `approve()`, `decline()`.
    

**Boundary**

-   `InvoiceForm`: `items?, amount, note, method`; methods: `submit()`, `showError()`.
    

**Control**

-   `InvoiceController`: `invoiceRepo, paymentRepo, gatewayAdapter, posAdapter`; methods: `createInvoice(...)`, `captureOnline(...)`, `captureCard(...)`, `captureCash(...)`.
    

#### 2.5.1.3 **Chức năng thống kê doanh thu (đối soát & báo cáo)**

**Entity**

-   `Report`: `reportId, period, store, channel, totals, createdAt`;
    
-   `ReportLine`: `lineId, reportId, invoiceId?, paymentId?, status(match|unmatch|duplicate), note`.
    

**Boundary**

-   `ReportUI`: tham số lọc kỳ/kênh/cửa hàng; export.
    

**Control**

-   `ReconcileController`: `txnRepo, statementAdapter, reportRepo`; methods: `reconcile(range, filters)`, `export()`.
    

### 2.5.2 Quan hệ giữa các lớp (tóm tắt)

-   `LoginForm → AuthenticationController ↔ User`.
    
-   `InvoiceForm → InvoiceController ↔ (Invoice, Payment)`; `InvoiceController` ↔ `gatewayAdapter` (online), ↔ `posAdapter` (thẻ).
    
-   `ReconcileController ↔ (Transaction/Payment, Invoice)`; `ReconcileController ↔ statementAdapter ↔ Bank/API`; `Report 1..* ReportLine`.
    

### 2.5.3 **Luồng dữ liệu chính** 

**Đăng nhập**

-   **Đầu vào:** `{ username/password | id_token(SSO) }`
    
-   **Xử lý:** validate → verify mật khẩu/**OIDC** → **tạo session** (regenerate ID; cookie **HttpOnly/Secure/SameSite**) → Menu chức năng tương ứng.
    
-   **Đầu ra:** cookie phiên an toàn + chuyển hướng GUI theo QTV/CCH/NV.
    

**Tạo hóa đơn & ghi nhận thanh toán**

-   **Đầu vào:** `{ amount, note, method }` (+ hàng hóa nếu cần)
    
-   **Xử lý:** tạo **Invoice**; nhánh ONLINE (tạo yêu cầu; chờ UC03), nhánh CASH (NV xác nhận); ghi **Payment**, cập nhật **Invoice.status**.
    
-   **Đầu ra:** biên nhận/phiếu thu + trạng thái Invoice/Payment.
    

**Đối soát & thống kê doanh thu**

-   **Đầu vào:** ledger nội bộ (Invoice/Payment) + **statement camt.053/CSV/API**.
    
-   **Xử lý:** chuẩn hóa; ánh xạ theo **bankRef/txnId** (fallback: orderRef+amount+time); so khớp & phân loại; tổng hợp KPI.
    
-   **Đầu ra:** **bảng tổng hợp & chi tiết**, export **CSV/Excel**.
    

### 2.5.4 **Biểu đồ lớp (Quản lý Thanh toán & Thống kê Doanh thu siêu thị)**

img17

----------

## 2.6 Phân tích thiết kế **biểu đồ thành phần**

img18

## 2.7 Phân tích thiết kế **biểu đồ triển khai**

img19

----------


# CHƯƠNG 3: PHÁT SINH MÃ TRÌNH

## 3.1 Mã trình chức năng **Đăng nhập**

### 3.1.1 Entity **USER**

```js
const bcrypt = require('bcrypt');
const mysql = require('mysql2/promise');
require('dotenv').config();

class User {
  constructor({ id, username, passwordHash, role = 'NV', status = 'ACTIVE' }) {
    this.id = id;
    this.username = username;
    this.passwordHash = passwordHash;
    this.role = role;        // 'QTV' | 'CCH' | 'NV'
    this.status = status;    // 'ACTIVE' | 'LOCKED'
  }

  isActive() {
    return this.status === 'ACTIVE';
  }

  checkRole(requiredRole) {
    return this.role === requiredRole;
  }

  async checkPassword(plain) {
    return bcrypt.compare(plain, this.passwordHash);
  }
}

// Kết nối MySQL
const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});

// Repository
const userRepo = {
  async findByUsername(username) {
    const [rows] = await pool.query('SELECT * FROM users WHERE username = ?', [username]);
    if (!rows.length) return null;
    const userData = rows[0];
    return new User({
      id: userData.id,
      username: userData.username,
      passwordHash: userData.password_hash, // cột trong DB
      role: userData.role,
      status: userData.status
    });
  }
};

module.exports = { User, userRepo };

```

### 3.1.2 AuthenticationController (Control):

```js
const jwt = require('jsonwebtoken');
const { userRepo } = require('../models/user');
require('dotenv').config(); // đọc biến môi trường

const JWT_SECRET = process.env.JWT_SECRET;

async function login(req, res) {
  const { username, password } = req.body;

  try {
    const user = await userRepo.findByUsername(username);
    if (!user) {
      return res.status(401).json({ message: 'Sai tài khoản hoặc mật khẩu' });
    }

    if (!user.isActive()) {
      return res.status(403).json({ message: 'Tài khoản đã ngưng hoạt động' });
    }

    const ok = await user.checkPassword(password);
    if (!ok) {
      return res.status(401).json({ message: 'Sai tài khoản hoặc mật khẩu' });
    }

    // Tạo token phiên
    const token = jwt.sign(
      { userId: user.id, role: user.role },
      JWT_SECRET,
      { expiresIn: '8h' }
    );

    res.json({ token, role: user.role });
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: 'Lỗi server' });
  }
}

function authorize(requiredRole) {
  // middleware kiểm tra token + quyền
  return (req, res, next) => {
    const auth = req.headers.authorization || '';
    const token = auth.replace('Bearer ', '');
    if (!token) return res.status(401).json({ message: 'Chưa đăng nhập' });

    try {
      const payload = jwt.verify(token, JWT_SECRET);
      req.user = payload;

      if (requiredRole && payload.role !== requiredRole) {
        return res.status(403).json({ message: 'Không đủ quyền' });
      }

      next();
    } catch (err) {
      return res.status(401).json({ message: 'Phiên không hợp lệ' });
    }
  };
}

module.exports = { login, authorize };

```

### 3.1.3 Login form:

```js
<!-- login-->
<form id="login-form">
  <input name="username" placeholder="Tên đăng nhập">
  <input name="password" type="password" placeholder="Mật khẩu">
  <button type="submit">Đăng nhập</button>
</form>

<script>
document.getElementById('login-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const form = e.target;

  const body = {
    username: form.username.value,
    password: form.password.value
  };

  const res = await fetch('/api/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(body)
  });

  const data = await res.json();
  if (!res.ok) {
    alert(data.message || 'Đăng nhập thất bại');
    return;
  }

  // Lưu token vào localStorage để các API khác dùng
  localStorage.setItem('token', data.token);
  alert('Đăng nhập thành công, vai trò: ' + data.role);
  // chuyển hướng tùy role (QTV/CCH/NV)
});
</script>

```


## 3.2 Mã trình chức năng **Tạo hóa đơn**

### 3.2.1 Entity Invoice & Payment:

```js
const mysql = require('mysql2/promise');
require('dotenv').config();

// Kết nối MySQL
const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});

// ===== Invoice =====
class Invoice {
  constructor({ id, amount, currency = 'VND', note, status = 'PENDING', method = null }) {
    this.id = id;
    this.amount = amount;
    this.currency = currency;
    this.note = note;
    this.status = status;
    this.method = method;
  }

  markPending() { this.status = 'PENDING'; }
  markPaid()    { this.status = 'PAID'; }
  markFailed()  { this.status = 'FAILED'; }
  cancel()      { this.status = 'CANCELED'; }
}

const invoiceRepo = {
  async create(data) {
    const [result] = await pool.query(
      `INSERT INTO invoices (amount, currency, note, status, method)
       VALUES (?, ?, ?, ?, ?)`,
      [data.amount, data.currency || 'VND', data.note, data.status || 'PENDING', data.method || null]
    );
    return new Invoice({ id: result.insertId, ...data });
  },

  async findById(id) {
    const [rows] = await pool.query('SELECT * FROM invoices WHERE id = ?', [id]);
    if (!rows.length) return null;
    return new Invoice(rows[0]);
  }
};

// ===== Payment =====
class Payment {
  constructor({ id, invoiceId, method, amount, providerRef = null, status = 'PENDING' }) {
    this.id = id;
    this.invoiceId = invoiceId;
    this.method = method;
    this.amount = amount;
    this.providerRef = providerRef;
    this.status = status;
  }

  approve() { this.status = 'SUCCESS'; }
  decline() { this.status = 'FAILED'; }
}

const paymentRepo = {
  async create(data) {
    const [result] = await pool.query(
      `INSERT INTO payments (invoice_id, method, amount, provider_ref, status)
       VALUES (?, ?, ?, ?, ?)`,
      [data.invoiceId, data.method, data.amount, data.providerRef || null, data.status || 'PENDING']
    );
    return new Payment({ id: result.insertId, ...data });
  },

  async findByProviderRef(ref) {
    const [rows] = await pool.query('SELECT * FROM payments WHERE provider_ref = ?', [ref]);
    if (!rows.length) return null;
    return new Payment(rows[0]);
  }
};

module.exports = { Invoice, Payment, invoiceRepo, paymentRepo };

```

### 3.2.2 InvoiceController – tạo hóa đơn & thanh toán:

```js
const { invoiceRepo, paymentRepo } = require('../models/invoice');

// Giả lập cổng thanh toán
const gatewayAdapter = {
  async createPaymentRequest(invoice) {
    // Trả về URL cổng thanh toán
    const fakeProviderRef = 'GW-' + invoice.id + '-' + Date.now();
    const paymentUrl = `https://gateway-demo/pay?ref=${fakeProviderRef}`;
    return { providerRef: fakeProviderRef, paymentUrl };
  }
};

// Tạo hóa đơn
async function createInvoice(req, res) {
  const { amount, note, method } = req.body; // method: 'CASH' hoặc 'ONLINE'

  if (!amount || !method) {
    return res.status(400).json({ message: 'Thiếu số tiền hoặc phương thức' });
  }

  try {
    // 1️⃣ Tạo invoice trong DB
    const invoice = await invoiceRepo.create({
      amount,
      note,
      method,
      status: 'PENDING'
    });

    // 2️⃣ Nếu là tiền mặt: đánh dấu đã thanh toán luôn
    if (method === 'CASH') {
      invoice.markPaid();
      await paymentRepo.create({
        invoiceId: invoice.id,
        method: 'CASH',
        amount,
        providerRef: null,
        status: 'SUCCESS'
      });

      // Có thể update status hóa đơn trong DB nếu muốn
      await invoiceRepo.updateStatus(invoice.id, invoice.status);

      return res.json({ invoice, message: 'Đã ghi nhận thanh toán tiền mặt' });
    }

    // 3️⃣ Nếu ONLINE: tạo yêu cầu cổng thanh toán
    if (method === 'ONLINE') {
      const { providerRef, paymentUrl } = await gatewayAdapter.createPaymentRequest(invoice);
      await paymentRepo.create({
        invoiceId: invoice.id,
        method: 'ONLINE',
        amount,
        providerRef,
        status: 'PENDING'
      });

      return res.json({
        invoice,
        paymentUrl,
        message: 'Hóa đơn tạo thành công, chuyển khách đến URL thanh toán'
      });
    }

    return res.status(400).json({ message: 'Phương thức không hỗ trợ' });
  } catch (err) {
    console.error(err);
    return res.status(500).json({ message: 'Lỗi server' });
  }
}

module.exports = { createInvoice };

```

### 3.2.3 Webhook nhận kết quả thanh toán (UC03):

```js
const { invoiceRepo, paymentRepo } = require('../models/invoice');

// status: 'SUCCESS' | 'FAILED'
async function handleGatewayWebhook(req, res) {
  const { providerRef, status } = req.body;

  try {
    // Tìm payment theo providerRef
    const payment = await paymentRepo.findByProviderRef(providerRef);
    if (!payment) {
      return res.json({ ok: true });
    }

    // Nếu đã xử lý rồi, bỏ qua
    if (payment.status === 'SUCCESS' || payment.status === 'FAILED') {
      return res.json({ ok: true, message: 'Đã xử lý trước đó' });
    }

    // Tìm invoice liên quan
    const invoice = await invoiceRepo.findById(payment.invoiceId);
    if (!invoice) {
      return res.json({ ok: true, message: 'Không tìm thấy hóa đơn' });
    }

    // Cập nhật trạng thái payment + invoice
    if (status === 'SUCCESS') {
      payment.approve();
      invoice.markPaid();
    } else {
      payment.decline();
      invoice.markFailed();
    }

    // Lưu vào DB
    await paymentRepo.updateStatus(payment.id, payment.status);
    await invoiceRepo.updateStatus(invoice.id, invoice.status);

    return res.json({ ok: true });
  } catch (err) {
    console.error(err);
    return res.status(500).json({ ok: false, message: 'Lỗi server' });
  }
}

module.exports = { handleGatewayWebhook };

```

### 3.2.4 Form tạo hóa đơn

```js
<form id="invoice-form">
  <input name="amount" type="number" placeholder="Số tiền">
  <textarea name="note" placeholder="Ghi chú"></textarea>
  <select name="method">
    <option value="CASH">Thanh toán tiền mặt</option>
    <option value="ONLINE">Thanh toán online</option>
  </select>
  <button type="submit">Tạo hóa đơn</button>
</form>

<script>
const token = localStorage.getItem('token');

document.getElementById('invoice-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const f = e.target;
  const body = {
    amount: Number(f.amount.value),
    note: f.note.value,
    method: f.method.value
  };

  const res = await fetch('/api/invoices', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer ' + token
    },
    body: JSON.stringify(body)
  });

  const data = await res.json();
  if (!res.ok) return alert(data.message || 'Lỗi tạo hóa đơn');

  if (data.paymentUrl) {
    // hóa đơn online => mở URL thanh toán
    window.open(data.paymentUrl, '_blank');
  } else {
    alert('Đã ghi nhận thanh toán tiền mặt');
  }
});
</script>

```
## 3.3 Mã trình chức năng **Đối soát, sao kê**

### 3.3.1 Entity Report & ReportLine

```js
const mysql = require('mysql2/promise');
require('dotenv').config();

// Kết nối MySQL
const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});

// ===== Report =====
class Report {
  constructor({ id, period, channel, totals, createdAt }) {
    this.id = id;
    this.period = period;   // vd: '2024-10-01..2024-10-31'
    this.channel = channel; // 'ONLINE' | 'CASH' | 'ALL'
    this.totals = totals;   // { totalAmount, matchedAmount, unmatchedAmount }
    this.createdAt = createdAt || new Date();
  }
}

// ===== ReportLine =====
class ReportLine {
  constructor({ id, reportId, invoiceId, paymentId, bankRef, status, note }) {
    this.id = id;
    this.reportId = reportId;
    this.invoiceId = invoiceId || null;
    this.paymentId = paymentId || null;
    this.bankRef = bankRef || null;
    this.status = status;   // 'MATCH' | 'UNMATCH' | 'DUPLICATE'
    this.note = note || '';
  }
}

// ===== Repository =====
const reportRepo = {
  // Lưu report + lines vào DB
  async save(report, lines) {
    // 1️⃣ Lưu report
    const [result] = await pool.query(
      `INSERT INTO reports (period, channel, total_amount, matched_amount, unmatched_amount, created_at)
       VALUES (?, ?, ?, ?, ?, ?)`,
      [
        report.period,
        report.channel,
        report.totals.totalAmount || 0,
        report.totals.matchedAmount || 0,
        report.totals.unmatchedAmount || 0,
        report.createdAt
      ]
    );

    report.id = result.insertId;

    // 2️⃣ Lưu từng line
    for (let i = 0; i < lines.length; i++) {
      const l = lines[i];
      const [lineResult] = await pool.query(
        `INSERT INTO report_lines (report_id, invoice_id, payment_id, bank_ref, status, note)
         VALUES (?, ?, ?, ?, ?, ?)`,
        [
          report.id,
          l.invoiceId,
          l.paymentId,
          l.bankRef,
          l.status,
          l.note
        ]
      );
      l.id = lineResult.insertId;
      l.reportId = report.id;
    }

    return { report, lines };
  }
};

module.exports = { Report, ReportLine, reportRepo };

```

### 3.3.2 Hàm parse sao kê

```js
// services/statementParser.js
function parseCsvStatement(csvText) {
  // Giả sử format: date,amount,providerRef,description
  const lines = csvText.trim().split('\n');
  const header = lines.shift();

  return lines.map(line => {
    const [dateStr, amountStr, providerRef, description] = line.split(',');
    return {
      date: new Date(dateStr),
      amount: Number(amountStr),
      providerRef: providerRef || null,
      description: description || ''
    };
  });
}

module.exports = { parseCsvStatement };

```

### 3.3.3 ReconcileService – logic đối soát cốt lõi

```js
// services/reconcileService.js
const { payments } = require('../models/invoice');
const { Report, ReportLine, reportRepo } = require('../models/report');

async function reconcile(bankTxns, { period, channel = 'ONLINE' }) {
  const lines = [];

  // Lọc payment nội bộ theo channel (vd lọc theo: ONLINE)
  const internalPayments = payments.filter(p => {
    if (channel === 'ALL') return true;
    return p.method === channel;
  });

  let totalAmount = 0;
  let matchedAmount = 0;

  for (const tx of bankTxns) {
    totalAmount += tx.amount;

    // ưu tiên match theo providerRef
    let matches = internalPayments.filter(p => p.providerRef === tx.providerRef);

    // fallback: match theo số tiền
    if (matches.length === 0) {
      matches = internalPayments.filter(p => p.amount === tx.amount);
    }

    if (matches.length === 1) {
      const p = matches[0];
      matchedAmount += tx.amount;
      lines.push(new ReportLine({
        invoiceId: p.invoiceId,
        paymentId: p.id,
        bankRef: tx.providerRef,
        status: 'MATCH',
        note: tx.description
      }));
    } else {
      lines.push(new ReportLine({
        bankRef: tx.providerRef,
        status: 'UNMATCH',
        note: 'Không tìm thấy payment tương ứng'
      }));
    }
  }

  const report = new Report({
    period,
    channel,
    totals: {
      totalAmount,
      matchedAmount,
      unmatchedAmount: totalAmount - matchedAmount
    }
  });

  return reportRepo.save(report, lines);
}

module.exports = { reconcile };

```
