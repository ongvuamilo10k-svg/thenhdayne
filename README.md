**Bài Tập 3** <br>
Thông Tin Sinh Viên 
- Họ tên: Nguyễn Văn Thanh
- MSSV: K235480206064
- Môn học: Hệ quản trị CSDL
- Lớp: 59KMT.K01
- Giảng viên: Đỗ Duy Cốp
# BÀI TẬP VỀ NHÀ : THIẾT KẾ VÀ CÀI ĐẶT CSDL QUẢN LÝ CẦM ĐỒ

## 1. Mô tả bài toán

Hệ thống cần quản lý các hợp đồng vay tiền thế chấp tài sản. Điểm đặc thù của hệ thống là cơ chế tính lãi linh hoạt, quản lý danh mục tài sản thế chấp và xử lý thanh lý đồ khi quá hạn.

## 2. Các quy tắc nghiệp vụ

### Khách hàng & Tài sản

- Một khách hàng có thể có nhiều hợp đồng cầm cố.
- Một hợp đồng có thể bao gồm nhiều tài sản thế chấp khác nhau.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/11741ede-7566-48a0-af92-621d1d21609e" />

Ảnh ràng các ràng buộc với nhau

### Khách hàng & Hợp đồng (Mối quan hệ 1-N)
**Đoạn thể hiện:** Ràng buộc `FK_HopDong_KhachHang` trong bảng `HopDong`.

 Một khách hàng có thể có nhiều hợp đồng cầm cố khác nhau. Khóa ngoại `MaKH` trong bảng `HopDong` đảm bảo mọi hợp đồng đều thuộc về một khách hàng cụ thể và cho phép một khách hàng mở nhiều khoản vay theo thời gian.
 
### Hợp đồng & Tài sản (Mối quan hệ 1-N)
**Đoạn thể hiện:** Ràng buộc `FK_TaiSan_HopDong` trong bảng `TaiSan`.

 Một hợp đồng có thể bao gồm nhiều tài sản thế chấp khác nhau. Điều này cho phép khách hàng dùng nhiều món đồ (ví dụ: xe máy và điện thoại) để đảm bảo cho cùng một khoản vay. Tất cả tài sản này đều liên kết với một mã hợp đồng (`MaHD`) duy nhất, hỗ trợ việc quản lý và trả đồ từng phần.

 ### Cơ chế lãi suất

#### Lãi đơn (Trước Deadline 1)

- 5.000đ / 1.000.000đ gốc / ngày.

#### Lãi kép (Từ sau Deadline 1)

- Lãi được tính dựa trên:
  - Gốc + lãi đơn đã tích lũy.

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/abb257ec-431d-4b68-a779-9097820b6690" />


Ảnh Tính lãi

### Kết quả từ hình ảnh:

Đã viết được fn_TinhTongSoNoHienTai . Thấy Nguyễn Văn ABC Và Chí Phèo đều vay nhưng chưa qua ngày vay nên chưa nhảy lên số tiền.

### Trạng thái hợp đồng

- Đang vay
- Quá hạn (nợ xấu)
- Đã thanh toán
- Đã thanh lý tài sản

## 4. Nhiệm vụ cụ thể của sinh viên
### Nhiệm vụ 1: Thiết kế CSDL

- Vẽ sơ đồ ERD:\
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/6726da13-71c9-45a7-be7d-767196d170af" />


  -Thực thể (Entities): Chính là các bảng như KhachHang, HopDong, TaiSan, LichSuThanhToan.
  
  Thuộc tính (Attributes): Các cột trong bảng như MaKH, TenKH, SoTienVay,....
  
  Mối quan hệ (Relationships): Các đường kẻ nối giữa các bảng thể hiện quan hệ $1:N$ (một-nhiều).
  
  Ví dụ: Một khách hàng có nhiều hợp đồng.
  
  Khóa chính/Khóa ngoại: Có biểu tượng chiếc chìa khóa (PK) và các đường nối thể hiện khóa ngoại (FK)

