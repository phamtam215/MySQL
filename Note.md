**1. Show database**
~~~~sql
show database;
~~~~
**2. Create database**
~~~~sql
CREATE DATABASE  <name>
~~~~
- Viết thường cũng được, nhưng sau này khó phân biệt

**3. Drop database**
~~~~sql
DROP DATABASE pet_shop;
~~~~

**4. Use <database_name>**
~~~~sql
USE <database_name>
~~~~
Lệnh này giống như là dùng con trỏ chuột click vào <database_name>
Sử dụng lệnh `SELECT database();` để biết đang ở đâu
~~~~sql
SELECT database();
~~~~
Nếu chưa được USE database nào trước đó thì kết quả của SELECT sẽ null, còn không sẽ hiện tên database đang trỏ đến


**5. A Database is bunch of tables => (ít nhất là trong dữ liệu quan hệ)**
- Bảng là 1 tập hợp các dữ liệu liên quan được tổ chức ở 1 định dạng có cấu trúc
- Mỗi bảng có nhiều Columns (headers) và các Rows(data)
- Database được tạo nên từ nhiều tables
- Cần xác định rõ Data type của mỗi Columns

![image](/uploads/05eef29574078b6b81c2b6e471ac7269/image.png)

- INT: Interger
- varchar: a variable-length string (1 string có độ dài thay đổi được) : "abc", "abcde", "99999" => rất hay được sử dụng

**6. Bài toán 1**
Draw Table
- username (max 15 chars)
- content (max 140 chars)
- number of favourites
Make sure to specify correct MySQL datatyptes

![image](/uploads/91be63732cdcd7be6445a2f7fff9a376/image.png)

**7. Create Table**
![image](/uploads/b8a9c4db3d0dcf2cb29585f71070b0d1/image.png)

![image](/uploads/eb01ac628d99a4fae46cd9cd2b98baf2/image.png)

~~~~sql
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
~~~~
**8. SHOW Table**
~~~~sql
USE animal;
SHOW TABLES; 
--- để show tất cả table 
SHOW COLUMNS FROM cats;
--- show tất cả column của table cats
DESC cats;
--- option 2 ngắn gọn hơn (DESCRIBLE)
~~~~
**9. DROP Table**
~~~~sql
DROP TABLE <table name>
~~~~

**10. Instert Data**
~~~~sql
INSERT INTO cats (name,age) VALUES ('Jenkins', 7);
~~~~


