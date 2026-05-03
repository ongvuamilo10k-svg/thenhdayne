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
Chú giải : ảnh ở trên là viết được câu lệnh Scalar fn trong SQL và đã khai thác được nó. <br>
**Viết câu lệnh inline table-valued function**
CREATE FUNCTION fn_SinhVienDat (@DiemMin FLOAT)<br>
RETURNS TABLE<br>
AS<br>
RETURN<br>
(<br>
    SELECT sv.[MaSinhVien], sv.[HoTen], d.[DiemThi]<br>
    FROM [SinhVien] sv<br>
    JOIN [Diem] d ON sv.[MaSinhVien] = d.[MaSinhVien]<br>
    WHERE d.[DiemThi] >= @DiemMin<br>
);<br>
SELECT * FROM dbo.fn_SinhVienDat(7);<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/aab31347-71b0-48c3-8533-69f35688b65f" />
Chú giải : trên ảnh đã viết được câu lệnh inline table-valued function và áp dụng vào bài toán quản ly.<br>
**Viết 1 câu lệnh Multi-statement table-valued function**
CREATE FUNCTION fn_BangDiemTongHop () <br>
RETURNS @KQ TABLE<br>
(<br>
    MaSinhVien INT,<br>
    HoTen NVARCHAR(100),<br>
    DiemTB FLOAT,<br>
    XepLoai NVARCHAR(20)<br>
)<br>
AS<br>
BEGIN<br>
    INSERT INTO @KQ<br>
    SELECT <br>
        sv.[MaSinhVien],<br>
        sv.[HoTen],<br>
        AVG(d.[DiemThi]),<br>
        dbo.fn_XepLoai(AVG(d.[DiemThi]))<br>
    FROM [SinhVien] sv<br>
    JOIN [Diem] d ON sv.[MaSinhVien] = d.[MaSinhVien]<br>
    GROUP BY sv.[MaSinhVien], sv.[HoTen];<br>
    RETURN;<br>
END;<br>
SELECT * FROM dbo.fn_BangDiemTongHop();<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4dd21fb0-8efa-49d4-9090-8e29be67da28" />
Chú giải : ảnh trên đã viết được câu lệnh Multi-statement table-valued và áp dụng vào bài toán để khai thác hàm đó <br>
**Phần 3 Xây dựng Store Proceduce**
Trong SQL Server có những SP có sẵn là :<br>
Xem danh sách bảng<br>
Xem cấu trúc bảng<br>
Xem database <br>
**code viết store procedure để thực hiện lệnh INSERT**<br>
CREATE PROCEDURE sp_ThemDiem <br>
    @MaSV INT,<br>
    @MaMH INT,<br>
    @Diem FLOAT<br>
AS<br>
BEGIN<br>
    IF @Diem < 0 OR @Diem > 10<br>
    BEGIN<br>
        PRINT N'Điểm không hợp lệ';<br>
        RETURN;<br>
    END<br>
    INSERT INTO [Diem]([MaSinhVien], [MaMonHoc], [DiemThi], [NgayThi])<br>
    VALUES (@MaSV, @MaMH, @Diem, GETDATE());<br>
END;<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/44bc4454-eb9c-4b73-9f7c-a34c39266bfe" />
Chú giải : INSERT được điểm vào bảng điểm thông qua câu lệnh trên. <br>
**code viết store procedure để thực hiện lệnh OUTPUT** <br>
CREATE PROCEDURE sp_TinhDiemTB <br>
    @MaSV INT,<br>
    @DiemTB FLOAT OUTPUT<br>
AS<br>
BEGIN<br>
    SELECT @DiemTB = AVG([DiemThi])<br>
    FROM [Diem]<br>
    WHERE [MaSinhVien] = @MaSV;<br>
