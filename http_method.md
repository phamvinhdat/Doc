# HTTP method

## REST

### REST _(REpresentational State Transfer)_

> `REST` định nghĩa các quy tắc kiến trúc để thiết kế `Web service`, chú trọng vào tài nguyên hệ thống, bao gồm các trạng thái tài nguyên được định danh như thế nào và được truyền tải qua `HTTP`, và được viết bởi nhiều ngôn ngữ khác nhau. Bên cạnh đó còn có các kiến trúc khác như `SOAP` và `WSDL` nhưng gần như đã bị `REST` thay thế bởi vì sự đơn giản và dễ sử dụng.

### Architectural constraints

- **Uniform interface**

  - **Resource identification in requests:** `REST` tập trung xung quanh các `resource`. Mỗi `resource` trong `RESTful` có một định danh duy nhất thông qua một `URI`(`Uniform Resource Identifier`) và định danh này ổn định ngay cả khi `resource` update. Điều này có nghĩa mỗi `resource` muốn expose thông qua `RESTful web service` phải có `URI` riêng.
  - **Resource manipulation through representations:** `Client` không tương tác trực tiếp với `server's resource`. Ví dụ: không cho phép `client` chạy các câu lệnh `SQL`. Thay vào đó `server`cung cấp các đại diện mà nhờ đó `client` có thể thao tác với tài nguyên máy chủ.
  - **Self-Descriptive messages:** Mỗi `message` (i.e. `request/response`) phải bao gồm đủ thông tin cho `receiver` để có thể hiểu nó độc lập.
  - **Hypermedia as the engine of application state:** Thay đổi `application state` bằng các `hyperlink` mà `server` cung cấp.

- **Client-server architecture**

  > `Client` và `server` hoạt động độc lập và tương tác giữa chúng chỉ là `reqest` được tạo bởi `client` và `response` được `server` gửi cho `client` để đáp lại `reqest` mà `client` gửi tới. `Server` chỉ ngồi đó và đợi `reqest` từ `client` đến.

- **Stateless**

  - Phi trạng thái có nghĩa là `server` sẽ không lưu giữ thông tin của `client` mà nó giao tiếp, thông tin hoặc được giữ trên `client` hoặc được chuyển thành trạng thái của tài nguyên. Mỗi `request` lên `server` thì `client` phải đóng gói thông tin đầy đủ để `server` hiểu được.
  - Điểu này đem lại hai lợi ích:
    - Giúp tách biệt `client` ra khỏi sự thay đổi của `server`.
    - Giúp hệ thống dễ phát triển, bảo trì, mở rộng vì không cần tốn công `CRUD` trạng thái của `client`.

- **Cacheable**

  > Dữ liệu mà máy chủ gửi có chứa thông tin về liệu dữ liệu này có `cache` lại hay không. Nếu dữ liệu được `cache` lại nó có thể chứa `version number` giúp `client` biết version nào đã có để không `reqest` lại cùng một dữ liệu, cũng như dữ liệu có hết hạn hay chưa.

- **Layered System**

  > Giữa `client` (gửi `request`) và `server`(gửi `response`) có thể có nhiều `server` ở giữa. Các `server` này có thể cung cấp `security layer`, `caching layer`, `load-balancing layer` hoặc các chức năng khác. Các `layer` này sẽ không ảnh hưởng đến `request` và `response`. `Client` không biết đến sự tồn tại của các lớp này.

- **Code on demand (optional)**

### Method

