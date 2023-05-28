# 1. CREATE DATABASE & TABLES

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
---
# 2. INSERTING DATA

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

**14. Dấu nháy**
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

---
# 3. CRUD 
**18. Introducing SELECT**
~~~~sql
-- give me all columns of cats table
SELECT * FROM cats
-- give me "name" columns of cats table
SELECT name FROM cats
-- give me "name", "age" columns of cats table
SELECT name, age FROM cats
~~~~

**19. WHERE**
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

**20. ALIASES**
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

**21. UPDATE**
~~~sql
UPDATE cats SET breed='Shorthair' WHERE breed='Tabby';
--- kiểu WHERE khác
UPDATE cats SET age=14 WHERE name='Misty';
~~~

**22. DELETE**
~~~sql
DELETE FROM cats WHERE name='Egg';
~~~
---
# 4. STRING
**23. CONCAT**
- Ta có 2 cột 'author_fname','author_lname'

![image](/uploads/df6a59b5f080e368c625045ee61befb4/image.png)

- Nếu muốn nối 2 data của 2 cột lại 

~~~sql
SELECT CONCAT (author_fname,author_lname)
~~~

![image](/uploads/81e687fa6ed2fe6d74df7f80a447d606/image.png)

- Thêm khoảng cách

~~~sql
SELECT CONCAT (author_fname, " ", author_lname)
~~~

- Thêm 1 kí tự gì đó ở giữa các cột 

~~~sql
SELECT CONCAT_WS (' - ', author_fname, author_lname)
~~~
- CONCAT cột chứa số vẫn được

**24. SUBSTRING**

~~~sql
SELECT SUBSTRING('Hello World', 1, 4); -- Hell
SELECT SUBSTRING('Hello World', 7); -- World
SELECT SUBSTRING('Hello World', -3); -- rld
~~~
~~~sql
SELECT SUBSTRING(title, 1, 10) AS 'short title' FROM books;
~~~
~~~sql
SELECT SUBSTR(author_lname,1,1) AS 'first charactor', author_lname from books;
~~~
![image](/uploads/c9af6044d72bb16a656f554e1f553cd9/image.png)

**25. COMBINE SUBSTRING & CONCAT**
~~~sql
SELECT CONCAT
    (
        SUBSTRING(title, 1, 10),
        '...'
    ) AS 'short title'
FROM books;
~~~

**26. REPLACE**
~~~sql
SELECT
  REPLACE('cheese bread coffee milk', ' ', ' and '); -- cheese and bread and coffee and milk
~~~

- Thay kí tự tất cả kí tự "e" bằng số 3 ở các data trong column title
~~~sql
SELECT REPLACE(title, 'e ', '3') FROM books;
~~~
- Lưu ý là nó điều chỉnh data trả về thôi chứ k thay đổi gì trong data gốc

**27. REVERSE**

~~~sql
SELECT REVERSE(author_fname), author_fname FROM books;
~~~ 

![image](/uploads/b53585a0919df06a25708241681ecc6e/image.png)

**28. CHAR_LENGTH**

~~~sql
SELECT author_lname, CHAR_LENGTH(author_lname) AS 'length' FROM books;
~~~

![image](/uploads/7046edb8b6558c44e9aff8c82ad33263/image.png)

**29. UPPER & LOWER**

~~~sql
SELECT UPPER(title) AS 'UPPER TITLE', title FROM books;
~~~

![image](/uploads/3e523751485f038bed5b00ae7abf52fc/image.png)

**30. INSERT**

- Lần lượt các argument là vị trí bắt đầu chèn, số kí tự bị thay thế, chuỗi cần chèn
~~~sql
SELECT INSERT ("Hello Bobby", 5,0,' There') -- Hello There Bobby
SELECT INSERT ("Hello Bobby", 6, 2,' There') -- 'Hello Thereobby'
~~~

**31. RIGHT LEFT**
- Trả về chuỗi theo thứ tự bên phải, hoặc bên trái sang tương ứng với số kí tự defined

~~~sql
SELECT LEFT("HELLO MYSQL", 4); -- HELL
SELECT RIGHT("HELLO MYSQL", 5); -- MYSQL
~~~

**32. REPEAT**
- Repeat chuỗi theo số lần tương ứng và ghép nó lại