END;<br>
DECLARE @TB FLOAT;<br>
EXEC sp_TinhDiemTB 1, @TB OUTPUT;<br>
SELECT @TB AS DiemTrungBinh;<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4c81ad82-dd72-4456-bd3b-e3f991f349ef" />
chú giải : đã chạy và OUTPUT được Diemtrungbinh ra màn hình.<br>
**code viết store procedure để trả về một tệp kết quả từ lệnh SELECT** <br>
CREATE PROCEDURE sp_BangDiem <br>
AS<br>
BEGIN<br>
    SELECT <br>
        sv.[HoTen],<br>
        mh.[TenMonHoc],<br>
        d.[DiemThi]<br>
    FROM [SinhVien] sv<br>
    JOIN [Diem] d ON sv.[MaSinhVien] = d.[MaSinhVien]<br>
    JOIN [MonHoc] mh ON d.[MaMonHoc] = mh.[MaMonHoc];<br>
END;<br>
EXEC sp_BangDiem;<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b0ff9dac-8b08-46fe-94a4-fc1b59e28954" />
chú giải : câu lệnh trên đã trả về một tệp kết quả từ lệnh SELECT. <br>
**Phần 4 : Trigger và xử lý logic nghiệp vụ** <br>
ALTER TABLE [SinhVien]<br>
ADD [DiemTB] FLOAT;<br>
GO<br>
CREATE TRIGGER trg_UpdateDiemTB<br>
ON [Diem]<br>
AFTER INSERT, UPDATE<br>
AS<br>
BEGIN<br>
    UPDATE sv<br>
    SET [DiemTB] = (<br>
        SELECT AVG([DiemThi])<br>
        FROM [Diem]<br>
        WHERE [MaSinhVien] = sv.[MaSinhVien]<br>
    )<br>
    FROM [SinhVien] sv<br>
    JOIN inserted i ON sv.[MaSinhVien] = i.[MaSinhVien];<br>
END;<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/8370fc96-a654-46d2-ab4f-0cb9eef9bedd" />
chú giải : ở trên là 1 trigger cập nhật điểm , nếu em nhập điểm ở bảng điểm thì cột DiemTB sẽ thay đổi theo.<br>
**Phần 5 : Cursor và Duyệt dữ liệu**<br>
<img DECLARE @Ten NVARCHAR(100); <br>
DECLARE sv_cursor CURSOR FOR<br>
SELECT [HoTen] FROM [SinhVien];<br>
OPEN sv_cursor;<br>
FETCH NEXT FROM sv_cursor INTO @Ten;<br>
WHILE @@FETCH_STATUS = 0<br>
BEGIN<br>
    PRINT N'Sinh viên: ' + @Ten;<br>
    FETCH NEXT FROM sv_cursor INTO @Ten;<br>
END;<br>
CLOSE sv_cursor;<br>
DEALLOCATE sv_cursor;<br>
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e26d3323-d63f-4919-847a-ecf60b173d74" />
chú giải : chạy cursor duyệt qua danh sách 1 câu lệnh SQL dạng SELECT
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e54dc925-f86e-4cf9-82a3-0db5cc7cf416" />
chú giải : hình ảnh không dùng cursor.<br>
**code không dùng cursor**<br>
SELECT N'Sinh viên: ' + [HoTen]<br>
FROM [SinhVien];<br>
**bảng so sánh giữa 2 cái**
<img width="890" height="225" alt="image" src="https://github.com/user-attachments/assets/42518910-3ec5-4c1d-84a2-cfc5ce7b834e" /><br>
Còn về bài toán mà chỉ cursor mới giải quyết được thì em chưa nghĩ ra và mới tham khảo trên mạng về bài toán Xét học bổng theo thứ tự xếp hạng điểm từ cao xuống thấp, nhưng chỉ cấp cho 3 sinh viên đầu tiên, và ngân sách học bổng giảm dần sau mỗi sinh viên.<br>
Ví dụ:<br>
Ngân sách ban đầu: 10 triệu<br>
Quy tắc:<br>
Sinh viên thứ 1 → 5 triệu<br>
Sinh viên thứ 2 → 3 triệu<br>
Sinh viên thứ 3 → phần còn lại<br>
Hoặc:<br>
Nếu sinh viên trước đã nhận học bổng thì số tiền còn lại của sinh viên sau sẽ thay đổi.<br>
lúc này thì các dòng sau dẽ phụ thuộc vào các dòng trước nên cần Logic → Cursor hợp lý hơn.

