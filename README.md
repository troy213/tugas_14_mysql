# Tugas 14 MySQL

Berikut perintah untuk membuat tabel nilai_mahasiswa:
```
DROP TABLE IF EXISTS nilai_mahasiswa;

CREATE TABLE nilai_mahasiswa (
nim CHAR(8) PRIMARY KEY,
nama VARCHAR(50),
semester_1 DECIMAL(4,2),
semester_2 DECIMAL(4,2),
semester_3 DECIMAL(4,2) );
```

#### Aturan:

#### 1. Ketika data mahasiswa diinput ke dalam tabel nilai_mahasiswa, buat sebuah trigger yang akan menghitung nilai IPK dari mahasiswa tersebut lalu simpan ke tabel baru.

#### 2. Agar lebih menantang, selain nilai IPK, tampilkan juga kombinasi nama dan nim mahasiswa dengan format NAMA_MAHASISWA (nim) seperti RIANA PUTRIA (17090113). Nama mahasiswa ditulis menggunakan huruf besar.

#### 3. Anda bebas untuk merancang tabel baru yang akan menampung nilai nama (nim) dan IPK ini. Sebagai contoh, misalkan saya input dua data berikut ke tabel nilai_mahasiswa:

```
INSERT INTO nilai_mahasiswa VALUES ('17090113', 'Riana Putria', 3.12, 2.98, 3.45);
INSERT INTO nilai_mahasiswa VALUES ('17140143', 'Rudi Permana', 2.56, 3.14, 3.22);
```

Maka hasilnya adalah sebagai berikut:
![Tugas 14](https://github.com/troy213/tugas_14_mysql/blob/main/Tugas%2014%20MySQL.jpg)

Sebagai tips, di dalamnya perlu menggunakan beberapa fungsi bawaan MySQL, seperti **UPPER()**, **CONCAT()** dan **ROUND()**.

#### Jawaban:

- Pertama saya buat terlebih dahulu tabel untuk menampung hasil dari trigger
```
CREATE TABLE ipk_mahasiswa (nama_dan_nip VARCHAR(50), nilai_ipk DECIMAL(4,2));
```

- Lalu saya buatkan Trigger AFTER INSERT pada tabel nilai_mahasiswa
```
DELIMITER $$

CREATE TRIGGER hitung_ipk
AFTER INSERT ON nilai_mahasiswa
FOR EACH ROW
BEGIN
  INSERT INTO ipk_mahasiswa VALUES (
  CONCAT(NEW.nama, ' (', NEW.nim,')'), ROUND(((NEW.semester_1 + NEW.semester_2 + NEW.semester_3)/3),2));
END$$

DELIMITER ;
```

- Kemudian, saya inputkan data ke dalam tabel nilai_mahasiswa agar ter-trigger
```
INSERT INTO nilai_mahasiswa VALUES 
  ('17201120', 'Tritera Erlangga', 3.87, 3.53, 3.71),
  ('17201121', 'Troy', 3.87, 3.53, 3.71);
```

**Output:**
![Tugas 14.1](https://github.com/troy213/tugas_14_mysql/blob/main/Tugas%2014%20MySQL.jpg)
