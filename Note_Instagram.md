# USER
~~~sql
CREATE TABLE users (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);
~~~

# PHOTO
- Có `user_id` tham chiếu đến id của TABLE `users`
~~~sql
CREATE TABLE photos (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    image_url VARCHAR(255) NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id)
);
~~~

# COMMENT
- Có `user_id` tham chiếu đến id của TABLE `users`
- Có `photo_id` tham chiếu đến id của TABLE `photos`
~~~sql
CREATE TABLE comments (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    comment_text VARCHAR(255) NOT NULL,
    photo_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(user_id) REFERENCES users(id)
);
~~~

# LIKE
- Không cần đề cập đến lượt likes ở những nơi nào khác nên không cần có `id`
- Đảm bảo cho số lượng like của 1 user trên 1 ảnh là chỉ một
~~~sql
- CREATE TABLE likes (
    user_id INTEGER NOT NULL,
    photo_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    PRIMARY KEY(user_id, photo_id)
);
~~~

# FOLLOWER
- Follower là người theo dõi, fllowee là người được theo dõi
- Một người chỉ có thể follow 1 một người 1 lần
~~~sql
CREATE TABLE follows (
    follower_id INTEGER NOT NULL,
    followee_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(follower_id) REFERENCES users(id),
    FOREIGN KEY(followee_id) REFERENCES users(id),
    PRIMARY KEY(follower_id, followee_id)
);
~~~

# HASTAGS

![image](/uploads/e4ca2f11cc41f208d2357b1e3c2e02eb/image.png)

![image](/uploads/3a66687d12cbc1bf5cff0afe13657f70/image.png)

![image](/uploads/70a42040fef0a8996a687a3ba7a1a285/image.png)

- Sử dụng solution 3
```sql
CREATE TABLE tags (
  id INTEGER AUTO_INCREMENT PRIMARY KEY,
  tag_name VARCHAR(255) UNIQUE,
  created_at TIMESTAMP DEFAULT NOW()
);
```
```sql
CREATE TABLE photo_tags (
    photo_id INTEGER NOT NULL,
    tag_id INTEGER NOT NULL,
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(tag_id) REFERENCES tags(id),
    PRIMARY KEY(photo_id, tag_id)
);
```
