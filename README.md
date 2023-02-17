- Tìm hiểu cơ chế hoạt động của Nodejs bao gồm:
    + Call stask: vùng nhớ đặc biệt trên chip máy tính nhằm để phục vụ thực thi các dòng lệnh (cụ thể ở đây là các hàm). Stack là hàng đợi theo kiểu LIFO (vào sau ra trước)
    + Heap      : vùng nhớ để chứa kết quả tạm thời để thực thi các hàm trong stask
    + Callback Queue/ Message Queue: khi các dòng lệnh cần thời gian chờ, ta sẽ khai báo các function callback xử lý sau khi dòng lệnh đó đã hoàn thành, thì các task đó sẽ được đẩy vào đây. Queue là hàng đợi theo kiểu FIFO (vào trước ra trước)
    + Event Loop: có thể giải thích nó là vòng lặp vô tận, và chỉ 1 công việc duy nhất là lấy các task từ Call Stack hoặc Callback Queue. Đầu tiên sẽ xử lý Call Stack trước, sau khi Call Stack trống thì nó sẽ kiểm tra Callback Queue để thực hiện.
1. Tìm hiểu sự khác nhau giữa blocking và non-blocking.
- blocking (đồng bộ) thực hiện lần lượt từng tác vụ, tiến trình sẽ chờ đợi cho đến khi tác vụ được hoàn tất mới thực hiện tác vụ tiếp theo.
- non-blocking (bất đồng bộ) thực hiện cùng lúc các tác vụ, tác vụ nào cần thời gian chờ sẽ được đẩy qua Callback Queue để xử lý, sau khi hoàn thành thì sẽ đẩy vào Call Stack để thực thi.


2. Tìm hiểu khái niệm và cơ chế hoạt động của Event-Based.
- Tìm hiểu cơ chế hoạt động của Nodejs Api trong: https://nodejs.org/docs/latest-v16.x/api/
bao gồm các khái niệm: Events, Http, Buffer, Stream, Async, Promise.
    + Cấu trúc của Nodejs gồm js và c/c++ trong đó:
        - Nodejs Api được viết bằng js, các thư viện hay gọi là module. Những module này giúp tương tác với các thành phần Nodejs bên trong bằng Javascript.
        - ...
    + Cơ chế hoạt động của Nodejs
        - Nodejs cấu trúc từ Javascript và C/C++ nên về bản chất ngôn ngữ Javascript không thực hiện những công việc server-side như đọc file, database. Nó uỷ quyền cho phần core thực hiện bởi C/C++.
        - Javascript là single thread nên khi có request đến server, nó đẩy event vào Event Queue, event loop sẽ nhận lần lượt các event để xử lý. Nếu event không Block lại chương trình quá lâu, nó sẽ tự xử lý rồi trả response lại. Những request tốn nhiều thời gian sẽ được event loop đẩy qua Thread Pool để thực hiện bằng C++. Khi thread Pool thực hiện xong, Nodejs đẩy callback của event đó về Event Queue để xử lý.
    + Promise
        - cơ chế của nó giúp thực hiện các tác vụ đồng bộ hơn và tránh rơi vào tình trạng callback hell (các callback lồng nhau quá nhiều)
        - một Promise có 1 trong 3 trạng thái: 
            + pending: trạng thái khởi tạo
            + fulfilled: có nghĩa thao tác đã hoàn thành thành công
            + rejected: có nghĩa thao tác thất bại
        - Khi sử dụng callback function, ta phải viết callback xử lý tính toán ngay (khi mà thực hiện các thao tác bất đồng bộ) nên khi xảy ra quá nhiều thao tác bất đồng bộ diễn ra, sẽ dẫn đến tình trạng các callback function lồng vào nhau. Còn Promise, nó làm 1 bản cam kết thực hiện trong tương lai, ta không cần phải viết xử lý then ngay lập tức mà có thể để promise object lại đến khi sau này cần mới thực hiện.
        - Promise có thể hiểu là 1 biến chứa kết quả cuộc 1 lần thực hiện bất đồng bộ. Nó khác với các xử lý callback là ta có thể mang vác cái biến này đi khắp nơi trong code mà ta thấy phù hợp. Tại đó gọi hàm then chứa callback xử lý.
        - khi xử lý quá nhiều lệnh bất đồng bộ thì sao. lúc đó mỗi bất đồng bộ là một object promise, ta sẽ gom các promise này lại rồi xử lý tại promise all
    + Async
        - bản chất khi ta sử dụng Async/Await chính là ta sử dụng gián tiếp Promise, khi async function return thì nó sẽ trả về 1 Promise
        - "Async/Await" và "Promises" là hai cách để giải quyết việc thực hiện tác vụ bất đồng bộ trong Node.js.

        - Khi nào sử dụng Async/Await:

            + Khi bạn muốn viết mã dễ đọc và dễ hiểu, và muốn tạo một đồng bộ hóa trong một dạng tự nhiên, giống như viết mã tuần tự.
            + Khi bạn cần trả về một giá trị từ một hàm bất đồng bộ.
            + Khi bạn muốn sử dụng try/catch để xử lý lỗi.
        - Khi nào sử dụng Promises:

            + Khi bạn muốn xử lý một số tác vụ liên tục và chờ cho tất cả các tác vụ được hoàn thành trước khi thực hiện một tác vụ tiếp theo.
            + Khi bạn muốn chạy nhiều tác vụ song song và chờ cho tất cả các tác vụ hoàn thành.
        - Tổng quan, Async/Await có thể là sự lựa chọn tốt hơn nếu bạn muốn viết mã dễ đọc, trong khi Promises là tốt hơn nếu bạn muốn chạy nhiều tác vụ song song.
    + Stream
        - là một chuỗi dữ liệu đi từ điểm này đến điểm khác.
        - có 4 loại stream: readable stream, writeable stream, duplex stream, transform stream
    + Buffer 
        - là một đối tượng được sử dụng để lưu trữ dữ liệu nhị phân. Nó cho phép làm việc với các chuỗi byte và dữ liệu nhị phân trong các quá trình xử lý dữ liệu của bạn. Một buffer trong Node.js có thể được khởi tạo từ một chuỗi, một mảng hoặc một buffer khác, và nó cung cấp các phương thức để thao tác với dữ liệu bên trong.
    + Http
        - là một giao thức truyền tải dữ liệu giữa máy khách và máy chủ trên mạng. Trong Node.js, module `http` được sử dụng để tạo một máy chủ HTTP, và không giới hạn chỉ dành riêng cho trình duyệt. Bằng cách sử dụng module `http` này cho phép ta xử lý các yêu cầu và phản hồi HTTP một cách dễ dàng và linh hoạt trong Node.js.
    + Events
        - là một cơ chế xử lý sự kiện trong Node.js, giúp cho việc lập trình trở nên dễ dàng hơn bằng cách kích hoạt các hàm xử lý sự kiện khi các sự kiện được phát sinh.
        - module events được sử dụng để tạo ra các đối tượng sự kiện. Đối tượng sự kiện này sẽ có các phương thức để đăng ký (thêm) các hàm xử lý sự kiện và phát sinh các sự kiện tương ứng. Khi sự kiện xảy ra, các hàm xử lý đã được đăng ký sẽ được gọi để xử lý sự kiện đó.
        - Các sự kiện phổ biến trong Node.js bao gồm error, data, end cho stream hoặc request, response cho HTTP server.
