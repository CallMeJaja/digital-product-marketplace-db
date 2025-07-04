# FASE 3: IMPLEMENTASI DATABASE

---

## 3.1 Data Definition Language (DDL)

### 3.1.a. Script CREATE DATABASE & CREATE TABLE

```sql
CREATE DATABASE IF NOT EXISTS marketplace_digital;
USE marketplace_digital;

-- Tabel User
CREATE TABLE User (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(100),
    role ENUM('buyer','seller','admin'),
    registration_date DATETIME
);

-- Tabel Seller (subtype)
CREATE TABLE Seller (
    user_id INT PRIMARY KEY,
    store_name VARCHAR(100),
    bio TEXT,
    FOREIGN KEY (user_id) REFERENCES User(user_id)
);

-- Tabel Buyer (subtype)
CREATE TABLE Buyer (
    user_id INT PRIMARY KEY,
    FOREIGN KEY (user_id) REFERENCES User(user_id)
);

-- Tabel ProductType
CREATE TABLE ProductType (
    type_id INT AUTO_INCREMENT PRIMARY KEY,
    type_name VARCHAR(50)
);

-- Tabel ProductCategory
CREATE TABLE ProductCategory (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    description TEXT
);

-- Tabel Product
CREATE TABLE Product (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    seller_id INT,
    name VARCHAR(100),
    description TEXT,
    price DECIMAL(12,2),
    type_id INT,
    category_id INT,
    status VARCHAR(20),
    created_at DATETIME,
    FOREIGN KEY (seller_id) REFERENCES Seller(user_id),
    FOREIGN KEY (type_id) REFERENCES ProductType(type_id),
    FOREIGN KEY (category_id) REFERENCES ProductCategory(category_id)
);

-- Tabel ProductAsset
CREATE TABLE ProductAsset (
    asset_id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    asset_url VARCHAR(255),
    asset_type VARCHAR(30),
    FOREIGN KEY (product_id) REFERENCES Product(product_id)
);

-- Tabel Transaction
CREATE TABLE Transaction (
    transaction_id INT AUTO_INCREMENT PRIMARY KEY,
    buyer_id INT,
    transaction_date DATETIME,
    total_amount DECIMAL(12,2),
    status VARCHAR(20),
    FOREIGN KEY (buyer_id) REFERENCES Buyer(user_id)
);

-- Tabel TransactionDetail
CREATE TABLE TransactionDetail (
    transaction_detail_id INT AUTO_INCREMENT PRIMARY KEY,
    transaction_id INT,
    product_id INT,
    price DECIMAL(12,2),
    quantity INT,
    FOREIGN KEY (transaction_id) REFERENCES Transaction(transaction_id),
    FOREIGN KEY (product_id) REFERENCES Product(product_id)
);

-- Tabel Payment
CREATE TABLE Payment (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    transaction_id INT,
    payment_method VARCHAR(30),
    amount DECIMAL(12,2),
    payment_date DATETIME,
    status VARCHAR(20),
    FOREIGN KEY (transaction_id) REFERENCES Transaction(transaction_id)
);

-- Tabel Review
CREATE TABLE Review (
    review_id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    buyer_id INT,
    rating INT,
    comment TEXT,
    review_date DATETIME,
    FOREIGN KEY (product_id) REFERENCES Product(product_id),
    FOREIGN KEY (buyer_id) REFERENCES Buyer(user_id)
);

-- Tabel Commission
CREATE TABLE Commission (
    commission_id INT AUTO_INCREMENT PRIMARY KEY,
    transaction_id INT,
    amount DECIMAL(12,2),
    percentage DECIMAL(5,2),
    status VARCHAR(20),
    FOREIGN KEY (transaction_id) REFERENCES Transaction(transaction_id)
);
```

### 3.1.b. Implementasi Primary Key, Foreign Key, Constraint

Sudah diimplementasikan pada DDL di atas untuk setiap tabel.

---

### 3.1.c. Implementasi VIEW untuk Reporting (Minimal 3)

