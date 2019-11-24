# Extending KrakenD

![important package](https://www.krakend.io/images/documentation/config-router-proxy-packages.png)

## The important packages

### The config package

- pkg config chứa các cấu trúc cần thiết để mô tả `service`
- `ServiceConfig` định nghĩa toàn bộ `service`. Khỏi tạo nó trước khi sử dụng để đảm bảo toàn bộ tham số được chuẩn hóa và các giá trị mặc định được áp dụng
- `config` cũng định nghĩa một `interface` cho một file config và `parser` dựa trên [viper](https://github.com/spf13/viper)

### The router package

- Chứa một `interface` và một số `implementations` cho `KrakenD` router layer sử dụng `mux` router từ net/http và `httprouter` được bao bọc bởi [gin](https://github.com/gin-gonic/gin) framework
- `Router layer` chịu trách nhiệm cho thiết lập HTTP(s) service, liên kết các `endpoints` được define ở `ServiceConfig` struct và chuyển đổi các HTTP request thành các proxy request trước khi đưa nó vào layer bên trong (proxy). Một khi proxy layer trả về một proxy response, router layer chuyển nó thành một HTTP response thích hợp và gửi nó về cho User.
- Layer này có thể dễ dàng mở rộng để sử dụng bất kì HTTP router, framework hoặc middleware nào.

### The proxy package

- Là nơi chứa hầu hết các thành phần và tính năng của KrakenD. Nó định nghĩa hai `interface` quan trọng:
    - Proxy chức năng chuyển đổi context nhất định và request thành response
    - Middleware là chức năng chấp nhận một hoặc nhiều proxy và trả về một proxy duy nhất bao bọc chúng.
- Layer này chuyển request nhận được từ layer ngoài (router) thành một hoặc một số request đền backend service, sử lí các response và trả về một response duy nhất

## Mở rộng KrakenD

Viết `plugin` hoặc viết `middleware`, có ba lựa chọn:

1. Viết và inject một `plugin` vào router layer
2. Viết và inject một `plugin` vào proxy layer
3. Viết một `middleware` và compile KrakenD với nó

Cách lựa chọn:

- Modify request trước khi KrakenD bắt đầu process ?
  - Chọn router plugin

- Thay đổi cách mà krakenD tương tác với backend service ?
  - Chọn proxy plugin

- Thay đổi bên trong cuả pipe, thêm tool, tích hợp, ... ?
  - Viết một custom middleware

the sequence of a request-response is as follows:

1. end-user gửi HTTP request đến KrakenD
2. Router chuyển HTTP request thành một số proxy request - HTTP
3. proxy pipe lấy data cho tất cả request, thao tác, tập hợp và trả về context 
4. Router chuyển proxy response thành HTTP response

### Writing and injecting plugins

- The http handler (router)
- The http client (proxy)

![diagram](https://www.krakend.io/images/documentation/krakend-plugins.png)