- Tìm hiểu cách thức hoạt động của webpack.
    + Webpack là một trình biên dịch và gói hợp nội dung cho JavaScript và các tài nguyên web khác, như HTML, CSS, hình ảnh và font. Nó hoạt động bằng cách đọc các tệp trong dự án và tạo ra một hoặc nhiều tệp đầu ra, tất cả các tệp đầu ra đó là các tệp đã được biên dịch và gói hợp nội dung.
- Tìm hiểu cấu trúc thư mục của Nodejs project.
    + Cấu trúc thư mục của một Node.js project không được yêu cầu theo bất kỳ chuẩn nào. Tuy nhiên, để dễ quản lý và hiểu quả hơn, một số dự án sử dụng các cấu trúc thư mục chuẩn sau đây:
        ```
        ├── node_modules/
        ├── public/
        │   ├── css/
        │   ├── img/
        │   └── js/
        ├── src/
        │   ├── config/
        │   ├── controllers/
        │   ├── models/
        │   ├── routes/
        │   └── views/
        ├── tests/
        ├── .gitignore
        ├── package.json
        ├── README.md
        └── server.js

        ```
        
        - node_modules/: Thư mục chứa các module và package mà project sử dụng, được cài đặt thông qua npm.
        - public/: Thư mục chứa các tài nguyên tĩnh, như các file CSS, JavaScript và hình ảnh, được sử dụng bởi trang web.
        - src/: Thư mục chứa mã nguồn của ứng dụng. Thường bao gồm các thư mục config/ để cấu hình ứng dụng, controllers/ để xử lý các yêu cầu HTTP, models/ để định nghĩa các đối tượng trong ứng dụng, routes/ để định nghĩa các đường dẫn URL và chuyển hướng đến các controller tương ứng và views/ để chứa các file view của ứng dụng.
        - tests/: Thư mục chứa các test cho ứng dụng.
        - .gitignore: File chứa danh sách các tệp và thư mục sẽ bị bỏ qua trong quá trình commit và push lên Git.
        - package.json: File chứa các thông tin cấu hình cho ứng dụng, bao gồm các package yêu cầu, phiên bản ứng dụng, tác giả, phụ thuộc và scripts.
        - README.md: File chứa các hướng dẫn và thông tin cho người dùng.
        - server.js: File chứa mã nguồn để khởi tạo và chạy ứng dụng.
- Tìm hiểu cách thức hoạt động của package.json và node_modules.
    + package.json là một tệp trong một dự án Node.js, nó chứa thông tin về dự án, các thư viện phụ thuộc và các lệnh để quản lý dự án. Nó giúp cho việc quản lý các phụ thuộc và các phiên bản của chúng trở nên dễ dàng hơn.
    + node_modules là thư mục chứa các gói (packages) đã được cài đặt cho dự án hiện tại. Khi bạn sử dụng lệnh npm install để cài đặt một gói, nó sẽ được tải về và lưu trữ trong thư mục node_modules. Khi bạn require một gói, Node.js sẽ tìm kiếm gói đó trong thư mục node_modules trước khi tìm kiếm trong các thư mục khác.
