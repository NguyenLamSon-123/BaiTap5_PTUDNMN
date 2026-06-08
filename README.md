# BaiTap5_PTUDNMN
# Nguyễn lam Sơn_K225480106076
# bt5:
- lý thuyết: <br>
      + docker là gì? <br>
      + các keyword được sử dụng trong docker-compose.yml <br>
      để mô tả 1 service, network, volume,... <br>
      liệt kê + ý nghĩa của từ khoá đó + ví dụ minh hoạ <br>
      + ưu điểm khi triển app sử dụng docker là gì? <br>
      + dùng docker: tạo app, test app OK trên laptop cá nhân <br>
      giờ muốn triển khai app này trên máy chủ thật ko có internet <br>
      thì các bước cần làm là? <br>
  - thực hành áp dụng: APP MONITOR + ALERT DATA REALTIME <br>
    sử dụng docker compose có nhiều serivce <br>
    và các thành phần cần thiết để tạo thành ứng dụng: <br>
     + nodered liên tục lấy dữ liệu từ nguồn nào đó (chứng khoán, thời tiết, giá vàng,...) <br>
       nguồn thực tế, số liệu luôn động sau thời gian ngắn <br>
     + nodered lưu trữ dữ liệu vào 2 database: mariadb để lưu giá trị tức thời <br>
       lưu lịch sử vào influxdb <br>
     + sử dụng grafana để trực quan hoá dữ liệu: vẽ biểu đồ <br>
     + sử dụng nginx để làm webserver <br>
       chạy 1 trang web html+js+css làm front-end <br>
       js: lấy dữ liệu tức thời trong mariadb qua (ajax | socket) <br>
           gọi api (api tự build bằng Flask giống bt1) <br>
           api trả về giá trị tức thời trong mariadb <br>
           hiển thị lên web, auto hiển thị số mới khi thay đổi <br>
       sử dụng iframe để gọi grafana <br>
       hiển thị biểu đồ dữ liệu lịch sử của thông số đã lưu <br>
     + QUAN SÁT DỮ LIỆU LỊCH SỬ => GIÁ TRỊ BẤT THƯỜNG <br>
       (VD MIỀN A..B: OK, DƯỚI A: ALERT LOW, TRÊN B: ALERT HIGH) <br>
     + nodered: kết hợp bot Telegram <br>
       khi dữ liệu not OK, thì gửi tin nhắn từ bot => group trên telegram <br>
       group đã add bot vào: (nhóm đã có 2 người), add thêm 1875746636 thành 3 người <br>
       mỗi khi bot gửi dữ liệu vào nhóm: mọi member of group đều nhận đc <br>
       nội dung alert: tường minh, có value gây alert <br>

     xuất tất cả các container ra file nén. <br>
     xoá mọi container đang chạy <br>
     load lại các container  từ file nén để khôi phục các container đã xoá** <br>
========= <br>
quá trình làm: chụp ảnh lại, mô tả cho ảnh <br>
  lưu vào trong github => paste link access public của repo: vào file excel online <br>
===================== <br>

cả 5 bài tập: <br>
biên tập lại xíu để phù hợp với bản print <br>
đóng quyển, ko cần bìa xanh, ko cần giấy bóng kính <br>
header+footer của các trang giấy: có tên + masv, bài tập lớp Môn, số trang <br>
trang 1 có đầy đủ thông tin cá nhân. <br>

