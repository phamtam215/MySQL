# 1. ONE TO MANY AND JOIN

**1. FOREIGN KEY**

![image](/uploads/445f056d75a672b15db00b5e9e8f03da/image.png)

~~~sql
CREATE TABLE customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(50)
);
 
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_date DATE,
    amount DECIMAL(8,2),
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
~~~
- Kết quả của 2 table

![image](/uploads/03772fc37250e3a10611187815c47063/image.png)

![image](/uploads/cbed457d784abe525c3292b8dd0d27d5/image.png)

- Nếu thực hiện thêm 1 order nhưng customer_id rất lớn

~~~sql
INSERT INTO orders (order_date, amount, customer_id)
VALUES ('2016-02-10', 99.99, 200);
~~~

- `customer_id` không chứa trong cột `id` của `customers` mà trước đó ta đã dùng `FOREIGN KEY (customer_id) REFERENCES customers(id)`
- vì thế sẽ báo lỗi

**2. CROSS JOIN**
- Là phương pháp nối 2 bảng lại với nhau, không có gì đặc biệt
~~~sql
SELECT * FROM orders 
WHERE customer_id = (SELECT id FROM customers WHERE last_name = 'George');
 
-- To perform a (kind of useless) cross join:
SELECT * FROM customers, orders;
~~~

**3. INNER JOIN**

![image](/uploads/ee0880dbced50c900bbb7ac189348cd1/image.png)

- Tương tự như populate, là sẽ link với các cột bên bảng khác
- Order 

![image](/uploads/e34a1adc038b0b2fac6017cce2cdcbea/image.png)

- Customers

![image](/uploads/cdb4ffef49050b87f4221866ea290db0/image.png)

- Ở khía cạnh của `customers`
~~~sql
SELECT * FROM customers
JOIN orders ON orders.customer_id = customers.id;
~~~

![image](/uploads/81eb32bd0778846b8f70f09e437ef0cf/image.png)

- Table đang join với `customers` là `orders`
- `ON`: `customer_id` của `orders` tương ứng với `id` của `customer`
- Kết quả sẽ in ra các cột của `customers` và `orders` của `customer_id` của `orders` có chứa trong trường `id` của `customer`
- `JOIN` keyword to bring the information from another table and we have to specify how to connect them by `ON`
- Sử dụng `JOIN` hoặc `INNER JOIN` là như nhau
```sql
ON orders.customer_id = customers.id <=> ON  customers.id = orders.customer_id;
```
- **INNER JOIN with GROUP BY**

~~~sql
SELECT 
    first_name, last_name, SUM(amount) AS total
FROM
    customers
        JOIN
    orders ON orders.customer_id = customers.id
GROUP BY first_name , last_name
ORDER BY total;
~~~

![image](/uploads/5b9aa213f790230cded96ea18802e516/image.png)

- `GROUP BY` kết hợp `INNER JOIN`
- Có thể sử dụng `GROUP BY customers.first_name , customers.last_name`, tuy nhiên vì order không có 2 trường `last_name` và `first_name` nên có thể không cần 

**4. LEFT JOIN**

![image](/uploads/07289d12eba622d0ac7b80f3e7e9aead/image.png)

~~~sql
SELECT 
    first_name, last_name, order_date, amount
FROM customers
LEFT JOIN orders ON orders.customer_id = customers.id;
~~~
- Nhìn vào ảnh ta thấy customers là A (left side), orders là B (right side), vì đây là LEFT JOIN có nghĩa là sẽ lấy hết giá trị A cùng với những giá trị trùng với B

![image](/uploads/30f061f151ee4dd2551f17bc094f4f30/image.png)

- Kết quả cho thấy có các giá trị `NULL` là vì ta sẽ lấy tất cả các giá trị bên customers là A sau đó link với B là orders, tuy nhiên ở orders phần customers_id là các giá trị (1,1,2,2,5) vì thế những customer nào là 3, 4 sẽ bị NULL ở phần orders (trong thực tế là lập account nhưng chưa đặt hàng)

- **LEFT JOIN with GROUP BY and IFNULL**

~~~sql
SELECT 
    first_name, 
    last_name, 
    IFNULL(SUM(amount), 0) AS money_spent
FROM
    customers
        LEFT JOIN
    orders ON customers.id = orders.customer_id
GROUP BY first_name , last_name;
~~~
- Thay thế giá trị `NULL` bằng `0` 

**5. RIGHT JOIN**

![image](/uploads/aa649df6317bc232e1496fac6fd137d2/image.png)
- Ngược lại với left join mà thôi

**6. ON DELETE CASCADE**
- Chuyện gì sẽ xảy ra nếu customers có link với orders bị xóa ?
  - Nếu xóa customer đó => báo lỗi vì có FOREIGN key tham chiếu đến nó. Vì foreign key constraint yêu cầu customer_id inside orders phải là real customer_id
  - 
~~~sql
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_date DATE,
    amount DECIMAL(8 , 2 ),
    customer_id INT,
    FOREIGN KEY (customer_id)
        REFERENCES customers (id)
        ON DELETE CASCADE
)
~~~

- `ON DELETE CASCADE` thêm option này vào thì khi 1 customer bị xóa mà có link với orders thì trường order của customer đó cũng sẽ tự động xóa theo
---
# 2. Many to Many
![image](/uploads/2518b8368fd12a46a136421ba7ee9383/image.png)

**Challenge 1: Identify the series that have no reviews**

~~~sql
SELECT title AS unreviewed_series FROM series
LEFT JOIN reviews ON series.id = reviews.series_id
WHERE series_id = NULL
~~~

