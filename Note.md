1. Show database
```bash
show database;
```
2. Create database
```bash
CREATE DATABASE  <name>
```
viet thuong cung duoc, nhung sau nay kho phan biet

3. Drop database
```bash
DROP DATABASE pet_shop;
```

4. Use <database_name>
```bash
USE <database_name>
```
Lệnh này giống như là dùng con trỏ chuột click vào <database_name>
Sử dụng lệnh `SELECT database();` để biết đang ở đâu
```bash
SELECT database();
```
Nếu chưa được USE database nào trước đó thì kết quả của SELECT sẽ null, còn không sẽ hiện tên database đang trỏ đến


5. A Database is bunch of tables => (ít nhất là trong dữ liệu quan hệ)
- Bảng là 1 tập hợp các dữ liệu liên quan được tổ chức ở 1 định dạng có cấu trúc
- Mỗi bảng có nhiều Columns (headers) và các Rows(data)
- Database được tạo nên từ nhiều tables
- Cần xác định rõ Data type của mỗi Columns
![image](https://github.com/phamtam215/NodeJs_BC/assets/85342922/08687d60-e775-453d-8a4e-7b51aeed41d7)
- INT: Interger
- varchar: a variable-length string (1 string có độ dài thay đổi được) : "abc", "abcde", "99999" => rất hay được sử dụng

6. Bài toán 1
Draw Table
- username (max 15 chars)
- content (max 140 chars)
- number of favourites
Make sure to specify correct MySQL datatyptes
![image](https://github.com/phamtam215/NodeJs_BC/assets/85342922/22002ae7-ee80-442e-93aa-a7768724852d)

7. Create Table
![image](https://github.com/phamtam215/NodeJs_BC/assets/85342922/8df28e2c-8253-4bd2-9d74-12a82b49896e)
![image](https://github.com/phamtam215/NodeJs_BC/assets/85342922/008f2dd1-d5cd-470b-a6a7-5eb2a5db75f8)

```bash
CREATE DATABASE animal;
use animal;
CREATE TABLE cats (
    name VARCHAR(50),
    age INT
);
 
CREATE TABLE dogs (
    name VARCHAR(50),
    breed VARCHAR(50),
    age INT
);
```
8. SHOW Table
```bash
USE animal;
SHOW TABLES; 
# để show tất cả table 
SHOW COLUMNS FROM cats;
# show tất cả column của table cats
DESC cats;
# option 2 ngắn gọn hơn (DESCRIBLE)
# 
```
9. DROP Table
```bash
DROP TABLE <table name>
```