~~~sql
SELECT REPEAT('hello', 3) -- hellohellohello
~~~

**33. TRIM**
- Xóa các kí tự trống ở hai bên chuỗi

~~~sql
SELECT TRIM('  hello ') -- hello
~~~
- LEADING: xóa bên trái ký tự mong muốn
~~~sql
SELECT TRIM(LEADING '.' FROM '....hello+++') -- hello+++
~~~
- TRAILING: xóa bên phải kí tự mong muốn
~~~sql
SELECT TRIM(TRAILING '+' FROM '....hello+++') -- ....hello
~~~
---
# 5. REFINING
**34. DISTINCT**
- Loại bỏ các kết quả giống nhau ở output của query
- Trước khi loại bỏ của `author_lname` có khá nhiều giá trị giống nhau

![image](/uploads/d15ee224e44bc278c30e5936e98b7ee7/image.png)

~~~sql
SELECT DISTINCT author_lname FROM books
~~~
- Sau khi loại bỏ 

![image](/uploads/2663cd98fcefb047d05e238a3067a1e3/image.png)

- Nếu full name của tác giả thì sao (cần 2 trường author_lname, author_fname), vì trong ví dụ này có thể có những tác giả cùng họ nhưng khác tên thì cũng cần phải được chọn
- Cách 1: Sử dụng concat + distinct

~~~sql
SELECT DISTINCT (CONCAT(author_fname, " ", author_lname)) from books
~~~

![image](/uploads/0c5afc7e4c343881ac1d9c44dcfc8f46/image.png)

- Cách 2: Sử dụng DISTINCT
~~~sql
SELECT DISTINCT (author_fname, author_lname) from books
~~~
- Nó sẽ lọc ra 2 cột tương ứng

![image](/uploads/0822b0fa78a3cebe481c2743762ff668/image.png)

**35. ORDER BY**
- Kết quả đầu ra của query sẽ được sắp xếp theo thứ tự của 1 cột (sau ORDER BY)
~~~sql
SELECT author_lname, book_id, author_fname FROM books ORDER BY author_fname
~~~

![image](/uploads/c6a3fb0cf9dce33a84b1951d0faf6266/image.png)

- Theo mặc định nó sẽ là thứ tự giảm dần (ASCENDING), điều chỉnh bằng thêm DESC ở cuối hoặc ASC

~~~sql
SELECT author_lname, author_fname FROM books ORDER BY author_fname DESC
~~~
- Sắp xếp theo nhiều cột
~~~sql
SELECT book_id, author_fname, author_lname, pages
FROM books ORDER BY author_lname, author_fname;
~~~
- Điều này có nghĩa là sẽ sắp xếp theo author_lname trước, đến author_fname
- author_fname sẽ sắp xếp những author_lname có trùng lname nhưng khác fname theo giá trị giảm dần của fname (default)

**36. LIMIT**
~~~sql
SELECT title, released_year FROM books LIMIT 0,3; -- bắt đầu từ row đầu tiên đến 3 row tiếp theo
SELECT title, released_year FROM books LIMIT 3; -- bắt đầu từ row đầu tiên đến 3 row tiếp theo
SELECT title, released_year FROM books LIMIT 2,10; -- bắt đầu từ row thứ 3 đến 10 row tiếp theo
SELECT * FROM books LIMIT 95,18446744073709551615; -- row 95 đến row cuối cùng
~~~

**37. LIKE**
- Không phân biệt chữ hoa chữ thường
- Dấu phần trăm giống như biểu thị nơi scope bắt đầu và kết thúc, ở trước thì là tất cả trước, ở sau là tất cả sau

- Tìm các data có dữ liệu chứa kí tự `da`
~~~sql
SELECT author_fname FROM books WHERE author_fname LIKE "%da%"
~~~

![image](/uploads/20b16b925928fab54d95a240601a05cd/image.png)


- Tìm các data có dữ liệu bắt đầu với kí tự `D`
~~~sql
SELECT author_fname FROM books WHERE author_fname LIKE "D%"
~~~

![image](/uploads/a0c10a1c33ba266e44af891efd0273bc/image.png)


- Tìm các data có dữ liệu kết thúc với kí tự `D`
~~~sql
SELECT author_fname FROM books WHERE author_fname LIKE "%D"
~~~

![image](/uploads/bb2f513a9be52f633d24c3fde9a31e92/image.png)