lưu ở bm để chấm điểm.** <br>
# PHẦN 1: LÝ THUYẾT (Viết vào báo cáo) <br>
Bạn có thể copy nội dung này vào file báo cáo Word/Google Docs của bạn. <br>
1. Docker là gì? <br>
Docker là một nền tảng mã nguồn mở giúp các nhà phát triển tự động hóa việc triển khai, phân phối và chạy ứng dụng bên trong các môi trường cô lập gọi là
container. Container chứa mọi thứ ứng dụng cần để chạy (code, runtime, thư viện), giúp nó hoạt động nhất quán trên mọi môi trường. <br>
3. Các keyword trong docker-compose.yml <br>
•	services: Định nghĩa các container (ứng dụng) sẽ chạy. Ví dụ: web, db. <br>
•	image: Chỉ định ảnh (image) Docker nào sẽ được sử dụng để tạo container. Ví dụ: image: mariadb:latest. <br>
•	ports: Ánh xạ cổng (port) từ máy host vào cổng của container. Ví dụ: ports: ["8080:80"] (truy cập port 8080 trên máy tính sẽ vào port 80 của container). <br>
•	volumes: Khai báo nơi lưu trữ dữ liệu vĩnh viễn (persistent data) để khi xóa container, dữ liệu không bị mất. Ví dụ: volumes: ["db_data:/var/lib/mysql"]. <br>
•	networks: Khai báo mạng ảo để các container trong cùng một file compose có thể giao tiếp với nhau qua tên service. <br>
•	environment: Truyền các biến môi trường vào container (thường dùng cho mật khẩu, cấu hình). Ví dụ: MYSQL_ROOT_PASSWORD: root. <br>
4. Ưu điểm khi triển khai app bằng Docker <br>
•	Tính nhất quán: "Chạy được trên máy tôi thì sẽ chạy được trên máy chủ". <br>
•	Triển khai nhanh: Khởi động container chỉ mất vài giây. <br>
•	Cô lập và an toàn: Các ứng dụng chạy độc lập, không xung đột tài viện hệ thống hay thư viện với nhau. <br>
•	Dễ dàng mở rộng và di chuyển. <br>
5. Cách triển khai app lên máy chủ không có internet <br>
•	Bước 1 (Trên laptop có mạng): Dùng lệnh docker save -o myapp.tar myapp_image để xuất các Docker image của ứng dụng ra thành file .tar. <br>
•	Bước 2: Copy file myapp.tar và thư mục code/cấu hình (gồm docker-compose.yml) sang USB. <br>
•	Bước 3: Cắm USB vào máy chủ offline, copy dữ liệu sang ổ cứng. <br>
•	Bước 4 (Trên máy chủ offline): Dùng lệnh docker load -i myapp.tar để nạp image vào Docker. <br>
•	Bước 5: Chạy docker compose up -d để khởi động ứng dụng. <br>
# PHẦN 2: THỰC HÀNH - APP MONITOR + ALERT
# Bước 1: Khởi tạo cấu trúc và docker-compose.yml
# <img width="365" height="250" alt="image" src="https://github.com/user-attachments/assets/5d8f58b1-6731-42fa-825b-7d73ecceb7e1" />
# <img width="623" height="230" alt="image" src="https://github.com/user-attachments/assets/a1a2a750-de8d-4e90-ae2d-6f5f425bfc45" />
# Bước 2: Thiết lập Node-RED (Lấy dữ liệu & Gửi Telegram)
Tìm kiếm và bấm nút Install cho 3 gói sau: <br>
node-red-node-mysql (Dùng cho MariaDB)

node-red-contrib-influxdb (Dùng cho InfluxDB)