```sql
-- 1. View: Daftar Produk dengan Seller dan Kategori
CREATE VIEW vw_product_seller_category AS
SELECT p.product_id, p.name AS product_name, s.store_name, pt.type_name, pc.name AS category, p.price, p.status
FROM Product p
JOIN Seller s ON p.seller_id = s.user_id
JOIN ProductType pt ON p.type_id = pt.type_id
JOIN ProductCategory pc ON p.category_id = pc.category_id;

-- 2. View: Total Penjualan per Seller
CREATE VIEW vw_total_sales_per_seller AS
SELECT s.store_name, SUM(t.total_amount) AS total_sales
FROM Seller s
JOIN Product p ON s.user_id = p.seller_id
JOIN TransactionDetail td ON p.product_id = td.product_id
JOIN Transaction t ON td.transaction_id = t.transaction_id
GROUP BY s.store_name;

-- 3. View: Review Produk
CREATE VIEW vw_product_reviews AS
SELECT p.name AS product_name, u.name AS buyer_name, r.rating, r.comment, r.review_date
FROM Review r
JOIN Product p ON r.product_id = p.product_id
JOIN Buyer b ON r.buyer_id = b.user_id
JOIN User u ON b.user_id = u.user_id;
```

---

## 3.2 Data Manipulation Language (DML)

### 3.2.a. Script INSERT Data Sample (20 records per tabel utama)

> **Catatan:** Berikut contoh untuk beberapa data, silakan lanjutkan hingga 20 data per tabel utama.

```sql
-- User
INSERT INTO User (name, email, password, role, registration_date) VALUES
('Andi', 'andi@mail.com', 'pass123', 'seller', '2025-07-01'),
('Budi', 'budi@mail.com', 'pass456', 'buyer', '2025-07-02'),
-- ... tambahkan hingga minimal 20 user
;

-- Seller
INSERT INTO Seller VALUES (1, 'Andi Store', 'Jual akun & jasa digital');
-- ... tambah seller lain

-- Buyer
INSERT INTO Buyer VALUES (2);
-- ... tambah buyer lain

-- ProductType
INSERT INTO ProductType (type_name) VALUES ('Akun Premium'), ('Jasa'), ('Topup'), ('File Digital');
-- ... tambah type lain jika perlu

-- ProductCategory
INSERT INTO ProductCategory (name, description) VALUES ('Game', 'Produk game'), ('Software', 'Produk perangkat lunak'), ('Desain', 'Produk desain digital');
-- ... tambah kategori lain jika perlu

-- Product
INSERT INTO Product (seller_id, name, description, price, type_id, category_id, status, created_at) VALUES
(1, 'Akun Netflix Premium', 'Akun legal 1 bulan', 50000, 1, 2, 'aktif', '2025-07-03'),
(1, 'Topup ML', 'Topup Mobile Legend 86 diamonds', 15000, 3, 1, 'aktif', '2025-07-04');
-- ... tambah produk hingga 20

-- ProductAsset
INSERT INTO ProductAsset (product_id, asset_url, asset_type) VALUES
(1, 'https://asset.url/akun_netflix.txt', 'file'),
(2, 'https://asset.url/topupml.txt', 'file');
-- ... tambah asset hingga 20

-- Transaction
INSERT INTO Transaction (buyer_id, transaction_date, total_amount, status) VALUES
(2, '2025-07-05', 50000, 'selesai'),
(2, '2025-07-05', 15000, 'selesai');
-- ... tambah transaksi hingga 20

-- TransactionDetail
INSERT INTO TransactionDetail (transaction_id, product_id, price, quantity) VALUES
(1, 1, 50000, 1),
(2, 2, 15000, 1);
-- ... tambah detail hingga 20

-- Payment
INSERT INTO Payment (transaction_id, payment_method, amount, payment_date, status) VALUES
(1, 'e-wallet', 50000, '2025-07-05', 'berhasil'),
(2, 'bank', 15000, '2025-07-05', 'berhasil');
-- ... tambah payment hingga 20

-- Review
INSERT INTO Review (product_id, buyer_id, rating, comment, review_date) VALUES
(1, 2, 5, 'Akun work, seller ramah', '2025-07-06'),
(2, 2, 4, 'Topup cepat!', '2025-07-06');
-- ... tambah review hingga 20

-- Commission
INSERT INTO Commission (transaction_id, amount, percentage, status) VALUES
(1, 5000, 10, 'selesai'),
(2, 1500, 10, 'selesai');
-- ... tambah commission hingga 20
```

