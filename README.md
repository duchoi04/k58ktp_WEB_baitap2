# k58ktp_WEB_baitap2
nội dung bài tập 2
==============================
NGÀY GIAO: 19/10/2025
==============================
DEADLINE: 26/10/2025
==============================
2. NỘI DUNG BÀI TẬP: 2.1. Cài đặt Apache web server:

Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop

Download apache server, giải nén ra ổ D, cấu hình các file:

D:\Apache24\conf\httpd.conf

D:Apache24\conf\extra\httpd-vhosts.conf để tạo website với domain: fullname.com code web sẽ đặt tại thư mục: D:\Apache24\fullname (fullname ko dấu, liền nhau)

sử dụng file c:\WINDOWS\SYSTEM32\Drivers\etc\hosts để fake ip 127.0.0.1 cho domain này ví dụ sv tên là: Đỗ Duy Cốp thì tạo website với domain là fullname ko dấu, liền nhau: doduycop.com

thao tác dòng lệnh trên file D:\Apache24\bin\httpd.exe với các tham số -k install và -k start để cài đặt và khởi động web server apache. 2.2. Cài đặt nodejs và nodered => Dùng làm backend:

Cài đặt nodejs:

download file https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi (đây ko phải bản mới nhất, nhưng ổn định)

cài đặt vào thư mục D:\nodejs

Cài đặt nodered:

chạy cmd, vào thư mục D:\nodejs, chạy lệnh npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"


download file: https://nssm.cc/release/nssm-2.24.zip giải nén được file nssm.exe copy nssm.exe vào thư mục D:\nodejs\nodered\

tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau): @echo off REM fix path set PATH=D:\nodejs;%PATH% REM Run Node-RED node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*

mở cmd, chuyển đến thư mục: D:\nodejs\nodered

cài đặt service a1-nodered bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"

chạy service a1-nodered bằng lệnh: nssm start a1-nodered 2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name 2.4. Cài đặt thư viện trên nodered:

truy cập giao diện nodered bằng url: http://localhost:1880

cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus

Sửa file D:\nodejs\nodered\work\settings.js : tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới adminAuth: { type: "credentials", users: [{ username: "admin", password: "chuỗi_mã_hoá_mật_khẩu", permissions: "*" }] },

với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php

chạy lại nodered bằng cách: mở cmd, vào thư mục D:\nodejs\nodered và chạy lệnh nssm restart a1-nodered khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880 2.5. tạo api back-end bằng nodered:

tại flow1 trên nodered, sử dụng node http in và http response để tạo api

thêm node MSSQL để truy vấn tới cơ sở dữ liệu

logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây):

http in : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem

function : để tiền xử lý dữ liệu gửi đến

MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý

http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json có thể thêm node debug để quan sát giá trị trung gian.

test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị 2.6. Tạo giao diện front-end:

html form gồm các file : index.html, fullname.js, fullname.css cả 3 file này đặt trong thư mục: D:\Apache24\fullname nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là doduycop khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css

index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.

fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn. 2.7. Nhận xét bài làm của mình:

đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?

đã hiểu cách sử dụng nodered để tạo api back-end như nào?

đã hiểu cách frond-end tương tác với back-end ra sao?

Bài làm:

2.1. Cài đặt Apache web server:


Vô hiệu hóa IIS:

<img width="1103" height="642" alt="Screenshot 2025-10-26 212151" src="https://github.com/user-attachments/assets/1e425d2c-fe4c-4d52-b0a3-4a22d32209e3" />


Tải Apache về và giải nén ra ổ D ( ở đây em giải nén ra ổ G):

<img width="1284" height="706" alt="Screenshot 2025-10-26 212253" src="https://github.com/user-attachments/assets/082f4536-8732-4234-94b4-31852e4dfcf5" />


Cấu hình file D:\Apache24\conf\httpd.conf:

<img width="820" height="122" alt="Screenshot 2025-10-26 212422" src="https://github.com/user-attachments/assets/937269aa-beaf-4c8d-b605-ec557ab86b5e" />


<img width="389" height="66" alt="Screenshot 2025-10-26 212514" src="https://github.com/user-attachments/assets/79d6bcfd-5fd3-45d2-9149-0ae67cd33262" />

<img width="408" height="72" alt="Screenshot 2025-10-26 212517" src="https://github.com/user-attachments/assets/9cbaefe5-6937-4205-92dd-bff0df1c262e" />


<img width="463" height="144" alt="Screenshot 2025-10-26 212618" src="https://github.com/user-attachments/assets/0b0644ae-433c-400d-953f-ceccf6e0a601" />



Cấu hình file D:Apache24\conf\extra\httpd-vhosts.conf:

<img width="557" height="196" alt="Screenshot 2025-10-26 212741" src="https://github.com/user-attachments/assets/bfe3ff66-d70a-40c9-9c95-8a570eea99d6" />

Tạo 1 thư mục chứa code web (html,css,js) trong thư mục Apache24:

