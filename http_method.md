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

- HEAD
  - Về cơ bản, HEAD giống hệt như GET trừ việc nó không gửi dữ liệu trong response. Nói cách khác, HEAD response chỉ trả về header.
  - Method này có thể được sử dụng để nhận về các metadata của API được chọn mà không cần trả về dữ liệu. Vì vậy, chúng thường được sử dụng để kiểm tra môt URI xem còn hiệu lực hay không, còn truy cập được hay không hoặc có thay đổi nào hay không.
  - Ví dụ:
      - `curl -X HEAD https://example.org -i`
      ```
      ```
- `PATCH` 
  - `PATCH` một phần tuongw tự như PUT, dùng để thay đổi resource ở phía server. Tuy nhiên, thay vì thay đổi toàn bộ resource, `PATCH` thay đổi một phần (partial) resource trên server.
  - Một số thuộc tính của PATCH:

  Property | Value
  ---------|--------
  Request has body | YES 
  Successful response has body | YES
  
- `OPTIONS`
  - HTTP `OPTIONS` method được sử dụng để mô tả những method có thể được dùng cho resource. Client có thể chỉ ra URL cho `OPTIONS` hoặc dùng dấu * cho toàn bộ server.
  - Ví dụ:
    - `curl -X OPTIONS https://example.org -i `
    ```
    HTTP/2 200
    allow: `OPTIONS`, GET, HEAD, POST
    cache-control: max-age=604800
    content-type: text/html; charset=UTF-8
    date: Mon, 08 Jul 2019 04:37:14 GMT
    expires: Mon, 15 Jul 2019 04:37:14 GMT
    server: EOS (vny006/0450)
    content-length: 0
    ```

- Response status code
  1. 1xx Informational response
    
      Code | Name | Description
      ------|-----|------------
      100 | Continue | Server gửi mã code này để thông báo đồng thuận cho client gửi tiếp request body, ví dụ như body của POST.

  2. 2xx Success
  
      Code | Name | Description
      ------|-----|------------
      200 | OK | Response tiêu chuẩn cho một HTTP request thành công.Response thực sự sẽ tuỳ thuộc vào request. Nếu response cho một GET request, response sẽ chứa đối tượng tương ứng với request, còn trong POST request, response sẽ chứa đối tượng hoặc kết quả của request.
      201 | Created | Request đã hoàn thành, tạo mới resource thành công.
      202 | Accepted | Yêu cầu được chấp nhận xử lý nhưng có thể chưa hoàn thành, request có thể được xử lý hoặc không và có thể không cho phép khi đang xử lý.
      203 | Non-Authoritative Information (since HTTP/1.1) | Server đóng vai trò là một proxy nhận response 200 OK từ server gốc nhưng trả về một phiên bản đã có thay đổi.
      204 | No Content | Server hoàn thành xử lý request nhưng không trả về dữ liệu gì cả.
      205 | Reset Content | Server xử lý process thành công nhưng không trả về dữ liệu. Tuy nhiên, khác biệt với 204 No Content là 205 Reset Content  yêu cầu  client refresh trang xem thông tin.

  3. 3xx Redirection

      Code | Name | Description
      ------|-----|------------
      300 | Multiple Choices | Đưa ra nhiều tuỳ chọn cho resource mà client có thể chọn.
      301 | Moved Permanently | Request được thực hiện và những request sau đó đến URI này đề được chuyển hướng do trình duyệt cache lại status.
      302 | Found (Previously "Moved temporarily") | Thông báo cho client redirect đến một URL khác.
      303 | See Other (since HTTP/1.1) | Response trả về có thể được tìm thấy ở một URI khác thông qua GET method. Khi nhận được response sau khi gửi POST(PUT/DELETE), client giả định là request đã được thực hiện và thực hiện một GET request khác đến URI được trả về.
      307 | Temporary Redirect (since HTTP/1.1) | Request sẽ được lặp lại với một URI khác. Tuy nhiên, request method sẽ không thay đổi. Ví dụ một POST request sẽ được redirect sang một POST request khác.
      308 | Permanent Redirect (RFC 7538) | Tương tự 301 nhưng không thay đổi method giống 307.

  4. 4xx Client Errors

      Code | Name | Description
      ------|-----|------------
      400 | Bad Request | Server không xử lý request do lỗi của client. Ví dụ như URI quá dài, sai cú pháp, ...
      401 | Unauthorized (RFC 7235) | Giống với 403 Forbidden, đặc biệt được sử dụng khi authentication được yêu cầu và thất bại hoặc không được cung cấp.
      403 | Forbidden | Client gửi một request hợp lệ, nhưng server từ chối thực hiện. Nguời dùng không đủ quyền thực hiện request.
      404 | Not Found | Resource client cần không có sẵn tuy nhiên sẽ có thể có trong tương lai. Những request sau này vẫn được chấp nhận.
      408 | Request Timeout | Server đợi quá lâu cho request từ client.

  5. 5xx Server Error

       Code | Name | Description
      ------|-----|------------
      500 | Internal Server Error | Thông báo chung khi server gặp lỗi không mong muốn.
      501 | Not Implemented | Server không nhận ra được request hoặc chưa implement request đó.
      502 | Bad Gateway | Server đóng vai trò gateway hoặc proxy nhận được response không hợp lệ từ server.
      503 | Service Unavailable | Server không thể xử lý request do quá tải hoặc đang bảo trì. Thông thường, đây là trạng thái tạm thời.
      504 | Gateway Timeout | Server đóng vai trò gateway hoặc proxy không nhận được response kịp lúc từ server.
## Reference
1. [https://medium.com/extend/what-is-rest-a-simple-explanation-for-beginners-part-2-rest-constraints-129a4b69a582](https://medium.com/extend/what-is-rest-a-simple-explanation-for-beginners-part-2-rest-constraints-129a4b69a582)
1. [https://restfulapi.net/http-methods/](https://restfulapi.net/http-methods/)
1. [https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
1. [https://restfulapi.net/rest-put-vs-post/](https://restfulapi.net/rest-put-vs-post/)
1. [https://restfulapi.net/idempotent-rest-apis/](https://restfulapi.net/idempotent-rest-apis/)
1. [rfc7231](https://tools.ietf.org/html/rfc7231#section-4.3)
1. [https://en.wikipedia.org/wiki/List_of_HTTP_status_codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
