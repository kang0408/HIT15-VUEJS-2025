### 1. API, RESTful API, HTTP Method, Cấu trúc URL

#### 1.1. API là gì?

- API (Application Programming Interface) là giao diện lập trình ứng dụng, cho phép các ứng dụng tương tác với nhau.

- API giúp các ứng dụng frontend (React, Angular, Vue, ...) tương tác, trao đổi dữ liệu với các ứng dụng backend (Node.js, Java, Python, ...).

- API hoạt động dựa trên mô hình Client-Server, trong đó:

  - Client (trình duyệt, ứng dụng di động, ...) gửi yêu cầu đến Server.
  - Server: xử lý yêu cầu, trả về dữ liệu cho Client. (dữ liệu có thể là JSON, XML, HTML, ...)

  ![alt text](img/week-6-fetch-api/image.png)

#### 1.2. RESTlul API

- REST (Representational State Transfer) là một kiến trúc thiết kế API giúp ứng dụng giao tiếp với nhau thông qua giao thức HTTP.
- RESTful API là API tuân thủ theo các nguyên tắc của REST.
- REST: là một dạng chuyển đổi cấu trúc dữ liệu, một kiểu kiến trúc để viết API. Nó sử dụng phương thức HTTP đơn giản để tạo cho giao tiếp giữa các máy. Vì vậy, thay vì sử dụng một URL cho việc xử lý một số thông tin người dùng, REST gửi một yêu cầu HTTP như GET, POST, DELETE … đến một URL để xử lý dữ liệu.
- Đặc điểm của RESTful API:
  - Sử dụng HTTP Method (GET, POST, PUT, PATCH, DELETE) để thực hiện các thao tác CRUD (Create, Read, Update, Delete) trên dữ liệu.
  - Sử dụng URL để xác định tài nguyên (resource) cần thao tác.
    - GET /users: Lấy danh sách người dùng.
    - POST /users: Tạo mới người dùng.
    - GET /users/:id: Lấy thông tin người dùng theo ID.
    - PUT /users/:id: Cập nhật thông tin người dùng theo ID.
    - DELETE /users/:id: Xóa người dùng theo ID.
  - Sử dụng JSON làm định dạng dữ liệu chính.
    ```json
    {
      "id": 1,
      "name": "Nguyen Van A",
      "email": "nguyenvana@gmail.com",
      "phone": "0123456789",
      "address": "Ha Noi"
    }
    ```

#### 1.3. HTTP Method

- HTTP Method (phương thức HTTP) là các phương thức được sử dụng để giao tiếp giữa client và server.
- Mỗi phương thức có một chức năng riêng, phù hợp với mục đích sử dụng.
- Các phương thức HTTP phổ biến:
  - GET: Lấy dữ liệu
  - POST: Gửi dữ liệu mới
  - PUT: Cập nhật toàn bộ dữ liệu
  - PATCH: Cập nhật một phần dữ liệu
  - DELETE: Xóa dữ liệu
  - HEAD, OPTIONS, CONNECT, TRACE

#### 1.4. Cấu trúc URL

- URL (Uniform Resource Locator) là địa chỉ của một tài nguyên duy nhất trên web.
- Mỗi URL hợp lệ sẽ trỏ tới một tài nguyên duy nhất, có thể là trang HTML, tài liệu CSS, JSON, hình ảnh, video...

- Một URL (Uniform Resource Locator) phổ biến thường có cấu trúc như sau:

  ```
  scheme://username:password@host:port/path?query#fragment
  ```

  Dưới đây là mô tả từng phần:

  | Thành phần                       | Mô tả                                                                                           |
  | -------------------------------- | ----------------------------------------------------------------------------------------------- |
  | **scheme**                       | Giao thức dùng để truy cập tài nguyên, thường là `http`, `https`, `ftp`...                      |
  | **username:password** (tùy chọn) | Thông tin xác thực (hiếm khi được dùng công khai vì lý do bảo mật).                             |
  | **host**                         | Tên miền hoặc địa chỉ IP của server, ví dụ: `www.example.com` hoặc `192.168.1.1`                |
  | **port** (tùy chọn)              | Cổng để kết nối, ví dụ `:80` cho HTTP, `:443` cho HTTPS.                                        |
  | **path**                         | Đường dẫn đến tài nguyên cụ thể trên máy chủ, ví dụ: `/products/item1`                          |
  | **query** (tùy chọn)             | Tham số truy vấn (query string), bắt đầu bằng `?`, ví dụ: `?id=123&sort=asc`                    |
  | **fragment** (tùy chọn)          | Mã định danh để di chuyển đến một phần cụ thể trong trang, bắt đầu bằng `#`, ví dụ: `#section2` |

- Ví dụ minh họa:

  ```
  https://www.example.com:443/products/item1?id=123&sort=asc#reviews
  ```

  - **scheme**: `https`
  - **host**: `www.example.com`
  - **port**: `443`
  - **path**: `/products/item1`
  - **query**: `id=123&sort=asc`
  - **fragment**: `reviews`