### Quan hệ dữ liệu

- 1 Khách hàng có thể có nhiều Hợp đồng.
- 1 Hợp đồng có thể cầm cố nhiều Tài sản.
- 1 Hợp đồng phát sinh theo thời gian nhiều lần biến động trạng thái hoặc số tiền nợ.


1. Bảng Khách Hàng (KhachHang)
   
Lưu trữ thông tin định danh và liên lạc duy nhất của từng khách hàng.

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e77e9d72-0d0d-47e5-89f2-efa8d1cbf965" />


MaKH (PK): Khóa chính, định danh duy nhất cho mỗi khách hàng.

TenKH: Họ và tên khách hàng.

SDT: Số điện thoại liên lạc.

CCCD: Số Căn cước công dân (Duy nhất).

2. Bảng Hợp Đồng (HopDong)

Quản lý các khoản vay vốn. Chứa khóa ngoại để xác định chủ sở hữu hợp đồng.
   
 <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4160123f-270a-4694-9167-27afdddeb0d4" />


MaHD (PK): Khóa chính của hợp đồng.

MaKH (FK): Khóa ngoại nối với bảng KhachHang.

NgayVay: Ngày bắt đầu vay.

SoTienVay: Dư nợ gốc của hợp đồng.

Deadline1: Mốc thời gian quy định lãi suất đơn.

Deadline2: Thời gian quá hạn.

TrangThai: Tình trạng hợp đồng (Đang vay, Quá hạn, Đã tất toán).

3. Bảng Tài Sản (TaiSan)

Quản lý danh sách các món đồ cầm cố. Một hợp đồng có thể cầm cố nhiều tài sản khác nhau.

  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/3cf250cb-9090-42c9-9adf-62bd78f9fce4" />

MaTS (PK): Khóa chính của tài sản.

MaHD (FK): Khóa ngoại nối với bảng HopDong.

TenTS: Tên gọi hoặc mô tả tài sản.

GiaTriDinhGia: Giá trị tài sản sau khi thẩm định.

IsSold: là trạng thái của tài sản thế chấp.

4. LichSuThanhToan

Mục đích: Theo dõi chi tiết các lần trả nợ từng phần theo thời gian.

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/da2f5b13-8e8c-4876-a6c2-88bd06faa88b" />


MaLog (PK): Khóa chính bản ghi nhật ký.

MaHD (FK): Khóa ngoại nối với bảng HopDong.

NgayTra: Ngày giờ thực hiện thanh toán.

SoTienTra: Số tiền thanh toán thực tế (Gốc/Lãi).

NguoiThu: Tên nhân viên tiếp nhận giao dịch.

### Event 1: Đăng ký hợp đồng mới (Vay tiền)

Viết Store Procedure tiếp nhận hợp đồng:

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ab2ceb4b-25eb-4711-88b8-89b8fdd154fc" />

Tiếp nhận thông tin khách hàng:

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/2b714daf-e83c-46ae-9686-4895eeb98d57" />


Sử dụng logic kiểm tra CCCD để phân loại:

Khách mới: Thực hiện INSERT vào bảng KhachHang.

Khách cũ: Tự động lấy MaKH hiện có để liên kết hợp đồng.

Sử dụng SCOPE_IDENTITY() để truy xuất ID vừa tạo, phục vụ nối khóa ngoại (FK).

Quản lý Danh sách tài sản & Định giá (Assets Management).

Tiếp nhận danh sách tài sản thông qua Table-Valued Parameter (@DanhSachTS).

Cho phép một hợp đồng cầm cố nhiều tài sản cùng lúc (
1
:
N
).

Lưu trữ chi tiết tên tài sản và giá trị định giá thẩm định.Số tiền vay gốc (Principal Amount):

Lưu trữ khoản nợ gốc tại bảng HopDong.Đây là cơ sở dữ liệu đầu vào để hệ thống tính toán lãi đơn và lãi kép dựa trên biến động thời gian thực.