node-red-contrib-telegrambot (Dùng để gửi tin nhắn cảnh báo)
# Bước 3: Khởi tạo bảng dữ liệu bên MariaDB
MariaDB đã tạo xong bảng dữ liệu sensor_data và đang thông suốt. <br>
# <img width="590" height="97" alt="image" src="https://github.com/user-attachments/assets/b0cc88dd-3815-4266-8e7e-30b57389cb95" />
# Bước 4: Tự động lấy giá Bitcoin (Cập nhật mỗi 5 giây)
thiết lập Luồng tự động lấy dữ liệu và lưu vào 2 Database.  
1. Kéo và cấu hình Node kích hoạt thời gian (inject) <br>
# <img width="728" height="873" alt="image" src="https://github.com/user-attachments/assets/724d702d-6326-4894-8755-2bc0e468941a" />
2. Kéo và cấu hình Node gọi API (http request) <br>
## URL: Dán chính xác link này vào: https://api.binance.com/api/v3/ticker/price?symbol=BTCUSDT
# <img width="652" height="862" alt="image" src="https://github.com/user-attachments/assets/1da0c0c9-cdba-4919-baed-21eda32492f4" />
3. Kéo và cấu hình Node lọc lấy số giá (function)
# <img width="812" height="860" alt="image" src="https://github.com/user-attachments/assets/91a932d1-1f5b-4f6d-90d6-ae20256433da" />
Sau đó nối dây từ Node Mỗi 5 giây vào đầu vào (chấm tròn bên trái) của node Gọi API Binance. <br>
# Bước 5: Rẽ nhánh lưu vào 2 Database cùng lúc
Từ node Lọc lấy giá số, dữ liệu sẽ được chia làm 2 đường độc lập <br>
Nhánh 1: Lưu vào MariaDB (Giá trị tức thời) <br>
Kéo thêm 1 node function nữa ra màn hình để viết câu lệnh SQL. <br>
Click đúp vào nó và sửa:Name: Điền Tạo lệnh INSERT <br>
# <img width="815" height="405" alt="image" src="https://github.com/user-attachments/assets/7c2c4ff6-3838-4d89-ad8e-5a81eb2475c9" />
Nối dây: * Nối từ node Lọc lấy giá số vào node Tạo lệnh INSERT. <br>
Nhánh 2: Lưu vào InfluxDB (Dữ liệu lịch sử)Kéo node influxdb out (nằm ở mục storage, có hình chiếc xô màu cam sẫm hoặc logo Influx) ra màn hình. <br>
Click đúp vào nó để cấu hình kết nối database lịch sử: <br>
Server: <br>
Bấm vào biểu tượng Cây bút chì để thêm server. <br>
Version: Chọn 1.x (Vì Docker của bạn đang chạy bản 1.8). <br>
Host: Điền chữ influxdb. <br>
Port: Điền 8086. <br>
Database: Điền history_db. <br>
Bấm nút Add ở góc trên. <br>
Measurement: Điền chữ bitcoin_history (Đây là tên bảng lịch sử trong InfluxDB). <br>
Name: Điền history_db <br>
Bấm Done. <br>
Nối dây: Kéo một đường dây trực tiếp từ đầu ra của node Lọc lấy giá số nối thẳng vào cục history_db (influxdb out) vừa tạo. <br>
# <img width="698" height="790" alt="image" src="https://github.com/user-attachments/assets/771db6ec-9a37-44fc-8c47-76b26eab7ff5" />
NHÁNH 3: CẢNH BÁO TELEGRAM TRÊN NODE-RED
1. Tạo Group Telegram theo đúng chuẩn đề bài:

Mở Telegram của bạn lên. Tạo 1 Group mới.

Kéo Bot Telegram của bạn (do bạn tạo qua BotFather) vào Group.

Add thêm tài khoản có ID 1875746636 vào Group (để nhóm có đủ 3 người như đề bài yêu cầu)
# <img width="426" height="900" alt="image" src="https://github.com/user-attachments/assets/651e5109-4447-4487-b410-533861b9a29b" />
2. Kéo thêm node xử lý cảnh báo: <br>
Trong Node-RED, từ đầu ra của node Lọc lấy giá số, bạn kéo thêm 1 nhánh thứ 3 nối vào một node function mới. Đặt tên node này là Logic Cảnh Báo. <br>
# <img width="827" height="870" alt="image" src="https://github.com/user-attachments/assets/e59a3ce0-9fdc-4f28-ba32-c3ef8315ea2e" />
3. Cấu hình Node gửi Telegram:

Kéo node telegram sender ra màn hình và nối từ đuôi node Logic Cảnh Báo vào nó.