<img width="1405" height="708" alt="Screenshot 2025-10-26 213657" src="https://github.com/user-attachments/assets/3a2aba81-a19a-4142-ae13-46f8dba95214" />


Sử dung Nodepad chạy quyền Admin mở vị trí file c:\WINDOWS\SYSTEM32\Drivers\etc\hosts để fake ip 127.0.0.1 cho domain:

<img width="1278" height="721" alt="Screenshot 2025-10-26 214045" src="https://github.com/user-attachments/assets/1ccf6b2e-025d-410e-a2d4-202e947f0060" />



<img width="1014" height="653" alt="Screenshot 2025-10-26 213959" src="https://github.com/user-attachments/assets/afc3f420-ba2e-4f80-bef3-a39b72fe5bcf" />



Thao tác các dòng lệnh trên file D:\Apache24\bin\httpd.exe và khởi đồng web server Apache:

image

<img width="1633" height="983" alt="Screenshot 2025-10-26 030022" src="https://github.com/user-attachments/assets/7edf6f06-e16e-40d6-8bd4-dc8fafa7a82d" />


2.2. Cài đặt nodejs và nodered => Dùng làm backend:


Cài đặt Nodejs: (từ link: https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi) chạy và cài vào thư mục G:\nodejs

<img width="1415" height="717" alt="Screenshot 2025-10-26 214834" src="https://github.com/user-attachments/assets/7e18ea3b-a20f-4e2d-ba35-5f792e09526a" />


Cài đặt nodered:

Chạy các lệnh trên CMD: npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered

<img width="707" height="150" alt="Screenshot 2025-10-26 215224" src="https://github.com/user-attachments/assets/f6199ea0-62c3-405f-a5de-5ec38163b7e5" />


download file: https://nssm.cc/release/nssm-2.24.zip, giải nén vào D:\nodejs\nodered<br>


tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):

<img width="1190" height="317" alt="Screenshot 2025-10-26 215423" src="https://github.com/user-attachments/assets/01faf390-192a-460d-81d5-06e22902cade" />


mở cmd, chuyển đến thư mục: D:\nodejs\nodered


cài đặt service a1-nodered bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"

<img width="1096" height="523" alt="Screenshot 2025-10-25 164137" src="https://github.com/user-attachments/assets/ef3af101-8f60-49e1-b9a9-41caa4870eb8" />


chạy service a1-nodered bằng lệnh: nssm start a1-nodered

<img width="1096" height="523" alt="Screenshot 2025-10-25 164137" src="https://github.com/user-attachments/assets/4c646455-b86f-4278-b967-dcdc8c43f601" />


2.3. Tạo csdl với đề tài là HỆ THỐNG QUẢN LÝ CỬA HÀNG ĐỒ ĂN NHANH – FastFoodDB:

<img width="244" height="295" alt="Screenshot 2025-10-26 221340" src="https://github.com/user-attachments/assets/05bb0912-383f-4ae7-9fce-21872764f3ca" />


2.4. Cài đặt thư viện trên nodered:

truy cập url: http://localhost:1880 và cài các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus

<img width="1326" height="987" alt="Screenshot 2025-10-26 221537" src="https://github.com/user-attachments/assets/06c06aae-ea67-4110-b444-89afd0816722" />


<img width="1621" height="864" alt="Screenshot 2025-10-26 221631" src="https://github.com/user-attachments/assets/308d6366-7ee1-4f4e-90d5-459b8198068c" />



Sửa file D:\nodejs\nodered\work\settings.js bằng nodepad quyền Admin: mật khẩu đã được em mã hóa qua tool: https://tms.tnut.edu.vn/pw.php

<img width="938" height="857" alt="Screenshot 2025-10-26 221817" src="https://github.com/user-attachments/assets/654fb99e-8ba0-456d-aec5-3c24318ec4d2" />


Chạy lại nodered qua CMD:

<img width="1096" height="523" alt="Screenshot 2025-10-25 164137" src="https://github.com/user-attachments/assets/d2b89d1c-d789-43b4-a642-d2537d055442" />


Truy cập lại url: http://localhost:1880 và đăng nhập với tài khoản admin:

<img width="1396" height="989" alt="Screenshot 2025-10-26 221911" src="https://github.com/user-attachments/assets/3784fa30-a2d8-4a22-a708-71b1471da156" />


2.5. tạo api back-end bằng nodered:
<img width="1528" height="875" alt="Screenshot 2025-10-26 235244" src="https://github.com/user-attachments/assets/7703edfa-4299-4642-9b6a-53ddb8921c59" />


2.6. Tạo giao diện front-end:
<img width="1920" height="1080" alt="Screenshot 2025-10-26 205446" src="https://github.com/user-attachments/assets/c1f34f67-30d8-4d25-84c6-77e5a0190794" />

2.7. Nhận xét bài làm của mình:

Em đã hiểu được quá trình cài đặt các phần mềm web server Apache 

Cách kết nối và truy cập cơ sở dữ liệu SQL Server trực tiếp qua Node-RED.

Biết cách tạo, chỉnh sửa và kiểm tra đơn giản REST AP