### Event 2: Tính toán công nợ thời gian thực

Viết Function:

#### fn_CalcMoneyTransaction(TransactionID, TargetDate)

- Tính số tiền phải trả của TransactionID đến ngày TargetDate. Tính số tiền khách đã trả trong 1 giao dịch cụ thể tính đến ngày chốt (TargetDate):

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/414d43ff-a9b0-4b19-a451-409cdace90d0" />

Sử dụng điều kiện NgayTra <= @TargetDate.

Nếu ngày khách trả tiền nằm sau ngày chốt, hàm trả về 0.

Nếu ngày khách trả tiền nằm trước hoặc đúng ngày chốt, hàm trả về số tiền thực tế đã trả.

Giúp báo cáo công nợ chính xác tại bất kỳ thời điểm nào trong quá khứ mà không bị lẫn lộn với các giao dịch phát sinh sau đó.

#### fn_CalcMoneyContract(ContractID, TargetDate)

- Tính tổng số tiền khách phải trả. Tính tổng nợ thực tế (Gốc + Lãi) của một hợp đồng tại thời điểm bất kỳ.
  - Gốc

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b2f49e3e-c07b-4179-a2f2-882290f3d803" />


Tiền gốc: Giá trị vay ban đầu được lấy từ bảng HopDong
  - Lãi đơn

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b2f49e3e-c07b-4179-a2f2-882290f3d803" />>
    

 Lãi đơn: Áp dụng công thức $Gốc + (Gốc \times r \times n)$ cho giai đoạn trong hạn (trước mốc Deadline1).   
  - Lãi kép
    
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b265aa38-59b8-4b82-8d4c-0a5ccf5e9a51" />

### Event 3: Xử lý trả nợ và hoàn trả tài sản

#### Nếu tài sản đã bị thanh lý
(Sau Deadline 2 và có cờ IsSold)

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/93555278-435e-44a5-9fb7-21211873ecde" />

Trong ảnh: (IF EXISTS): Đóng vai trò là "người gác cổng". Hệ thống sẽ yêu cầu khách hàng đưa ra mã hợp đồng và lục soát trong bảng TaiSan xem có món đồ nào của hợp đồng này đã bị bán (IsSold = 1) hay chưa.

(SELECT): Thay vì báo lỗi hệ thống khó hiểu, câu lệnh này xuất ra một cái Bảng thông báo đã thanh lý,  để nhân viên biết rõ lý do từ chối thu tiền.

Kết quả: bảng LichSuThanhToan không hề tăng thêm tiền, đảm bảo tính an toàn tuyệt đối khi tài sản không còn trong kho.


#### Nếu tài sản chưa bị thanh lý

 Tính tổng nợ
 
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/536c4f25-b494-455a-be71-6bdd3ed13ef9" />



- Trừ số tiền khách trả vào hệ thống.
- 
<img width="1919" height="859" alt="Screenshot 2026-05-12 231242" src="https://github.com/user-attachments/assets/5a049ab5-d51e-4cca-a929-86aa182aa2d3" />



#### Nếu trả hết tiền

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/faf130c4-ac1e-4e1f-8026-ac540b5078a7" />


Khi trả hết tiền @DuNoConLai <= 0	Tự động chuyển trạng thái hợp đồng thành "Đã thanh toán đủ" và giải phóng tài sản kho

### Danh sách gợi ý trả lại tài sản

<img width="1919" height="859" alt="Screenshot 2026-05-12 231532" src="https://github.com/user-attachments/assets/f37fc5c5-bd5b-4bc6-9982-9c01a354efa7" />