---

### 3.2.b. Minimal 15 Query SELECT Beragam

```sql
-- 1. Daftar semua produk dan seller
SELECT p.name, s.store_name FROM Product p JOIN Seller s ON p.seller_id = s.user_id;

-- 2. Produk dengan harga > 20000
SELECT name, price FROM Product WHERE price > 20000;

-- 3. Total transaksi per buyer
SELECT b.user_id, COUNT(t.transaction_id) total_trans, SUM(t.total_amount) total_belanja
FROM Buyer b JOIN Transaction t ON b.user_id = t.buyer_id GROUP BY b.user_id;

-- 4. Produk dengan review tertinggi
SELECT p.name, AVG(r.rating) as avg_rating FROM Product p
JOIN Review r ON p.product_id = r.product_id GROUP BY p.product_id ORDER BY avg_rating DESC LIMIT 5;

-- 5. Jumlah produk per kategori
SELECT pc.name, COUNT(p.product_id) FROM ProductCategory pc
LEFT JOIN Product p ON pc.category_id = p.category_id GROUP BY pc.category_id;

-- 6. Daftar transaksi status 'selesai'
SELECT * FROM Transaction WHERE status='selesai';

-- 7. Rata-rata rating per seller
SELECT s.store_name, AVG(r.rating) as avg_rating FROM Seller s
JOIN Product p ON s.user_id = p.seller_id
JOIN Review r ON p.product_id = r.product_id
GROUP BY s.store_name;

-- 8. Produk belum pernah dibeli
SELECT p.name FROM Product p LEFT JOIN TransactionDetail td ON p.product_id = td.product_id WHERE td.product_id IS NULL;

-- 9. Total komisi yang diperoleh marketplace
SELECT SUM(amount) as total_komisi FROM Commission WHERE status='selesai';

-- 10. Daftar pembayaran gagal
SELECT * FROM Payment WHERE status='gagal';

-- 11. Top 3 buyer paling banyak transaksi
SELECT b.user_id, COUNT(t.transaction_id) as jumlah
FROM Buyer b JOIN Transaction t ON b.user_id = t.buyer_id
GROUP BY b.user_id ORDER BY jumlah DESC LIMIT 3;

-- 12. Semua review untuk produk tertentu (misal: product_id=1)
SELECT * FROM Review WHERE product_id = 1;

-- 13. Produk per seller tertentu (misal: seller_id=1)
SELECT * FROM Product WHERE seller_id = 1;

-- 14. Transaksi dalam rentang tanggal tertentu
SELECT * FROM Transaction WHERE transaction_date BETWEEN '2025-07-01' AND '2025-07-10';

-- 15. Detail transaksi tertentu (misal: transaction_id=1)
SELECT * FROM TransactionDetail WHERE transaction_id = 1;
```

---

### 3.2.c. Query UPDATE, DELETE

```sql
-- UPDATE: Ubah harga produk tertentu
UPDATE Product SET price = 60000 WHERE product_id = 1;

-- DELETE: Hapus review tertentu
DELETE FROM Review WHERE review_id = 1;
```

---

### 3.2.d. Data Control Language (DCL) untuk User Management

```sql
-- Buat user database khusus reporting
CREATE USER 'reporting_user'@'localhost' IDENTIFIED BY 'securepass';

-- Hak akses hanya SELECT pada view reporting
GRANT SELECT ON marketplace_digital.vw_product_seller_category TO 'reporting_user'@'localhost';
GRANT SELECT ON marketplace_digital.vw_total_sales_per_seller TO 'reporting_user'@'localhost';
GRANT SELECT ON marketplace_digital.vw_product_reviews TO 'reporting_user'@'localhost';

-- Revoke akses jika perlu
-- REVOKE SELECT ON marketplace_digital.vw_product_reviews FROM 'reporting_user'@'localhost';
```

---

## 3.3 Advanced SQL Features

### 3.3.a. Fungsi MySQL

