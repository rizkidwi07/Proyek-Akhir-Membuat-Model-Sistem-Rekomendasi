# Laporan Proyek Machine Learning - Rizki Dwi Sya'bana Nugraha
## Domain Proyek

Domain yang dipilih untuk proyek _machine learning_ ini adalah **Rekomendasi film**, dengan judul **Model Rekomendasi: Film Indonesia**

### Latar Belakang

<div><img src="https://github.com/rizkidwi07/Source/raw/main/Foto-by-GoodStats-1024x538.jpg") width="1000"/></div><br />
  
Dalam era digital saat ini, banyaknya pilihan film yang tersedia di berbagai platform membuat pengguna kesulitan menemukan film yang sesuai dengan preferensi mereka. Oleh karena itu, sistem rekomendasi menjadi solusi untuk membantu pengguna menemukan film yang relevan berdasarkan rating, genre, atau preferensi pengguna lain.

Sistem rekomendasi ini menggunakan metode Content-Based Filtering dan Collaborative Filtering untuk memberikan rekomendasi yang lebih personal. Proyek ini bertujuan untuk mengembangkan model yang dapat merekomendasikan film Indonesia kepada pengguna berdasarkan data dari IMDb.

Database Film Indonesia yang dikumpulkan melalui Kaggle, memberikan peluang untuk menganalisis dan merekomendasi harga properti film dengan lebih akurat. Dengan menggunakan metode _machine learning_, kita dapat memanfaatkan data historis untuk mengembangkan model rekomendasi.

### Mengapa dan Bagaimana Masalah Ini Harus Diselesaikan?

- Meningkatkan pengalaman pengguna dalam menemukan film yang sesuai.

- Memanfaatkan teknologi kecerdasan buatan dalam industri hiburan.

- Mengoptimalkan waktu pengguna dalam mencari film.


## Business Understanding

### Problem Statements

Berdasarkan latar belakang di atas, berikut ini merupakan rincian masalah yang dapat diselesaikan pada proyek ini:
- Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang ada, bagaimana kita dapat merekomendasikan film lain yang mungkin disukai oleh pengguna? 

### Goals

Tujuan dari proyek ini adalah:
- Menghasilkan sejumlah rekomendasi film yang dipersonalisasi untuk pengguna dengan teknik content-based filtering.
- Menghasilkan sejumlah rekomendasi film yang sesuai dengan preferensi pengguna dan belum pernah ditonton sebelumnya dengan teknik collaborative filtering.

### Solution statements
- Content-Based Filtering: Merekomendasikan film berdasarkan kemiripan fitur (genre).
- Collaborative Filtering: Merekomendasikan film berdasarkan kesamaan preferensi pengguna lain.
  
## Data Understanding
Dataset yang digunakan merupakan kumpulan film Indonesia dari IMDb dengan penambahan ID unik untuk film.  Data ini dapat diunduh melalui Kaggle. 
Informasi Dataset:

Jenis | Keterangan
--- | ---
Title | Database Film Indonesia
Source | [Kaggle](https://www.kaggle.com/datasets/haryodwi/database-film-indonesia/data)
Maintainer | [Haryo Dwi](https://www.kaggle.com/haryodwi)
License | Data files © Original Authors
Visibility | Public
Tags | Business
Usability | 4.12

### Variabel-variabel pada Dataset Daftar Harga Rumah di Kota Bandung adalah sebagai berikut:
Variabel-variabel pada Database Film Indonesia adalah sebagai berikut:
- `movie_id`: ID unik film sesuai dengan yang ada di IMDb.
- `title`: Judul film.
- `year`: Tahun rilis film.
- `description`: Sinopsis singkat mengenai film.
- `genre`: Genre film.
- `rating`: Rating usia film.
- `users_rating`: Rata-rata rating dari pengguna yang memberikan ulasan.
- `votes`: Jumlah pengguna yang memberikan rating terhadap film.
- `languages`: Bahasa yang digunakan dalam film.
- `directors`: Nama sutradara film.
- `actors`: Daftar pemeran utama dalam film.
- `runtime`: Durasi film dalam menit.
- `user_id`: ID unik pengguna.

## Exploratory Data Analysis
<div><img src="https://github.com/rizkidwi07/Source/raw/main/Screenshot%202025-03-05%20134943.png") width="650"/></div><br />
Terdapat 12 kolom. Di sini tidak terdapat ID unik pengguna, maka dari itu akan ditambahkan kolom `user_id`.

<div><img src="https://github.com/rizkidwi07/Source/raw/main/Screenshot%202025-03-05%20135427.png") width="650"/></div><br />
`user_id` telah ditambahkan dengan format U + 00..

<div><img src="https://github.com/rizkidwi07/Source/raw/main/Screenshot%202025-03-05%20135004.png") width="450"/></div><br />

