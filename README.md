Họ Và Tên : Nguyễn Văn Thanh<br>
MSSV : K235480106064<br>
Lớp : K59KMT<br>
Đề tài : Quản lý điểm sinh viên <br>
**Phần 1: Tạo database và các bảng <br>**
-- Tạo Database <br>
CREATE DATABASE [QuanLyDiemSV_K235480106064]; <br>
GO <br>
 
USE [QuanLyDiemSV_K235480106064]; <br>
GO <br>
CREATE TABLE [SinhVien] ( <br>
    [MaSinhVien] INT IDENTITY(1,1) PRIMARY KEY, -- PK <br>
    [HoTen] NVARCHAR(100) NOT NULL, <br>
    [NgaySinh] DATE, <br>
    [GioiTinh] NVARCHAR(10), <br>
    [DiaChi] NVARCHAR(200) <br>
);<br>

CREATE TABLE [MonHoc] (<br>
    [MaMonHoc] INT IDENTITY(1,1) PRIMARY KEY, -- PK<br>
    [TenMonHoc] NVARCHAR(100) NOT NULL,<br>
    [SoTinChi] INT CHECK ([SoTinChi] > 0), -- CK<br>
    [HocPhi] MONEY<br>
);<br>

CREATE TABLE [Diem] (<br>
    [MaDiem] INT IDENTITY(1,1) PRIMARY KEY, -- PK<br>
    [MaSinhVien] INT,<br>
    [MaMonHoc] INT,<br>
    [DiemThi] FLOAT CHECK ([DiemThi] >= 0 AND [DiemThi] <= 10), -- CK<br>
    [NgayThi] DATE,<br>
 
CONSTRAINT FK_Diem_SinhVien FOREIGN KEY ([MaSinhVien]) <br>
        REFERENCES [SinhVien]([MaSinhVien]), <br>

CONSTRAINT FK_Diem_MonHoc FOREIGN KEY ([MaMonHoc]) <br>
        REFERENCES [MonHoc]([MaMonHoc]) <br>
);<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/1a4c553a-d368-4124-ace8-4fe56b971c2a" />
Chú thích: Em đã tạo thành công 3 bảng với các kiểu dữ liệu đúng yêu cầu.<br>
Các Primary Key của các bảng :<br>
[MaSinhVien] → khóa chính của bảng SinhVien.<br>
[MaMonHoc] → khóa chính của bảng MonHoc.<br>
[MaDiem] → khóa chính của bảng Diem.<br>
Các Foreign Key của các bảng :<br>
[MaSinhVien] trong bảng Diem → liên kết tới SinhVien.<br>
[MaMonHoc] trong bảng Diem → liên kết tới MonHoc.<br>
Các ràng buộc Check Constraint :<br>
[SoTinChi] > 0 → số tín chỉ phải dương.<br>
[DiemThi] >= 0 AND <= 10 → điểm hợp lệ<br>
**Phần 2: Xây dựng Function<br>**
Trong SQL Server có các loại function build_in là : <br>
Scalar functions → trả về 1 giá trị <br>
Aggregate functions → tính toán trên nhiều dòng<br>
String functions → xử lý chuỗi<br>
Date functions → xử lý ngày tháng<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d2d1417c-f426-4162-8d28-db6711363d4f" />
Đây là hàm em tìm hiểu được để lấy ngày hiện tại.<br>
Hàm do người dùng tự viết trong SQL (User-Defined Function – UDF) mang mục đích để đóng gói logic xử lý và tái sử dụng — nhất là khi logic đó lặp lại hoặc không có sẵn trong system function.<br>
Các loại UDF trong SQL Server : <br>
Scalar Function : dùng khi tính toán đơn giản, trả về 1 giá trị, trong select và where.<br>
Inline Table-Valued Function (ITVF) : dùng khi trả về 1 bảng hoặc 1 tập dữ liệu, hiệu năng tốt.<br>
Multi-Statement Table-Valued Function (MTVF) : dùng khi logic phức tạp, cần nhiều bước xử lý. <br>
`-Có nhiều system function rồi mà vẫn cần tự viết Fn riêng vì System function là dùng chung, không có thể tự hiểu nhiều bài toán cụ thể , còn fn riêng là có thể xử lý bài toán riêng, có thể kết hợp nhiều logic vào với nhau , dễ dàng tái sử dụng và bảo trì.` <br>
**Viết 1 hàm Scalar function**<br>
CREATE FUNCTION fn_XepLoai (@Diem FLOAT) <br>
RETURNS NVARCHAR(20) <br>
AS <br>
BEGIN <br>
    DECLARE @Loai NVARCHAR(20); <br> 

    IF @Diem >= 8<br> 
        SET @Loai = N'Giỏi';<br> 
    ELSE IF @Diem >= 6.5<br> 
        SET @Loai = N'Khá';<br> 
    ELSE IF @Diem >= 5 <br> 
        SET @Loai = N'Trung bình'; <br> 
    ELSE<br> 
        SET @Loai = N'Yếu';<br> 

    RETURN @Loai;<br> 
END;<br> 
SELECT <br> 
    [MaSinhVien],<br> 
    [DiemThi],<br> 
    dbo.fn_XepLoai([DiemThi]) AS XepLoai<br> 
FROM [Diem];<br> 
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f57c7da7-0581-4fdb-8615-8712638307d4" />