![alt text](img/week-6-fetch-api/image-1.png)

### 2. Call API / Fetch API

- Call API nghĩa là gửi một yêu cầu (request) đến một API (Application Programming Interface) để lấy hoặc gửi dữ liệu.
- Fetch API là sử dụng hàm `fetch()` được cung cấp bởi Javascript từ ES6 để thực hiện call API

#### 2.1. Các thành phần đi kèm

```js
fetch(url, {
  method: 'GET' | 'POST' | 'PUT' | 'DELETE' | ...,
  headers: {
    'Content-Type': 'application/json',
    // các headers khác (auth, token...)
  },
  body: JSON.stringify(data), // chỉ dùng khi method là POST, PUT,...
})
```

| Thành phần | Ý nghĩa                                                                                   |
| ---------- | ----------------------------------------------------------------------------------------- |
| `method`   | Phương thức HTTP (GET, POST, PUT, DELETE...)                                              |
| `headers`  | Các thông tin gửi kèm, thường để mô tả loại dữ liệu (VD: `Content-Type`, `Authorization`) |
| `body`     | Dữ liệu gửi lên server (chỉ áp dụng cho POST, PUT), thường là JSON hoặc form-data         |

- Ví dụ:

```js
fetch("https://api.example.com/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Authorization: "Bearer your_token_here",
  },
  body: JSON.stringify({
    name: "Alice",
    age: 25,
  }),
})
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.error("Lỗi:", error));
```

#### 2.2. Thực hành

##### 1. **GET** – Lấy dữ liệu từ server

- Ví dụ 1:

  ```js
  fetch("https://api.example.com/users")
    .then((res) => res.json())
    .then((data) => console.log(data));
  ```

- Ví dụ 2:

  ```js
  fetch("https://api.example.com/users?role=admin&page=2", {
    headers: {
      Authorization: "Bearer your_token_here",
    },
  })
    .then((res) => res.json())
    .then((data) => console.log(data));
  ```

  **Giải thích**:

  - Query: `?role=admin&page=2` → lọc user theo vai trò và phân trang.
  - Header: gửi kèm token xác thực người dùng.

##### 2. **POST** – Tạo mới tài nguyên

- Ví dụ 1:

  ```js
  fetch("https://api.example.com/users", {
    method: "POST",
    body: JSON.stringify({ name: "Alice", age: 25 }),
    headers: { "Content-Type": "application/json" },
  });
  ```

- Ví dụ 2:

  ```js
  fetch("https://api.example.com/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: "Bearer your_token_here",
    },
    body: JSON.stringify({
      name: "Bob",
      age: 30,
    }),
  });
  ```

  👉 **Giải thích**:

  - Gửi kèm token để xác thực người tạo.
  - Gửi body dạng JSON.

##### 3. **PUT** – Cập nhật toàn bộ thông tin

- Ví dụ 1:

  ```js
  fetch("https://api.example.com/users/123", {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ name: "Bob", age: 32 }),
  });
  ```

  **Ý nghĩa**: Ghi đè toàn bộ thông tin user ID = 123.

- Ví dụ 2:

  ```js
  fetch("https://api.example.com/users/123", {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
      Authorization: "Bearer your_token_here",
    },
    body: JSON.stringify({ name: "Bob", age: 35, email: "bob@example.com" }),
  });
  ```

  **Giải thích**:

  - Gửi đầy đủ thông tin mới, vì PUT thường yêu cầu "replace toàn bộ".
  - Cần xác thực qua `Authorization`.

##### 4. **PATCH** – Cập nhật một phần thông tin

- Ví dụ 1:

  ```js
  fetch("https://api.example.com/users/123", {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ age: 28 }),
  });
  ```

  **Ý nghĩa**: Chỉ cập nhật `age` của user 123.

- Ví dụ 2:

  ```js
  fetch("https://api.example.com/users/123", {
    method: "PATCH",
    headers: {
      "Content-Type": "application/json",
      Authorization: "Bearer your_token_here",
    },
    body: JSON.stringify({ isActive: false }),
  });
  ```

  **Giải thích**:

  - Không thay đổi toàn bộ user, chỉ cập nhật `isActive`.
  - Gửi token để xác thực.

##### 5. **DELETE** – Xoá tài nguyên

- Ví dụ:

  ```js
  fetch("https://api.example.com/users/123", {
    method: "DELETE",
  });
  ```

  **Ý nghĩa**: Xoá user có ID = 123.

- Ví dụ 2:

  ```js
  fetch("https://api.example.com/users/123", {
    method: "DELETE",
    headers: {
      Authorization: "Bearer your_token_here",
    },
  });
  ```

  **Giải thích**: Chỉ admin/owner mới được xoá → cần token.

#### [Swagger Document](https://cloth-management-be.onrender.com/api-docs/#/)
