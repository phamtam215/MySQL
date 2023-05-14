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

**11. View all rows in table**
~~~~sql
SELECT * FROM <table-name>;
~~~~
**12. Insert Multiple Data**
~~~~sql
INSERT INTO cats (name, age) VALUES ('Meatball', 5), ('Turkey', 1), ('Potato Face', 15);
~~~~
**13. Null Column**
- Xuất hiện khi sử dụng `SHOW cats`, hoặc `DESC cats`

![image](/uploads/e1bea3d013f5c66127a5a4c56e180ccf/image.png)
- Có nghĩa là cột `name` và cột `age` cho phép giá trị NULL
- Ví dụ
~~~~sql
INSERT INTO cats (name) VALUES ('Jenkins');
-- Kết quả là Jenkins NULL
~~~~
~~~~sql
INSERT INTO cats () VALUES ();
-- Kết quả là NULL NULL
~~~~
- Thêm ràng buộc không NULL khi ta tạo bảng
~~~~sql
CREATE TABLE cat2
(
  name VARCHAR(50) NOT NULL, 
  age INT NOT NULL
);
~~~~

![image](/uploads/f4f461e90462f57b5f1d1df54196e0d7/image.png)

**14. Dấu nháy***
- Nếu thêm cùng loại thì phải thêm dấu `\`
```bash
"Tom's cat"
'Tom"s cat'
'Tom\'s cat'
"Tom\"s cat"
```
**15. Add default value**
- Khi data insert vào trống thì nó sẽ quy định giá trị mặc định của đata
~~~~sql
CREATE TABLE cat3
(
  name VARCHAR(50) DEFAULT 'no name provided'
  age INT DEFAULT 99
);
~~~~
- Các trường hợp như
~~~~sql
-- kết quả là giá trị mặc định
INSERT INTO cat2(name,age) VALUES ();
-- kết quả name null, age giá trị mặc định
INSERT INTO cat2(name) VALUES (NULL);
~~~~
- Vì thế, giá trị mặc định khi nó không được quy định giá trị, còn nếu quy định giá trị là NULL thì nó vẫn NULL
- Có thể add thêm `NOT NULL` vào
~~~~sql
CREATE TABLE cats4  (    
    name VARCHAR(20) NOT NULL DEFAULT 'unnamed',    
    age INT NOT NULL DEFAULT 99 
);
~~~~
**16. Primary Keys**
- Giải quyết tình trạng là có thể có nhiều data giống hệt nhau trong bảng, việc ngăn giống data ở từng hàng là diều không thể ở SQL
- Ý tưởng cần 1 trường gì đó để phân biệt các đối tượng như này
- Sinh ra khóa chính
- Khóa chính sẽ là một số cột được sử dụng làm mã định danh duy nhất cho từng hàng riêng lẻ, vì vậy chúng phải là duy nhất trong bảng.
- Có nghĩa là nếu chúng ta cố gắng add thêm data nhưng ở trường id là giống nhau nó sẽ báo lỗi nhằm bảo đảm id là unique
~~~~sql
CREATE TABLE unique_cats (
	cat_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age INT NOT NULL
);

-- option 2
CREATE TABLE unique_cats (
	cat_id INT,
    name VARCHAR(100) NOT NULL,
    age INT NOT NULL,
    PRIMARY KEY (cat_id)
);

INSERT INTO unique_cats (cat_id,name,age) VALUES(1,'tam',20);
INSERT INTO unique_cats (cat_id,name,age) VALUES(2,'tam',20); --1 sẽ báo lỗi duplicate
~~~~
- Tuy nhiên, việc add thêm 1,2,3 như này khá phiền, cần 1 cơ chế auto increment
- PRIMARY KEY không thể là giá trị NULL, DESC table ra sẽ thấy điều đó
**17. AUTO_INCREMENT**
- Tự động tăng giá trị khóa chính cat_id, bắt đầu từ 1
- DESC ra sẽ thấy trường extra có dòng auto_increment

![image](/uploads/d2c872d03b493cc3d244bec293c24de1/image.png)
~~~~sql
CREATE TABLE unique_cats3 (
    cat_id INT AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    age INT NOT NULL,
    PRIMARY KEY (cat_id)
);
~~~~
- Nếu tự sửa cat_id trong trường hợp cat_id đã tăng đến 3
  - Nếu add vào giá trị có cat_id thêm vào bằng 3 thì nó báo duplicate
  - Nếu add vào 4 thì nó vẫn tăng bình thường
  - Nếu add vào 5 thì sẽ là 1,2,3,5
  - Nếu từ 5 add kiểu tự động tăng thì 1,2,3,5,6 bỏ qua giá trị 4

----
**18 Introducing SELECT**
~~~~sql
-- give me all columns of cats table
SELECT * FROM cats
-- give me "name" columns of cats table
SELECT name FROM cats
-- give me "name", "age" columns of cats table
SELECT name, age FROM cats
~~~~

**19 WHERE**
- WHERE rất được hay sử dụng
~~~~sql
-- tìm tất cả con mèo có tuổi bằng 4, hiển thị tất cả cột
SELECT * FROM cats WHERE age=4; 
-- tìm tất cả con mèo có tuổi bằng 4, hiển thị cột "name" và "age"
SELECT name, age FROM cats WHERE age=4;
-- tìm tất cả con mèo có name bằng "Egg", hiển thị tất cả cột 
SELECT * FROM cats WHERE name="Egg"; 
-- không phân biệt chữ hoa thường
SELECT * FROM cats WHERE name="EgG";
SELECT * FROM cats WHERE name="EGG"; 
-- tìm tất cả con mèo mà giá trị 2 cột cat_id và tuổi bằng nhau
SELECT * FROM cats WHERE cat_id=age;
~~~~

**20 ALIASES**
- Dễ dàng cho việc đọc result hơn
- Có thể đổi tên 1 cột trong truy vấn đề có thể ngắn, dễ đọc, dễ type...
- Không phải đổi tên cột mà là đổi ở chỗ kết quả đầu ra 
~~~sql
SELECT cat_id, name FROM cats;
~~~
![image](/uploads/d97557a79e2374c01bc0d9c52eccc7e4/image.png)

~~~sql
SELECT cat_id AS id, name FROM cats;
~~~

![image](/uploads/ac8af285beed4f606384e27a8a402484/image.png)

**21 UPDATE**
~~~sql
UPDATE cats SET breed='Shorthair' WHERE breed='Tabby';
--- kiểu WHERE khác
UPDATE cats SET age=14 WHERE name='Misty';
~~~

**22 DELETE**
~~~sql
DELETE FROM cats WHERE name='Egg';
~~~