- GET

  > Dùng để lấy dữ liệu về từ `server`. Câu truy vấn sẽ được đính kèm vào đường dẫn `HTTP request`. `GET` chỉ dùng để lấy dữ liệu từ `server` và không sủa đổi theo bất kì cách nào (mặc dù hoàn toàn có thể làm được). Vì `GET requests` không làm thay đổi `state` của `resource` nên nó được coi là **`safe method`**. `GET APIs` cũng là **`idempotent`** nghĩa là thực hiện nhiều `request` giống nhau thì kết quả trả về luôn như nhau cho đến khi một `API` khác (`POST or PUT`) là thay đổi `state` của `resource`.

  - `Status code`
    - Nếu `resource` tìm thấy trên `server`: `200(OK)`
    - Không tìm thấy: `404(NOT FOUND)`
    - Sai: `400(BAD REQUEST)`
  - Đặc điểm
    - `GET` có thể được `cached`, `bookmark` và lưu trong lịch sử của trình duyệt.
    - `GET` bị giới hạn về chiều dài, do chiều dài của `URL` là có hạn.
    - `GET` không nên dùng với dữ liệu quan trọng, chỉ dùng để nhận dữ liệu.
  - Ví dụ:
    - HTTP GET `http://www.appdomain.com/users`
    - HTTP GET `http://www.appdomain.com/users?size=20&page=5`
    - HTTP GET `http://www.appdomain.com/users/123`
    - HTTP GET `http://www.appdomain.com/users/123/address`

- POST
  > Dùng để tạo một `resource` mới. `POST` không phải `safe method` và cũng không phải `idempotent`, hai `POST request` giống nhau kết quả sẽ dẫn đến hai `resource` khác nhau chứa cùng mọt thông tin (ngoại trừ `resource id`).

  - `Status code`
    - Nếu một `resource` mới được tạo: `201(CREATED)`
    - `Resource` không thể định danh bởi một `URI`: `200(OK)` or `204(NO CONTENT)`
  - Đặc điểm
    - `POST` không thể `cached`(trừ khi được chỉ định), `bookmark` hay lưu trong lịch sử trình duyệt.
    - `POST` không bị giới hạn về độ dài.
  - Ví dụ:
    - HTTP POST `http://www.appdomain.com/users`
    - HTTP POST `http://www.appdomain.com/users/123/accounts`

- PUT
  > Dùng để `update` một `resource` (nếu `resource` không tồn tại thì `API` có thể quyết định tạo mới hoặc không). `PUT` không là `safe method`.

  - `Status code`
    - Nếu một `resource` mới được tạo: `201(CREATED)`
    - Nếu một `resource` bị sửa đổi: `200(OK)` or `204(NO CONTENT)`

  - Đặc điểm: giống `POST`

  - ví dụ:
    - HTTP PUT `http://www.appdomain.com/users/123`
    - HTTP PUT `http://www.appdomain.com/users/123/accounts/456`

  - Khác biệt với `POST`
  
  >|`PUT`|`POST`|
  >|-|-|
  >|Thực hiện trên một `resource` đơn lẻ<br>`PUT /questions/{question-id}`|Thực hiện trên một tập `resource`<br>`POST /questions`|
  >|`Idempotent`|Không `idempotent`|
  >|Thông thường, trong thực tế luôn, luôn luôn sử dụng `PUT` cho `update`|Luôn luôn sử dụng `POST` cho tạo mới|

- DELETE
  > Sử dụng để xóa `resource` (định danh bởi `Request-URI`). `POST` không là `safe method` nhưng là `idempotent`.

  - `Status code`
    - Nếu được tực hiện và `response` bao gồm `entity`: `200(OK)`
    - Nếu nằm trong `queue`: `202 (ACCEPTED)`
    - Nếu được thực hiện và `response` không bao gồm `entity`: `204(NO CONTENT)`
    - Không thấy `resource`: `404(NOT FOUND)`

  - Đặc điểm: `not cacheable`
  - ví dụ:
    - HTTP DELETE `http://www.appdomain.com/users/123`
    - HTTP DELETE `http://www.appdomain.com/users/123/accounts/456`

## Reference
1. [https://medium.com/extend/what-is-rest-a-simple-explanation-for-beginners-part-2-rest-constraints-129a4b69a582](https://medium.com/extend/what-is-rest-a-simple-explanation-for-beginners-part-2-rest-constraints-129a4b69a582)
1. [https://restfulapi.net/http-methods/](https://restfulapi.net/http-methods/)
1. [https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
1. [https://restfulapi.net/rest-put-vs-post/](https://restfulapi.net/rest-put-vs-post/)
1. [https://restfulapi.net/idempotent-rest-apis/](https://restfulapi.net/idempotent-rest-apis/)
