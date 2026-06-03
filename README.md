# BaiTap5_PTUDNMN
# nguyễn lam sơn_K225480106076
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
## <img width="378" height="115" alt="image" src="https://github.com/user-attachments/assets/5fef0f2c-a5c2-484f-8857-479cbd1b9a5b" />
# <img width="1104" height="303" alt="image" src="https://github.com/user-attachments/assets/e753b7b8-e3e4-43e5-a6fe-af25b44b50b2" />
# Bước 2: Thiết lập Node-RED (Lấy dữ liệu & Gửi Telegram)
## <img width="1919" height="980" alt="image" src="https://github.com/user-attachments/assets/ae02eda3-9f95-4929-b92e-76d57ff5863c" />
Truy cập http://<ip-máy-ảo>:1880 <br>
Kéo thả các node để tạo luồng: <br>
Inject node: Đặt lặp lại mỗi 5 giây. <br>
HTTP Request node: Gọi một API thời tiết hoặc giá vàng công khai (hoặc dùng hàm random để giả lập dữ liệu thay đổi liên tục). <br>
MySQL node & InfluxDB node: Lưu giá trị vào 2 database này. <br>
Switch node: Kiểm tra logic bất thường (VD: > 80 là High, < 20 là Low). <br>
Telegram bot node: Nối từ cổng ra của Switch node. <br>
Bạn cần tạo 1 bot qua BotFather trên Telegram, lấy Token. <br>