Điều kiện:
- Giá trị tài sản còn lại >= dư nợ còn lại.

 Đây là phần quan trọng nhất để bảo vệ nguồn vốn của chủ tiệm. Hệ thống chỉ gợi ý trả lại một món đồ cụ thể nếu tổng giá trị định giá của các món đồ còn lại trong kho vẫn đủ lớn hơn hoặc bằng số nợ khách chưa trả xong.

 (Tổng giá trị các tài sản hiện có) - (Giá trị món đồ định trả) >= Dư nợ còn lại.

 Giúp chủ tiệm có thể linh hoạt trả bớt đồ cho khách để giải phóng kho bãi nhưng vẫn giữ lại đủ tài sản có giá trị làm vật thế chấp an toàn cho số nợ còn lại.

### Event 4: Truy vấn danh sách nợ xấu (Nợ khó đòi)

Xuất danh sách các khách hàng:
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/fb4171b0-a8d8-4444-b24d-f0c469aa942e" />


Những hợp đồng có Deadline 1 < GETDATE() (đã quá hạn cam kết lần 1) và TrangThai <> 'Đã thanh toán đủ'.

Sắp xếp: Danh sách được ưu tiên theo Số ngày quá hạn giảm dần (người nợ lâu nhất hiện lên đầu) để bộ phận thu hồi nợ dễ dàng theo dõi.

Do em để ngày vay là 12-5-2026 và hạn tháng 6 nên chưa có ai quá hạn nợ sấu . trong ảnh là code để tìm nợ sấu. 

Tổng nợ hiện tại: Sử dụng hàm fn_TinhNoXau để tính toán cả Gốc + Lãi lũy tiến đến thời điểm hiện tại. Điều này giúp nhân viên báo con số chính xác cho khách hàng ngay lập tức.

Nợ sau 1 tháng (Dự báo): Đây là tính năng quản trị. Hệ thống tự động tính toán số tiền khách sẽ phải trả nếu tiếp tục nợ thêm 30 ngày nữa. Con số này dùng để gây áp lực hoặc thuyết phục khách hàng thanh toán sớm nhằm tránh lãi chồng lãi..


### Event 5: Quản lý thanh lý tài sản

#### Trigger 1

Tự động chuyển trạng thái hợp đồng sang:
- Quá hạn (nợ xấu)
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/48311b56-5cc9-4b27-b172-e4a8bb0f4756" />


Điều kiện:
- Hợp đồng đang ở trạng thái "Đang vay"
- Ngày vượt quá Deadline 1

#### Trigger 2

Tự động chuyển trạng thái tài sản sang:
- Sẵn sàng thanh lý
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/dd906d19-e1f1-4ec9-8115-5cb8356b1d77" />



Điều kiện:
- Hợp đồng đang ở trạng thái "Quá hạn (nợ xấu)"
- Ngày vượt quá Deadline 2

#### Tự động chuyển trạng thái tài sản thành:
- Đã bán thanh lý
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f8251488-e274-4ba1-b916-20d28983ca7d" />


Điều kiện:
- Trạng thái hợp đồng chuyển sang:
  - Đã thanh lý
  - 
#### Nhật ký giao dịch

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/06c6dc85-a6a7-4513-bd63-935a6288deba" />

#### Sự kiện bổ sung

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/5d355f10-b390-497b-9033-83ec73365dc1" />

1. Audit Log (Lịch sử thanh toán)
Mục tiêu: Lưu vết toàn bộ dòng tiền thu được từ khách hàng. Thay vì chỉ cập nhật số tổng, bảng này lưu lại từng giao dịch nhỏ.

Các thành phần:

Ngày Giao Dịch: Thời điểm thực tế nhân viên thu tiền.

Số Tiền Thu: Ghi nhận 2,000,000đ (tiền lãi khách vừa đóng).

Ghi Chú: Hệ thống tự động điền "Thu lãi gia hạn hợp đồng", giúp đối soát kế toán cuối tháng minh bạch.

Giá trị: Giúp chủ tiệm kiểm soát nhân viên, tránh việc thu tiền khách nhưng không nhập vào hệ thống (chống gian lận).

Dời Deadline: Bạn có thể thấy Hạn Mới D2 đã được đẩy sang tháng 7 năm 2026.