Dari output terlihat bahwa:

- Jumlah data terdiri dari 1.272 baris dan 11 kolom.
- Terdapat missing value di beberapa kolom, diantaranya pada `description`, `genre`, `rating`, dan `runtime`.

<div><img src="https://github.com/rizkidwi07/Source/raw/main/Screenshot%202025-03-05%20135011.png") width="450"/></div><br />
Terdapat 1.272 data film yang unik dengan 16 macam genre.

<div><img src="https://github.com/rizkidwi07/Source/raw/main/Screenshot%202025-03-05%20135018.png") width="450"/></div><br />

Dari hasil fungsi describe(), diketahui bahwa skala `rating` berkisar antara 1,2 hingga 9,4.

## Data Preparation

Teknik yang digunakan dalam penyiapan data (Data Preparation) yaitu:

- Menangani Missing Values

  Pada kasus dataset ini ada beberapa kolom dengan missing values yang tidak sedikit. Sebenarnya sayang jika data missing value ini langsung di-drop begitu saja. Namun, kita tidak bisa mengidentifikasi nama data ini termasuk ke dalam kategori mana. Oleh karena itu, untuk saat ini kita akan drop saja missing value ini.
  
- Menyamakan Jenis Genre

  Sebelum masuk tahap akhir (pemodelan), kita perlu menyamakan jenis genre. Kadang, genre yang sama memiliki nama film yang berbeda. Jika dibiarkan, hal ini bisa menyebabkan bias pada data. Sebagai contoh, pada data yang digunakan dalam studi kasus ini.  

- Mengurutkan berdasarkan movie_id
  Setelah menyamakan jenis `genre`, langkah berikutnya adalah mengurutkan film berdasarkan movie_id. Ini penting untuk mempersiapkan data sebelum dimasukkan ke dalam model.

- Menghapus data duplikat
  Selanjutnya, kita hanya akan menggunakan data unik untuk dimasukkan ke dalam proses pemodelan. Oleh karena itu, kita perlu menghapus data yang duplikat dengan fungsi drop_duplicates(). Dalam hal ini, kita membuang data duplikat pada kolom `movie_id`. Dengan langkah ini, film akan terurut berdasarkan `movie_id`, dan data siap untuk diproses lebih lanjut dalam sistem rekomendasi.

- Konversi Data Series ke List
  Setelah mengurutkan film dan memasukkannya ke dalam variabel film, kita perlu melakukan konversi data dari series menjadi list. Hal ini berguna untuk mempermudah pengolahan data dalam model rekomendasi.

- Membuat dictionary
  Setelah kita memiliki list untuk `movie_id`, `movie_name`, dan `movie_genre`, langkah selanjutnya adalah membuat dictionary. Dictionary ini akan membantu kita dalam menyimpan informasi terkait pengguna dan film dengan cara yang lebih terstruktur. Dengan mengurutkan data dan membuat dictionary, struktur data menjadi lebih jelas dan terorganisir. Hal ini memudahkan dalam mengakses dan memanipulasi data, sehingga proses pengembangan model rekomendasi menjadi lebih efisien.

- Penghapusan Kolom yang tidak relevan
  Goal proyek kita kali ini adalah menghasilkan rekomendasi sejumlah film yang sesuai dengan preferensi pengguna berdasarkan rating yang telah diberikan sebelumnya. Dari data pengguna, kita akan mengidentifikasi film-film yang mirip dan belum pernah ditonton oleh pengguna untuk direkomendasikan. Dari analisis yang dilakukan, ditemukan kolom `year`, `description`, `genre`, `rating`, `votes`, `languages`, `directors`, `actors`, dan `runtime` yang tidak memberikan kontribusi signifikan terhadap model rekomendasi. Kolom-kolom ini dapat menyebabkan kebingungan dan tidak memberikan informasi tambahan yang berguna. Kolom-kolom yang tidak relevan akan dihapus dari DataFrame menggunakan metode drop(). Ini membantu menjaga dataset tetap bersih dan fokus pada fitur yang lebih penting bagi analisis dan model rekomendasi. Pembersihan data membantu meningkatkan kualitas dataset dengan menghapus atau menangani missing values, sehingga analisis yang dilakukan menjadi lebih akurat. Data yang bersih dan lengkap mengurangi kemungkinan adanya bias atau kesalahan dalam model yang dibangun.

- Representasi Fitur dengan TF-IDF
  Pada tahap ini, kita akan membangun sistem rekomendasi sederhana berdasarkan jenis genre film. TF-IDF membantu dalam menyoroti kata yang unik dalam genre tiap film, sehingga membuat representasi fitur dari setiap film dalam bentuk vektor. Implementasinya dengan cara menghitung nilai TF-IDF untuk setiap kata yang muncul dalam genre dari film. Setiap film akan direpresentasikan sebagai vektor dalam ruang fitur berbasis kata, dengan bobot TF-IDF yang menggambarkan kepentingan relatif kata tersebut.