Click đúp cấu hình bot bằng Token lấy từ BotFather.
# <img width="941" height="866" alt="image" src="https://github.com/user-attachments/assets/9659ba43-b122-4391-a5f3-740b3c2fe9cf" />
Do em k thể cho add thêm bot vào để lấy id phòng lên em đã thêm 2 node Telegram receiver và debug để xem đoạn chat của tele chả về đâu 
# <img width="530" height="150" alt="image" src="https://github.com/user-attachments/assets/7945c1b6-aba4-4d34-a3e3-7daaf04b888d" />
# <img width="305" height="897" alt="image" src="https://github.com/user-attachments/assets/5cdc8c7b-75b7-4cde-b6a8-07a34b5bd94b" />
# Bước 6: Kích hoạt hệ thống
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1de00ebf-e4e5-462e-bd64-b548f31c12e5" />
# <img width="1246" height="1078" alt="image" src="https://github.com/user-attachments/assets/3e31b2a1-67af-4015-b9bf-9e272351ff47" />
# Phần 3: Vẽ biểu đồ Grafana & Lấy mã nhúng Iframe
Truy cập Grafana <br>
# <img width="1918" height="1025" alt="image" src="https://github.com/user-attachments/assets/ccb9d145-5912-44ab-8b81-23e334ed694e" />
TẠO BIỂU ĐỒ TRÊN GRAFANA & LẤY MÃ NHÚNG IFRAME
tạo biểu đồ mới
# <img width="1901" height="1013" alt="image" src="https://github.com/user-attachments/assets/34ccc4e9-6352-4fb2-a23f-17edc45c82af" />
Thêm InfluxDB vào Grafana
# <img width="1896" height="946" alt="image" src="https://github.com/user-attachments/assets/d9c33f13-ef2f-4b3f-84f4-8c689f05c6bf" />
# <img width="1918" height="1031" alt="image" src="https://github.com/user-attachments/assets/68a1058d-e6c4-4c80-b191-25f997c9c4ec" />
# <img width="1778" height="972" alt="image" src="https://github.com/user-attachments/assets/a82440ad-429c-4906-9528-51164c33ef2d" />
tạo Panel mới
BƯỚC 1: CHỌN ĐÚNG NGUỒN DỮ LIỆU (DATA SOURCE)
Nhìn xuống khu vực nửa dưới của màn hình (chỗ để cấu hình).

Bạn sẽ thấy một ô tên là Data source.

Bấm vào mũi tên xổ xuống ở ô đó và chọn chữ InfluxDB
# <img width="665" height="172" alt="image" src="https://github.com/user-attachments/assets/27f96dc9-5ddb-481f-aa2d-fe3ed511037c" />
BƯỚC 2: NHẬP LỆNH VẼ BIỂU ĐỒ
# <img width="1082" height="203" alt="image" src="https://github.com/user-attachments/assets/dbc087a2-20dd-4c0b-bc56-7c8a324ab24a" />
BƯỚC 3: CHỈNH THỜI GIAN HIỂN THỊ (Rất quan trọng)
# <img width="1171" height="912" alt="image" src="https://github.com/user-attachments/assets/f3df8d39-126f-415b-baf7-025444a05625" />
BƯỚC 4: HOÀN THIỆN VÀ LƯU LẠI
Bấm vào nút Refresh (biểu tượng vòng tròn xoáy mũi tên ở cạnh chỗ chọn thời gian) để nó tải dữ liệu mới nhất
Để refresh 5s để cập nhật liên 
# <img width="1548" height="982" alt="image" src="https://github.com/user-attachments/assets/9041b11b-9d31-494b-88a2-6ccce337f6ed" />
# Phần 4: LẤY MÃ NHÚNG BIỂU ĐỒ GRAFANA
# 1. giữ đoạn mã này lại để lát dán vào file Web.
# <img width="1542" height="931" alt="image" src="https://github.com/user-attachments/assets/8bdfd88c-ae8e-4728-b1d6-793d88450b3a" />
# 2. VIẾT FLASK API LẤY GIÁ TỨC THỜI (Mã nguồn mở)
# <img width="1113" height="863" alt="image" src="https://github.com/user-attachments/assets/3ed8135d-786a-465a-8021-3ab86cf54aea" />
sau đó docker compose restart flask_api lại 
# 3. TRIỂN KHAI FRONT-END TRÊN WEBSERVER NGINX
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/102731f1-9166-489d-ae53-a814a24e7081" />
# KẾT QUẢ:
# <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53fbbf4a-e18b-484e-b135-31d50784846f" />
# BƯỚC 5:
1. xuất tất cả các container ra file nén. <br>
2. xoá mọi container đang chạy <br>
3. load lại các container  từ file nén để khôi phục các container đã xoá <br>
BƯỚC 1: XUẤT TẤT CẢ CÁC IMAGE RA FILE NÉN (.tar)
Trước tiên, bạn gõ lệnh này để xem danh sách các Image đang có trên máy:
# docker images
Sau đó, bạn chạy lệnh lưu (save) toàn bộ các dịch vụ của bài tập 5 thành các file nén bằng cách chỉ định rõ tên file đầu ra -o (output): <br>
# <img width="843" height="492" alt="image" src="https://github.com/user-attachments/assets/37c408dd-2f9e-4eed-9d6a-ade04748a6aa" />
# <img width="801" height="143" alt="image" src="https://github.com/user-attachments/assets/84d8c16c-183e-4bfe-befb-6bb0d04d02f9" />
FIle xuất ra nằm trong thư mục bài tập 
# <img width="347" height="420" alt="image" src="https://github.com/user-attachments/assets/c222733c-7f17-432a-a12f-4e9631dc406f" />
BƯỚC 2: XÓA SẠCH MỌI CONTAINER ĐANG CHẠY
# <img width="1118" height="301" alt="image" src="https://github.com/user-attachments/assets/9a749e64-fbfd-4f90-9362-e1c72af4d8d8" />
BƯỚC 3: Load lại các file nén để phục hồi hệ thống <br>
dùng lệnh docker load để giải nén các file .tar ngược trở lại vào Docker (mô phỏng việc mang sang máy mới không có mạng)
# <img width="612" height="276" alt="image" src="https://github.com/user-attachments/assets/496153ac-a37f-4a91-b4c0-a9acd4e0e9db" />
Khi Terminal chạy giải nén xong xuôi, bật lại toàn bộ hệ thống
# docker compose up -d
## SAU KHI GIẢI NÉN VÀ KHỞI ĐỘNG LẠI HỆ THỐNG MỌI THỨ LẠI HOẠT ĐỘNG LẠI BÌNH THƯỜNG
## <img width="1918" height="1036" alt="image" src="https://github.com/user-attachments/assets/33c8af2b-a02f-4771-9ff1-98489f1e5c2d" />