- Tìm tất cả data có độ dài là 4 kí tự

~~~sql
SELECT author_fname FROM books WHERE author_fname LIKE "____"; -- 4 dấu gạch ngang
~~~

![image](/uploads/08b0f879feebe3c0aaf6cfd386b3f338/image.png)


- Tìm tất cả data mà có 3 kí tự và kí tự a nằm giữa
~~~sql
SELECT author_fname FROM books WHERE author_fname LIKE "_a_";
~~~

![image](/uploads/0982026a9f2d862e25afd10c174e478f/image.png)

- Tìm tất cả data có chứa kí tự `%`
~~~sql
SELECT author_fname FROM books WHERE author_fname LIKE "%\%%;
~~~
---
# 6. AGGREGATE

**38. COUNT**
- Đếm giá trị không null
~~~sql
SELECT COUNT(*) FROM books; -- đến tất cả hàng
SELECT COUNT(author_lname) FROM books; -- đếm tổng author_lname 
SELECT COUNT(DISTINCT author_lname) FROM books; -- đếm tất cả author_lname 
SELECT COUNT(DISTINCT author_lname) FROM books AS 'number of author_lname'; -- đếm tất cả author_lname và hiển thị tên cột là 'number of author_lname'
~~~

**39. GROUP BY**
- Gộp các giá trị giống nhau thành 1 group
- Ví dụ có `Lê Tâm`, `Phạm Tâm` thì chỉ hiển thị ở cột GROUP BY là `Tâm` nhưng count ra sẽ là 2
- Ứng dụng để đếm số lượng từng phần tử giống nhau trong 1 cột
- Sẽ có 2 tên Lahiri nhưng khác `author_fname` (`author_fname` có thể có giá trị hoặc null vì ở đây đếm tất cả các hàng `của 1 group` (`COUNT(*)`))
~~~sql
SELECT author_lname, COUNT(*) 
FROM books
GROUP BY author_lname;
~~~

![image](/uploads/cc49b26c2072ea6607f7490d0fc2f532/image.png)
- Chỉ được select cột mà nó thực hiện GROUP BY vì nếu SELECT thêm cột khác để hiện thị, thì sẽ hiển thị cột đó như thế nào ? không có cách để hiện thị cột đó vì thế nên sẽ không SELECT

- `GROUP BY` multiple columns
- Hiển thị số lượng người có cả họ và tên trùng nhau
~~~sql
SELECT author_fname, author_lname, COUNT(*) 
FROM books 
GROUP BY author_lname, author_fname;
~~~

![image](/uploads/3b716c5dfa9af9249d7447e7ce19f5ae/image.png)

**40. MIN MAX**

~~~sql
SELECT MAX(pages) FROM books; -- trả về cột MAX(pages) với giá trị là số pages lớn nhất
 
SELECT MIN(author_lname) FROM books;
~~~

- Chỉ được select 1 cột thôi
- Tuy nhiên muốn hiển thị tên của book có độ dài trang lớn nhất ?
=> Sử dụng `Subqueries`

**41. SUBQUERIES**
- Làm thế nào để show ra tên tiêu đề của book có nhiều số trang
- Cách 1
~~~sql
SELECT title, pages FROM books ORDER BY pages DESC LIMIT 1
~~~
  - Cách 2: sử dụng `SUBQUERIES` là query con
~~~sql
SELECT title, pages FROM books
WHERE pages = (SELECT MAX(pages) FROM books)
~~~
- Tương đương với
~~~sql
SELECT title, pages FROM books
WHERE pages = 634
~~~
- Cách 2 sẽ tối ưu hơn cách 1 vì có thể show ra nhiều giá trị (nếu nhiều cuốn sách có chung giá trị pages lớn nhất) vì cách 1 `LIMIT 1` thôi

**42. MIN,MAX kết hợp GROUP BY**
- Tìm số năm release lớn nhất và số năm nhỏ nhất của 1 tác giả
~~~sql
USE books;
SELECT author_fname, author_lname,
COUNT(*) AS 'number of books',
MAX(released_year) AS 'latest_year',
MIN(released_year) AS 'earlies_year'
FROM books
GROUP BY author_fname, author_lname;
~~~

![image](/uploads/50c499e38be0bb2fe14ab7257be9f43c/image.png)