- Menyandikan (encode) fitur
  Pada tahap ini, `user_id` dan `movie_id` akan diencode ke dalam indeks integer dan dipetakan ke dataframe yang berkaitan. Lalu, cek beberapa hal dalam data seperti jumlah user, jumlah film, kemudian mengubah nilai rating menjadi float.

## Modeling dan Result
  Pada tahap modeling ini dibuat beberapa model dengan algoritma yang berbeda-beda. Pada proyek ini akan dibuat 2 model, diantaranya:
  - Content-Based Filtering: Menggunakan TF-IDF untuk mengidentifikasi korelasi antara nama film dengan genre film dan Cosine Similarity untuk menghitung derajat kesamaan (similarity degree) antar film dengan teknik cosine similarity. Hal ini digunakan untuk menghitung kemiripan antara film berdasarkan genre.
  
  Hasil Rekomendasi:
  Sistem rekomendasi yang dikeluarkan sistem ini adalah berupa top-N recommendation. Kita akan mendapatkan rekomendasi film yang mirip dengan 'MeloDylan' dengan genre 'Drama'. Sistem memberikan rekomendasi 5 film restoran dengan kategori 'Drama'.
    Hasil Rekomendasi:
    <div><img src="https://github.com/rizkidwi07/Source/raw/main/Screenshot%202025-03-05%20142921.png") width="450"/></div><br />
    
  - Collaborative Filtering: Menggunakan Matrix Factorization untuk menemukan hubungan antara pengguna dan film berdasarkan rating yang diberikan. Pada tahap ini, model menghitung skor kecocokan antara pengguna dan film dengan teknik embedding. Pertama, lakukan proses embedding terhadap data user dan film. Selanjutnya, lakukan operasi perkalian dot product antara embedding user dan film. Selain itu, dapat menambahkan bias untuk setiap user dan film. Skor kecocokan ditetapkan dalam skala [0,1] dengan fungsi aktivasi sigmoid.
  
    Hasil Rekomendasi:
    <div><img src="https://github.com/rizkidwi07/Source/raw/main/Screenshot%202025-03-05%20142955.png") width="450"/></div><br />
    Hasil di atas adalah rekomendasi untuk user dengan id U001 dan U002. Dari output tersebut, kita dapat membandingkan antara film with high ratings from user dan Top 10 film recommendation untuk user. Perhatikanlah, beberapa film rekomendasi menyediakan kategori `genre` yang sesuai dengan rating user. Pada user dengan id U001, diperoleh 2 rekomendasi film dengan kategori 'Biography' yang sesuai dengan rating yang diberikan sebelumnya.
  

## Evaluation
Pada proyek ini, model yang dibuat merupakan kasus prediksi harga dan menggunakan metriks akurasi. Metrik evaluasi yang digunakan:

- Binary Crossentropy: Untuk menghitung loss function.
- Adam (Adaptive Moment Estimation): Sebagai optimizer, dan 
- Root Mean Squared Error (RMSE): Sebagai metrics evaluation.
  
Analisis:
Untuk mendapatkan rekomendasi film, pertama kita ambil sampel user secara acak dan definisikan variabel film_not_watched yang merupakan daftar film yang belum pernah ditonton oleh pengguna. Daftar film_not_watchd inilah yang akan menjadi film yang direkomendasikan. 

Sebelumnya, pengguna telah memberi rating pada beberapa film yang telah mereka tonton. Ggunakan rating ini untuk membuat rekomendasi film yang mungkin cocok untuk pengguna. Film yang akan direkomendasikan tentulah film yang belum pernah ditonton oleh pengguna. Oleh karena itu, buat variabel movie_not_watched sebagai daftar restoran untuk direkomendasikan pada pengguna. 

Selanjutnya, untuk memperoleh rekomendasi film, gunakan fungsi model.predict() dari library Keras dan kita dapat membandingkan antara film with high ratings from user dan Top 10 film recommendation untuk user. 

## Kesimpulan
Proyek ini berhasil mengembangkan sistem rekomendasi film menggunakan dua metode yang berbeda. Hasil evaluasi menunjukkan bahwa metode Content-Based Filtering sistem rekomendasi yang dikeluarkan sistem ini adalah berupa top-N recommendation dan metode Collaborative Filtering yang dikeluarkan adalah film with high ratings from user dan Top 10 film recommendation untuk user. 

Sistem rekomendasi ini dapat dikembangkan lebih lanjut dengan menerapkan teknik Hybrid Filtering untuk meningkatkan akurasi dan relevansi rekomendasi.