## KẾT QUẢ TRIỂN KHAI THỰC TẾ <br>
Hệ thống đã triển khai thực tế thành công và đạt được toàn bộ các mục tiêu đặt ra:

1. Thu thập và Lưu trữ dữ liệu song song (Node-RED) <br>
Xây dựng luồng logic trên Node-RED liên tục lấy dữ liệu giá Bitcoin động từ API nguồn thực tế.

Hệ thống xử lý chuyển tiếp ghi dữ liệu song song vào InfluxDB (để lưu chuỗi thời gian lịch sử phục vụ vẽ biểu đồ) và vào MariaDB (lưu giá trị tức thời phục vụ gọi dữ liệu ngắn).

2. Trực quan hóa Biểu đồ Thời gian thực (Grafana) <br>
Kết nối thành công nguồn dữ liệu InfluxDB sang Grafana. Thiết lập Dashboard cấu hình chế độ tự động làm mới (Auto-Refresh mỗi 5 giây), tạo ra đồ thị biến động kéo <br>
dãn liên tục theo tọa độ thời gian thực rất trực quan. <br>

3. Lớp Back-end Flask API Ổn định <br>
Xây dựng API Python tại cổng 5000 (/api/price), xử lý dứt điểm lỗi phân quyền mạng nội bộ giữa các container và tích hợp thuật toán thông minh tự động quét danh <br>
sách bảng (bitcoin_history) để trả dữ liệu JSON chuẩn xác, hạn chế tối đa lỗi cứng tên bảng.

5. Giao diện Front-end Nginx Tổng quan <br>
Triển khai cổng Web tĩnh qua Nginx tại địa chỉ IP cục bộ máy chủ 192.168.1.14. Trang chủ hiển thị đồng bộ:

Khung trên hiển thị số tiền USD tức thời (tự cập nhật mỗi 2 giây bằng kỹ thuật gọi Ajax ngầm tới Flask).

Khung dưới nhúng trực tiếp biểu đồ lịch sử uốn lượn được truyền qua thẻ Iframe từ Grafana.

5. Hệ thống Cảnh báo tự động qua Telegram (Alerting) <br>
Thiết lập bộ lọc điều kiện trên Node-RED Function Node. Khi giá Bitcoin đột ngột nhảy vượt qua ngưỡng an toàn đã cấu hình, hệ thống tự động biên dịch thông điệp tường minh chứa giá trị lỗi và bắn tin nhắn cảnh báo khẩn cấp trực tiếp về Group Telegram gồm 3 thành viên (bao gồm Bot, Sinh viên và tài khoản Giảng viên chấm bài có ID: 1875746636).

Dự án được hoàn thành với kết quả vận hành mượt mà và kiểm thử đóng gói thành công độc lập.
