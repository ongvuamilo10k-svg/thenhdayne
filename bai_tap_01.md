# thenhdayne
1. Download và cài đặt SQL Server 2025, phiên bản Developer

  
   Cài đặt với 2 kiểu login (Mixed Mode): Windows Authentication (nhớ **Add Current User**) và SQL Server Authentication (username mặc định là **sa**, chỉ cần nhập mật khẩu **123** , nhớ nhập 2 chỗ: Enter password và Confirm password)
   <img width="1061" height="662" alt="image" src="https://github.com/user-attachments/assets/b308c6d0-ba9e-4b32-902d-16a4127c17e8" />

2. Cấu hình cho SQL Server làm việc ở cổng động (Dynamic Port), TCP: enable+active yes cho 127.0.0.1, chọn cổng động là 3xxxx với xxxx là **4 số cuối của mã số sv**, (nếu cổng này đã mở sẵn trước đó bởi 1 ứng dụng khác thì chọn cổng là 4xxxx hoặc 5xxxx)
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/943505df-84f0-49bd-bd76-4c6ecfeef5ab" />
<img width="1920" height="88" alt="image" src="https://github.com/user-attachments/assets/a6d3bb1b-a65f-43f3-9598-cc93fab9a526" />
3. Kiểm tra xem service SQL Server có đang running và mở đúng cổng đã chọn hay không?

   Sử dụng lệnh trên cmd: *netstat -ano | findstr LISTENING* để liệt kê các cổng mà máy tính đang mở,

   Nếu thấy dòng: **TCP    0.0.0.0:xxxxx**  với xxxxx là cổng đã chọn ở bước 2 là OK.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/090ed63c-2b8c-4a1a-98c7-b64dce6ec0ad" />

4. Cài đặt SQL Server Management Studio

   Link tải SSMS: https://learn.microsoft.com/en-us/ssms/install/install
5. Chạy phần mềm ssms để Đăng nhập vào SQL Server bằng 2 cách: Windows Authentication và SQL Server Authentication.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c0ed5feb-e532-4e04-8982-14e3a302a7bb" />Đăng nhập bằng Windown Authentication
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e5031cf2-bd39-46a7-9c83-7146cb588cf8" />Đăng nhập bằng SQL Server Authentication
Servername: localhost,36064 (với xxxxx là cổng đã chọn ở bước 2)
6. Sử dụng giao diện đồ hoạ của ssms: Tạo cơ sở dữ liệu mới (create database) với tên tuỳ ý, chọn Path (nơi lưu trữ db) cho file lưu dữ liệu và file lưu log ở ổ đĩa khác với ổ C. mở path đã chọn xem 2 file đã tạo ra.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c5185cf5-f4c2-45af-90e9-4ad1a8bbfe02" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/fab75e67-58df-45e8-9030-0ee02f594eca" />

7. Sử dụng giao diện đồ hoạ của ssms: Tạo bảng dữ liệu (create and design table) với tên bảng tuỳ ý, có các trường dữ liệu phù hợp với dữ liệu của file [data mẫu (CSV)](./svtnut.csv?raw=true), với Khoá chính (Primary Key) là trường **masv**
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/86fe836a-4063-4045-8f22-e286838254f2" />

8. Sử dụng giao diện đồ hoạ của ssms: Tìm cách import dữ liệu từ file mẫu vào trong bảng vừa tạo.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/672febe9-efd7-46d1-a545-51b5c6f8f77b" />

9. Mở cửa sổ mới để gõ lệnh trong ssms: GÕ lệnh để kiểm tra xem số dòng của bảng dữ liệu sau khi import, kết quả ok sẽ khoảng 12020 dòng.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4f68ddb4-652c-4d8f-9360-848e068fe165" />


10. Trong cửa sổ mới để gõ lệnh: Gõ lệnh để thêm (insert) 1 row vào bảng, với dữ liệu là thông tin cá nhân của sv đang làm bài (mỗi sv sẽ luôn khác nhau ở bước này).
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/aceda365-e0ce-40e0-90fd-5704152630f7" />

11. Trong cửa sổ mới để gõ lệnh: Gõ lệnh để cập nhật(update) trường noisinh thành 'Sao Hoả' cho những dòng có noisinh và diachi đều là NULL.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9ece5deb-063f-40c3-bc35-6aea2bd9fc70" />

12. Sử dụng giao diện đồ hoạ của ssms: Tạo bảng **SaoHoa** gồm những sinh viên có nơi sinh ở 'Sao Hoả', keyword gợi ý: sử dụng 1 câu lệnh: SELECT + INTO
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/06913243-439c-44d1-8e87-34e7ca868734" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/806957a8-697f-4e13-9d17-b6a78f567a3a" />

13. Trong cửa sổ mới để gõ lệnh: Gõ lệnh xoá (delete) trong bảng **SaoHoa** những sinh viên cùng họ với em, vd em họ nguyễn thì xoá những sv họ nguyễn.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/6bf9ef5c-41fe-42e7-af81-7c487c72e755" />

14. Sử dụng giao diện đồ hoạ của ssms: Xuất toàn bộ kết quả của các bước 6,7,8,9,10,11,12,13 ra file **dulieu.sql** , keyword gợi ý: sử dụng tính năng GEN SCRIPT struct+data cho database
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/33545173-033e-4afe-94ce-92fd75a8bd6d" />

15. Sử dụng giao diện đồ hoạ của ssms: Xoá csdl đã tạo, sau khi xoá thành công, kiểm tra tại path (path chọn ở bước 6) xem còn tồn tại 2 file của bước 6 không? 
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/099acd06-965a-4e68-bdcb-1ebad6f47222" />

--> Sau khi xóa csdl đã tạo thì 2 file của bước 6 đã bị mất đi.<br>
16. Tạo cửa sổ mới để gõ lệnh: mở file **dulieu.sql** của bước 14, chạy toàn bộ các lệnh này. REFRESH lại cây liệt kê các database => kiểm chứng kết quả được tạo ra tương đương với các bước 6,7,8,9,10,11,12,13.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ca1e4c4f-d679-4bd5-8d92-ac6320decb0b" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9fb06ba6-cd6f-4ce0-b127-a2a687bab11d" />

17. upload file **dulieu.sql** lên github repository của em.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/910e4a45-50c8-4b8d-b155-4bcbf2bf5220" />