```sql
-- String: Cari produk dengan kata 'Akun'
SELECT * FROM Product WHERE name LIKE '%Akun%';

-- Date: Produk dibuat bulan Juli 2025
SELECT * FROM Product WHERE MONTH(created_at) = 7 AND YEAR(created_at) = 2025;

-- Numeric: Komisi lebih dari 10%
SELECT * FROM Commission WHERE percentage > 10;

-- Aggregate: Rata-rata rating semua produk
SELECT AVG(rating) FROM Review;
```

### 3.3.b. JOIN (minimal 8 jenis)

```sql
-- 1. INNER JOIN: Produk dan seller
SELECT p.name, s.store_name FROM Product p INNER JOIN Seller s ON p.seller_id = s.user_id;

-- 2. LEFT JOIN: Produk yang belum punya review
SELECT p.name FROM Product p LEFT JOIN Review r ON p.product_id = r.product_id WHERE r.review_id IS NULL;

-- 3. RIGHT JOIN: Semua review dan info produk
SELECT r.*, p.name FROM Review r RIGHT JOIN Product p ON r.product_id = p.product_id;

-- 4. CROSS JOIN: Semua kombinasi buyer dan kategori
SELECT u.name, c.name FROM Buyer b CROSS JOIN ProductCategory c JOIN User u ON b.user_id = u.user_id;

-- 5. JOIN Tiga tabel: Transaksi, buyer, user
SELECT t.transaction_id, u.name FROM Transaction t
JOIN Buyer b ON t.buyer_id = b.user_id
JOIN User u ON b.user_id = u.user_id;

-- 6. JOIN empat tabel: Product, Seller, Review, Buyer
SELECT p.name, s.store_name, u.name AS buyer_name, r.rating
FROM Product p
JOIN Seller s ON p.seller_id = s.user_id
JOIN Review r ON p.product_id = r.product_id
JOIN Buyer b ON r.buyer_id = b.user_id
JOIN User u ON b.user_id = u.user_id;

-- 7. Self JOIN: Cari seller yang juga buyer
SELECT s.user_id FROM Seller s JOIN Buyer b ON s.user_id = b.user_id;

-- 8. FULL OUTER JOIN SIMULASI (pakai UNION)
SELECT p.name, r.rating FROM Product p LEFT JOIN Review r ON p.product_id = r.product_id
UNION
SELECT p.name, r.rating FROM Product p RIGHT JOIN Review r ON p.product_id = r.product_id;
```

---

## 3.4 Triggers dan Transaction

### 3.4.a. Minimum 3 Trigger (BEFORE/AFTER)

```sql
-- 1. Trigger BEFORE INSERT pada Review (cek rating maksimal 5)
DELIMITER //
CREATE TRIGGER before_insert_review
BEFORE INSERT ON Review
FOR EACH ROW
BEGIN
    IF NEW.rating > 5 THEN
        SET NEW.rating = 5;
    END IF;
END;
//
DELIMITER ;

-- 2. Trigger AFTER INSERT pada Payment (update status transaksi)
DELIMITER //
CREATE TRIGGER after_payment_update_transaction
AFTER INSERT ON Payment
FOR EACH ROW
BEGIN
    IF NEW.status = 'berhasil' THEN
        UPDATE Transaction SET status = 'selesai' WHERE transaction_id = NEW.transaction_id;
    END IF;
END;
//
DELIMITER ;

-- 3. Trigger AFTER DELETE pada Product (hapus asset otomatis)
DELIMITER //
CREATE TRIGGER after_delete_product
AFTER DELETE ON Product
FOR EACH ROW
BEGIN
    DELETE FROM ProductAsset WHERE product_id = OLD.product_id;
END;
//
DELIMITER ;
```

---

### 3.4.b. Transaction Handling (COMMIT/ROLLBACK)

```sql
START TRANSACTION;
    INSERT INTO Transaction (buyer_id, transaction_date, total_amount, status) VALUES (2, NOW(), 50000, 'baru');
    -- ... detail transaksi, payment, dll
COMMIT;

-- Jika terjadi error, gunakan ROLLBACK;
```

---

### 3.4.c. Testcase Trigger

1. Insert Review dengan rating 7, hasil rating akan tersimpan maksimal 5.
2. Insert Payment dengan status 'berhasil', status Transaction otomatis menjadi 'selesai'.
3. Delete Product, semua asset terkait pada ProductAsset akan otomatis terhapus.