**Challenge 2: Identify average vote genre of series**
~~~sql
SELECT genre ROUND(AVG(rating), 2) as avg_rating FROM series
JOIN reviews ON series.id = review.series.id
GROUP BY genre
~~~

**Challenge 3: Đưa ra firstname, lastname với tổng số review, điểm MIN, điểm MAX, tổng trung bình điểm, status là inactive nếu rating = 0**
- Overview
    - LEFT JOIN với reviews để xác định là có review
    - In ra firstname, lastname, count review, avg, min, max
    - Status sử dụng case

~~~sql
SELECT first_name, last_name,
       COUNT(rating) as nums_of_rating
       IFNULL(MIN(rating), 0) as min,
       IFNULL(MAX(rating), 0) as min,
       ROUND(IFNULL(AVG(rating),0),2) as average,
       CASE
            WHEN COUNT(rating) > 0 THEN 'ACTIVE'
            ELSE 'INACTIVE'
       END AS status
FROM reviewers
LEFT JOIN reviews ON reviewers.id = reviews.reviewer_id
GROUP BY first_name , last_name;
~~~

![image](/uploads/b2c679d1e431faae4a3d5acf95fc36c5/image.png)

**Challenge 4: In ra kết quả gồm title, rating, reviewers**

~~~sql
SELECT 
    title,
    rating,
    CONCAT(first_name, ' ', last_name) AS reviewer
FROM
 reviews
        INNER JOIN
    series ON reviews.series_id = series.id
        INNER JOIN
    reviewers ON reviews.reviewer_id = reviewers.id;
~~~
---
**6. VIEW**

~~~sql
SELECT title, released_year, genre, rating, first_name, last_name FROM reviews
JOIN series ON series.id = reviews.series_id
JOIN reviewers ON reviewers.id = reviews.reviewer_id;
~~~
- Kết quả của truy vấn này là 1 bảng
- Xuất phát từ nhu cầu ta muốn sử dụng bảng kết quả này cho những query lần sau, thì thay vì mỗi lần phải thực hiện các query ở trên lần nữa thì ta sử dụng View
- View sẽ biến bảng kết quả thành 1 bảng ảo (khi show table là có luôn bảng ảo này), bảng này có thể thực hiện các thao tác như 1 bảng thông thường nhưng không ảnh hưởng đến data của các bảng thực

~~~sql
CREATE VIEW full_reviews AS
SELECT title, released_year, genre, rating, first_name, last_name FROM reviews
JOIN series ON series.id = reviews.series_id
JOIN reviewers ON reviewers.id = reviews.reviewer_id;
~~~
- Tạo 1 bảng ảo `full_reviews`
~~~sql
SELECT * FROM full_reviews; -- kết quả như trên
~~~

**7. UPDATEABLE VIEW**
- Bảng ảo ở 6 sẽ không thể update data được vì có chứa JOIN
- Ngoài ra còn có GROUP BY,... tra cứu document => các kết quả đó, bảng ảo của nó sẽ chỉ có thể thao tác xem, không thể update
- Tuy nhiên, nhưng bảng ảo kết quả của những query này có thể update được (thêm, sửa, xóa)
~~~sql
CREATE VIEW order_series AS
SELECT * FROM series ORDER BY released_year;
~~~

**8. REPLACING/ALTERING VIEW**
- Xảy ra các trường hợp
    - muốn thay đổi query để đưa ra bảng view khác thay thế view cũ 
    - ví dụ như thêm cột, đổi order ... 
- Cách 1
~~~sql
ALTER VIEW ordered_series AS
SELECT * FROM series ORDER BY released_year DESC; -- sắp xếp lại thứ tự giảm dần
~~~
- Cách 2
~~~sql
CREATE OR REPLACE VIEW ordered_series AS
SELECT * FROM series ORDER BY released_year DESC;
~~~
- DROP VIEW
~~~sql
DROP order_series
~~~

**9. HAVING CLAUSE**
~~~sql
SELECT 
    title, 
    AVG(rating),
    COUNT(rating) AS review_count
FROM full_reviews 
GROUP BY title WHERE HAVING(rating) > 1;
~~~
- Mệnh đề `HAVING` được sử dụng trong truy vấn sau mệnh đề `GROUP BY` để áp dụng điều kiện lên các kết quả đã được nhóm.

**10. SQL MODE**
- Xem mode hiện tại
~~~sql
SELECT @@GLOBAL.sql_mode;
SELECT @@SESION.sql_mode;
~~~
- SET mode
- Khi sử dụng lệnh SET này cần liệt kê tất cả mode vào, nếu muốn disable mode nào thì bỏ mode đó ra
- Các mode được lấy từ dòng xem mode ở trên
- Các mode cách nhau dấu phẩy
~~~sql
SET GLOBAL sql_mode = '<mode_name>';
SET SESSION sql_mode = '<mode_name>';
SET GLOBAL sql_mode =" ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION"
~~~

**11. STRICT_TRANS_TABLES**
- Là 1 SQL mode mặc định
- Trans =  Transactional
- Ở mode này nếu cột là số mà điền string là báo lỗi
- Nếu không có mode này thì điền string vào trường số thì vẫn bình thường

**12. MORE MODE**
1. MODE ONLY_FULL_GROUP_BY là mode mà chỉ cho phép những trường được SELECT có thể GROUP_BY, nếu tắt mode này đi thì 
~~~sql
SELECT title, rating FROM series GROUP BY title
~~~
- Query này sẽ chạy, rating sẽ hiện thị giá trị đầu tiên của nó trong 1 group

 Và nhiều mode khác nữa tự nghiên cứu đi
 ---