**43. SUM**
- Tính tổng số 1 cột (nếu cột đó là string thì sẽ là 0)
~~~sql
SELECT SUM(author_fname) FROM books;
~~~
- Tính tổng số trang những người có trùng tên với nhau
~~~sql
SELECT author_lname, COUNT(*), SUM(pages)
FROM books
GROUP BY author_lname;
~~~

**43. AVG**
~~~sql
SELECT AVG(pages) FROM books;
~~~

~~~sql
SELECT 
    released_year, 
    AVG(stock_quantity), 
    COUNT(*) FROM books
GROUP BY released_year;
~~~

![image](/uploads/7dc54b0a1c454805d7f28a1d4b3e295e/image.png)

**44. EXERCISE**
- in ra số books trong database
~~~sql
SELECT COUNT(*) FROM books;
~~~
- in ra số books theo từng năm
~~~sql
SELECT released_year, 
COUNT(*) FROM books GROUP BY released_year;
~~~
- in ra tổng số books trong stock
~~~sql
SELECT SUM(stock_quantity) AS 'number of books' FROM books;
~~~
- in ra trung bình released_year cho mỗi author
- mỗi author là 1 group AVG(relseaed_year) là cho mỗi group
~~~sql
SELECT * FROM books;
SELECT author_fname, author_lname,
AVG(released_year) 
FROM books 
GROUP BY author_fname, author_lname;
~~~
- tìm fullname của author người mà viết quyển sách nhiều trang nhất
~~~sql
SELECT CONCAT(author_fname, " ",author_lname) AS 'fullname'  
FROM books
WHERE pages = (SELECT MAX(pages) FROM books);
~~~
- hiển thị số lượng sách theo mỗi năm (số năm hiển thị tăng dần), đưa ra tổng số sách, đưa ra trung bình số trang
~~~sql
SELECT released_year,
COUNT(*) AS 'boooks',
AVG(pages)
FROM books 
GROUP BY released_year
ORDER BY released_year DESC
~~~
---
# REVISTING DATA TYPES
**45. VARCHAR & CHAR**
- VARCHAR: bao nhiêu kí tự thì lưu chừng đó byte
- nếu name là "tam" -> thì chiếm 3 slot trong ô nhớ, "tam1" chiếm 4 slot
~~~sql
CREATE TABLE cats (
    name VARCHAR(4),
~~~
- CHAR: bao nhiêu kí tự cũng lưu tối đa số byte
- bất kể name là "ta", "tam", "tam1" thì nếu thiếu nó tự động fill vào chỗ trống cho đủ 4 slot gây lãng phí
~~~sql
CREATE TABLE cats (
    name CHAR(4),
~~~

![image](/uploads/9132713b455d3dd90c395c8d1753cc00/image.png)

- Tuy nhiên số lượng bộ nhớ của VARCHAR đôi khi sẽ bị dôi 1 byte so với CHAR
- Vì thế nếu lưu data với số lượng ô nhớ nhất định thì nên dùng CHAR cho tiết kiệm hơn
- Ví dụ như lưu số điện thoại, zip code, yes/no
- Còn lại nến sử dụng VARCHAR

**46. INT, TINYINT, BIGINT**
- Tra bảng để có lựa chọn tốt nhất tránh lãng phí tài nguyên

**47. DECIMAL**
~~~sql
CREATE TABLE products (
  price DECIMAL(5,2)
);
~~~
- `DECIMAL(5,2)`: có nghĩa là phần thập phân 2 chữ số, phần nguyên 3 chữ số cộng lại là 5 chữ số
- Trường hợp lỗi
  - `ERROR`: nếu phần nguyên vượt quá số chữ số quy định
  - `WARNING`: nếu phần thập phân vượt quá chữ số quy định, nó sẽ vẫn lưu nhưng sẽ làm tròn, nếu ta lưu chữ số `23,21534` thì nó sẽ lưu là `23,22`

**58. FLOAT & DOUBLE**
- So với kiểu `DECIMAL` thì `DECIMAL` chính xác hơn tuy nhiên lại chiếm nhiều dung lượng hơn
- Nếu ta sử dụng float hoặc double thì sẽ lưu trữ số lớn hơn và tiết kiệm được dung lượng
- Nhưng trả giá bằng độ chính xác

![image](/uploads/eb15397d0dc1aaa7b7747deeb823e56f/image.png)

- `FLOAT` chính xác tới 7 chữ số thập phần, và `DOUBLE` thì 15

**59. DATE & TIMES**
- DATE: YYYY-MM-DD
- TIME: HH:MM:SS
- DATETIME: YYYY-MM-DD HH:MM:SS

~~~sql
CREATE TABLE people (
	name VARCHAR(100),
    birthdate DATE,
    birthtime TIME,
    birthdt DATETIME
);
 
INSERT INTO people (name, birthdate, birthtime, birthdt)
VALUES ('Elton', '2000-12-25', '11:00:00', '2000-12-25 11:00:00');
 
INSERT INTO people (name, birthdate, birthtime, birthdt)
VALUES ('Lulu', '1985-04-11', '9:45:10', '1985-04-11 9:45:10');
 
INSERT INTO people (name, birthdate, birthtime, birthdt)
VALUES ('Juan', '2020-08-15', '23:59:00', '2020-08-15 23:59:00');
~~~

**60. CURDATE & CURTIME**

~~~sql
SELECT CURTIME(); -- current time
 
SELECT CURDATE(); --current date
 
SELECT NOW(); --current datetime
~~~

**61. DATE FUNCTION**

~~~sql
SELECT 
    birthdate,
    DAY(birthdate),
    DAYOFWEEK(birthdate),
    DAYOFYEAR(birthdate)
FROM people;
~~~

![image](/uploads/11ea801b94a293620d979f017b32484e/image.png) 

~~~sql
SELECT 
    birthdate,
    MONTHNAME(birthdate),
    YEAR(birthdate)
FROM people;
~~~

![image](/uploads/ef1edd6081864791f2a33d13d4b6c4ce/image.png)

~~~sql
SELECT 
    birthtime,
    HOUR(birthtime),
    MINUTE(birthtime)
FROM people;
~~~

~~~sql
SELECT 
    birthdt,
    MONTH(birthdt),
    DAY(birthdt),
    HOUR(birthdt),
    MINUTE(birthdt)
FROM people;
~~~

**62. FORMATTING DATE**
- Tự tạo format riêng cho mình
- Sử dụng date format, đọc doc để biết %a và cá cái khác là gì
~~~sql
SELECT birthdate, DATE_FORMAT(birthdate, '%a %b %D') FROM people;
 
SELECT birthdt, DATE_FORMAT(birthdt, '%H:%i') FROM people;
 
SELECT birthdt, DATE_FORMAT(birthdt, 'BORN ON: %r') FROM people;
~~~

**63. DATE MATH**
- DATEDIFF(expr1, expr2) (tìm khoảng cách giữa 2 thời điểm)
~~~sql
SELECT DATEDIFF(CURDATE().birthdate) FROM people
~~~
- DATE_ADD (date, INTERVAL expr unit ) (tìm thời điểm sau này 1 khoảng thời gian)
~~~sql
SELECT DATE_ADD(CURDATE(), INTERVAL 1 YEAR) --- 2 DAY, 3 MONTH
~~~

Một số ví dụ khác
~~~sql
SELECT birthdate, YEAR(birthdate + INTERVAL 21 YEAR) FROM people
~~~

**64. TIMESTAMP**
- Chỉ hỗ trợ thời gian từ 1970 đến 2038
- Tiết kiệm bộ nhớ hơn
~~~sql
SELECT TIMESTAPMP(2023-05-27 17:23:14);
~~~
- Kết quả y hệt

![image](/uploads/4c4baef409d73dbaeba415a199b5cd0e/image.png)

- TIMESTAMP có thể được dùng để cập nhật thời gian default và on
- default : trường hợp này là mỗi khi data được insert vào thì trường created_at sẽ được tạo với giá trị mặc định là `CURRENT_TIMESTAMP`
~~~sql
CREATE TABLE captions (
  text VARCHAR(150),
  created_at TIMESTAMP default CURRENT_TIMESTAMP
);
~~~
- update : trường hợp này là mỗi khi data bị sửa thì trường update_at sẽ được update với `CURRENT_TIMESTAMP`
CREATE TABLE captions2 (
  text VARCHAR(150),
  created_at TIMESTAMP default CURRENT_TIMESTAMP,
  updated_at TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
---
# LOGICAL OPERATORS

**65. NOT EQUAL**
~~~sql
SELECT * FROM books WHERE released_year != 2017;
~~~

**66. NOT LIKE**

~~~sql
SELECT * FROM books WHERE released_year != 2017;
~~~ 

![image](/uploads/b77589d864dcb09ec61cf191ff6187e3/image.png) 

- `NOT LIKE` là phần data còn lại

**67. GREATER THAN**
~~~sql
SELECT * FROM books
WHERE pages > 500;
~~~

**68. LESS THAN or EQUAl TO**
~~~sql
SELECT * FROM books
WHERE released_year <= 1985;
~~~

**69. AND**
- In ra các book được viết bởi Dave Eggers published sau năm 2010
- Có 2 phần thông tin cơ bản cần kết hợp là được viết bởi Dave Eggers và publish sau 2010
~~~sql
SELECT * FROM books WHERE author_lname = 'Eggers' AND released_year > 2010;
~~~
![image](/uploads/53ce33f74991c77c0554f1d2e91c9114/image.png)
- Có thể add thêm nhiều `AND` query nữa

**70. OR**
- Lọc title các quyển sách có tác giả là Eggers hoặc số năm sau 2010

```sql
SELECT title, author_lname, released_year FROM books
WHERE author_lname='Eggers' OR
released_year > 2010;
```

![image](/uploads/13849d8e8084de6168a00766195d8a3b/image.png)

**71. BETWEEN**
- Đưa ra các book publish giữa nằm 2004 và 2015
- Cách 1
~~~sql
SELECT title, released_year FROM books WHERE released_year >= 2004 AND released_year <= 2015;
~~~
- Cách 2
~~~sql
SELECT title, released_year FROM books WHERE released_year BETWEEN 2004 AND 2015;
~~~
- NOT BETWEEN
~~~sql
SELECT title, released_year FROM books WHERE released_year NOT BETWEEN 2004 AND 2015;
~~~

**72. Comparing Date**
- Có thể sử dụng CAST để chuyển sang từ dữ liệu này sang dữ liệu khác
- Ở ví dụ lần này, convert từ chuỗi thành date
~~~sql
SELECT CAST ('09:00:00' AS TIME)
~~~
- Cần convert sang từ string sang giờ để so sánh những là recommend
~~~sql
SELECT * FROM people WHERE birthtime 
BETWEEN CAST('09:00:00' AS TIME) 
AND CAST('16:00:00' AS TIME);
~~~

![image](/uploads/5391a212281057c24252d4b3707481fc/image.png)

- Kết quả là như nhau nhưng cái trên được recommend hơn
~~~sql
SELECT * FROM people WHERE birthtime 
BETWEEN '09:00:00'
AND '16:00:00';
~~~

**73. IN & %**
- In ra tất cả các books được viết bởi Carver hoặc Lahiri hoặc Smith
~~~sql
SELECT title, author_lname FROM books 
WHERE author_lname ='Carver'
OR author_lname ='Lahiri'
OR author_lname ='Smith'
~~~
- Cách nhanh hơn
~~~sql
SELECT title, author_lname FROM books 
WHERE author_lname IN ('Carver', 'Lahiri', 'Smith' )
~~~
- Ngược lại NOT IN
~~~sql
SELECT title, author_lname FROM books 
WHERE author_lname NOT IN ('Carver', 'Lahiri', 'Smith' )
~~~
- In ra tất cả các books có năm là số lẻ
- Thay vì sử dụng NOT IN và truyền vào các số chẵn thì ta sử dụng `MODULO %`
~~~sql
SELECT title, author_lname, released_year FROM books 
WHERE released_year % 2 != 0;
~~~

**74. CASE**
- Sử dụng khi đưa ra quyết định về các kết quả dựa trên các giá trị có sẵn
- Kiểu như kết hợp nhiều if statemnent
~~~sql
SELECT 
    title,
    stock_quantity,
    CASE
        WHEN stock_quantity <= 40 THEN '*'
        WHEN stock_quantity <= 70 THEN '**'
        WHEN stock_quantity <= 100 THEN '***'
        WHEN stock_quantity <= 140 THEN '****'
        ELSE '*****'
    END AS stock
FROM
    books;
~~~ 

![image](/uploads/e6669a0d9bc1965b4abd90d79d1bbea7/image.png)

**75. IS NULL**
~~~sql
SELECT * FROM books WHERE title IS NULL
~~~
---
# CONSTRAINTS & ALTER TABLE

**76. UNIQUE CONSTRAINTS**
- Ràng buộc UNIQUE là một cách thiết lập và thực thi tính duy nhất của một cột cụ thể
~~~sql
CREATE TABLE contacts (
	name VARCHAR(100) NOT NULL,
    phone VARCHAR(15) NOT NULL UNIQUE
);
 
INSERT INTO contacts (name, phone)
VALUES ('billybob', '8781213455');
-- Xảy ra lỗi
INSERT INTO contacts (name, phone)
VALUES ('billybob', '8781213455');
~~~ 
- Xảy ra lỗi khi add thêm một số điện thoại trùng với số điện thoại đã có trong cột "phone", do cột phone có sử dụng UNIQUE constraint


**77. CHECK CONSTRAINTS**
- Add những giá trị vào cột tuổi phải lớn hơn 18
~~~sql
CREATE TABLE parties (
name VARCHAR(10),
age INT CHECK (age>18)
);
~~~
- Add `age > 18` vào sẽ báo lỗi
- Chỉ cần điền vào CHECK 1 impression

**78. NAMED CONSTRAINTS**
- Thông thường khi không thỏa mãn CONSTRAINT nó sẽ báo lỗi `Error Code: 3819. Check constraint 'parties_chk_1' is violated.`
- Chúng ta có thể custom tên của CONSTRAINT đó lại để mỗi constraint có lỗi khác nhau
~~~sql
CREATE TABLE users2 (
    username VARCHAR(20) NOT NULL,
    age INT,
    CONSTRAINT age_not_negative CHECK (age >= 0)
);
~~~
- `Error Code: 3819. Check constraint 'parties_chk_1' is violated.` ==> `Error Code: 3819. Check constraint 'age_not_negative' is violated.`

**79. MULTIPLE -COLUMN CHECK**
- **UNIQUE CONSTRAINTS**
~~~sql
CREATE TABLE companies (
    name VARCHAR(255) NOT NULL,
    address VARCHAR(255) NOT NULL,
    CONSTRAINT name_address UNIQUE (name , address)
);
~~~
- **CHECK CONSTRAINTS**

~~~sql
CREATE TABLE houses (
  purchase_price INT NOT NULL,
  sale_price INT NOT NULL,
  CONSTRAINT sprice_gt_pprice CHECK(sale_price >= purchase_price)
);
~~~

**80. ALTER TABLE**
- Thay đổi cấu trúc của TABLE

- **ADDING COMLUMN**
~~~sql

ALTER TABLE users
ADD COLUMN address VARCHAR(15); -- giá trị ban đầu là null

ALTER TABLE companies
ADD COLUMN employee_count INT NOT NULL DEFAULT 1; -- giá trị ban đầu là 1
~~~

- **DROPPING COMLUMN**
~~~sql
ALTER TAble users
DROP COLUMN employee_count
~~~

- **RENAMEING COMLUMN**
~~~sql
ALTER TABLE suppliers RENAME TO companies; -- đổi tên table
ALTER TABLE companies RENAME COLUMN username TO company_name; -- đổi tên column
~~~

- **MODIFY COLUMN**
- Đổi lại kiểu dữ liệu và các định nghĩa khác của Column
~~~sql
ALTER TABLE companies
MODIFY company_name VARCHAR(100) DEFAULT 'unknown';
~~~
- Đổi tên, kiểu dữ liệu và các định nghĩa khác của Column
~~~sql
ALTER TABLE suppliers
CHANGE business biz_name VARCHAR(50);
~~~

**81. CONSTRAINTS OF TABLE**

~~~sql
CREATE TABLE houses(
    purchase_price NOT NULL,
    sale_price INT NOT NULL,
    CONSTRAIT sprice_gr_price CHECK (sale_price >= purchase_price )
)
~~~

- **ADD NEW CONSTRAINTS**
~~~sql
ALTER TABLE houses 
ADD CONSTRAINT positive_pprice CHECK (purchase_price >= 0);
~~~
- **DROP CONSTRAINTS**
~~~sql
ALTER TABLE houses DROP CONSTRAINT positive_pprice;
~~~

