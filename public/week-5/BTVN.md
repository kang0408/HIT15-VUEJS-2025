#### Bài 1: Quản lý công việc

Bạn nhận được một nhiệm vụ vô cùng cam go và đầy thách thức đến từ tên Boss xấu tính xấu nết. Tên Boss này bắt bạn phải clone website này 👉 [Quản lý công việc](https://simple-todo-app-cyan.vercel.app/) với mức độ giống và hoàn thiện tối thiểu là 90%. Bạn không thể hoàn thành? Bạn bị PHẠT!

##### Sau đây là các yêu cầu chính của website trên, hãy tuân thủ đúng theo nó:

**1. Thêm công việc mới**

- Khi người dùng bấm vào dấu **`+`** ở góc dưới bên phải màn hình sẽ hiển thị một trường nhập thông tin công việc, khi nhấn nút **Thêm** sẽ hiển thị thông báo thành công.
- Không cho phép thêm công việc trống
- Bấm nút **Hủy** thì sẽ tắt giao diện thêm mới công việc.

**2. Hiển thị danh sách công việc**

Mỗi công việc sẽ hiển thị:

- Nội dung. Nội dung quá dài sẽ hiển thị là dấu `...`
- Một ô checkbox để tích hoàn thành hoặc hủy hoàn thành
- Một nút 👁️. Khi nhấn vào sẽ xem hiển thị thông tin chi tiết nội dung công việc
- Một nút 🗑️. Khi nhấn vào sẽ xóa công việc khỏi danh sách

Khi không có công việc nào trong danh sách, hiển thị thông báo như trên website

**3. Tìm kiếm và lọc theo trạng thái**

- **Tìm kiếm**:
  Có một trường nhập nội dung công việc và tìm công việc tương ứng với nội dung đã nhập. Nhập tới đâu tìm kiếm tới đó
- **Lọc theo trạng thái:**
  Có ba chế độ lọc:

  - Tất cả: Hiển thị toàn bộ công việc.
  - Đã hoàn thành: Chỉ hiển thị các công việc đã đánh dấu hoàn thành.
  - Chưa hoàn thành: Chỉ hiển thị công việc chưa hoàn thành.

- **Lưu ý:**
  - Khi tìm kiếm không chỉ có thể tìm kiếm trong toàn bộ danh sách công việc mà có thể tìm kiếm trong cả danh sách đã được lọc theo trạng thái
  - Yêu cầu này ứng dụng kiến thức về `computed`

**4. Dashboard**

- Dashboard sẽ hiển thị tổng số công việc, công việc đã hoàn thành, công việc chưa hoàn thành theo dạng cột

- Sử dụng `watch` để theo dõi sự thay đổi của danh sách công việc, từ đó tính toán được các thông số yêu cầu ở trên

**5. Lưu dữ liệu vào localStorage (Tự tìm hiểu)**

- Khi danh sách công việc thay đổi (thêm, xóa, cập nhật trạng thái), dữ liệu sẽ được lưu vào localStorage.

- Khi tải lại trang, dữ liệu từ localStorage được load lại để hiển thị.

**6. Chia nhỏ component**
Trước khi đến yêu cầu thứ 6 này, hãy đảm bảo rằng các yêu cầu phía trên đã chạy ổn định. Sau đó, hãy commit những thay đổi đó rồi mới được phép chuẩn qua bước này để thuận tiện cho việc đánh giá sau này

Các component yêu cầu:

- Component hiển thị thông báo `Message.vue`
- Component hiển thị thống kế công việc `Dashboard.vue`

Sau đó hãy sử dụng và truyền props phù hợp cho các component này

##### 7. Các yêu cầu ngoài lề:

- Tổ chức thư mục, format code cẩn thận, sạch, rõ ràng, cụ thể, thống nhất
- Viết comment ngắn giải thích mục đích của biến đó làm gì, hàm đó làm gì
- Website phải có icon và tên website tương tự như mẫu
- Deploy website bằng Vercel, sau đó gửi vào nhóm Vue link Github và Vercel

##### Hãy trải nghiệm sâu website được cung cấp để hiểu rõ hơn yêu cầu của Boss

##### Mọi thắc mắc hãy liên hệ HOTLINE 👉 <a>ZALO - 0386907498 - Trần Danh Khang</a> để được giải đáp thắc mắc

#### Bài 2: Weather Application

Tên Boss xấu tính không chỉ yêu cầu một website, hắn còn bắt bạn tiếp tục hoàn thành website `Weather App` bỏ dở trước đó. Hắn rất giận bạn! Vì hắn bảo bạn là "LƯỜI THỐI RA!". Nhưng tôi không tin điều đó, tôi tin bạn rất là chăm chỉ học và làm bài tập.

Sau đó chính là yêu cầu tiếp theo của website này

**1. Weather API và Country API**

```js
http://api.weatherapi.com/v1/forecast.json?q=${name.value}&days=7&key=${API_KEY}
```

Đây chính là API để lấy ra thông tin thời tiết cho 7 ngày sắp tới, những tham số cần chú ý ở đây đó là:

- `name`: Đây chính là tên nước bạn nhập vào ô input. Ví dụ `Việt Nam`
- `API_KEY`: Đây là key mà api yêu cầu để có quyền truy cập tới API. Tôi sẽ cung cấp nó cho bạn.

  ```js
  const API_KEY = `07ca3773bc5d4dc29c373649251402`;
  ```

Hãy call api để lấy ra các thông mà chúng ta cần. Trong data api trả về ta cần lấy những thông tin như sau và hiển thị lên màn hình:

- Địa điểm - `location`
- Thời tiết hiện tại - `current`
- Thời tiết 7 ngày tới - `forecast.forecastday`.

Tiếp đây là API giúp ta lấy được hình ảnh lá cờ theo quốc gia

```js
`https://restcountries.com/v3.1/name/${country}`;
```

API này yêu cầu tham số là tên `country`. Sau khi call API thành công sẽ trả về 1 mảng là thông tin của đất nước đó. Để lấy ra được ảnh lá cờ, bạn hãy tìm tới thuộc tính `flags.png` và hiển thị lên màn hình

**2. Chia nhỏ component**
Trước khi đến yêu cầu thứ 2 này, hãy đảm bảo rằng các yêu cầu phía trên đã chạy ổn định. Sau đó, hãy commit những thay đổi đó rồi mới được phép chuẩn qua bước này để thuận tiện cho việc đánh giá sau này

Các component yêu cầu:

- Component hiển thị thông tin thời tiết `WeatherInfor.vue`
- Các component nhỏ hiển thị thông tin từng ngày `WeatherDay.vue`

Sau đó hãy sử dụng và truyền props phù hợp cho các component này

##### 7. Các yêu cầu ngoài lề:

- Tổ chức thư mục, format code cẩn thận, sạch, rõ ràng, cụ thể, thống nhất
- Viết comment ngắn giải thích mục đích của biến đó làm gì, hàm đó làm gì

##### Mọi thắc mắc hãy liên hệ HOTLINE 👉 <a>ZALO - 0386907498 - Trần Danh Khang</a> để được giải đáp thắc mắc

##### Vì một vài vấn đề liên quan tới API của bên thứ 3 không thể chạy trên Vercel, mong rằng bị cáo làm việc tích cực với số HOTLINE được cung cấp
