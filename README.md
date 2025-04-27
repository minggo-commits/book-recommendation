# Laporan Proyek Machine Learning - Muh. Arsan Akbar

## Project Overview
Seiring dengan berkembangnya teknologi informasi, banyak aplikasi manajemen buku di perpustakaan kini telah menyediakan berbagai koleksi buku dalam format digital yang dapat diakses secara daring. Aksesibilitas ini mendorong kebutuhan akan fitur pencarian buku yang lebih cerdas, salah satunya melalui penerapan sistem rekomendasi. Sistem rekomendasi berfungsi untuk membantu pengguna menemukan buku yang sesuai dengan minat dan preferensi mereka, dengan cara memberikan saran berdasarkan masukan atau kriteria tertentu yang mereka tentukan. Seperti yang dijelaskan oleh Murti et al. (2019), sistem rekomendasi merupakan teknik yang bertujuan untuk menyarankan item pilihan yang paling relevan bagi pengguna.

Contohnya, pada aplikasi E-Library di Perpustakaan Politeknik Negeri Banyuwangi, sistem pencarian telah dibangun untuk memudahkan pengguna dalam menemukan buku yang diinginkan. Namun, dalam praktiknya ditemukan bahwa akurasi pencarian terkadang kurang optimal. Misalnya, ketika pengguna mengetikkan kata kunci berupa judul lengkap sebuah buku, sistem tidak selalu mampu menampilkan hasil yang sesuai, bahkan menampilkan pesan "Data Kosong", padahal buku tersebut sebenarnya tersedia dalam basis data. Permasalahan ini menunjukkan pentingnya penggunaan metode atau algoritma pencarian yang lebih efektif untuk meningkatkan kinerja sistem rekomendasi (Sadesty Rahmadhani et al. 2024).

Berkaitan dengan permasalahan tersebut, dalam proyek ini dilakukan pengembangan sistem rekomendasi buku berbasis Content-Based Filtering dan Collaborative Filtering menggunakan dataset Book-Crossing. Proyek ini bertujuan untuk mengoptimalkan proses pencarian dan penemuan buku, dengan memberikan rekomendasi yang lebih relevan berdasarkan konten buku maupun pola interaksi pengguna sebelumnya. Dengan pendekatan ini, diharapkan pengguna dapat menerima saran buku yang sesuai dengan preferensi mereka, meskipun tidak memasukkan kata kunci secara persis. Selain itu, proyek ini juga mengkaji penerapan model berbasis machine learning dan deep learning untuk meningkatkan akurasi serta kualitas rekomendasi yang dihasilkan.

Referensi: 
[Murti, H., Lestariningsih, E., & ., S. (2019). PERANCANGAN SISTEM REKOMENDASI BUKU PADA KATALOG PERPUSTAKAAN MENGGUNAKAN PENDEKATAN CONTENT-BASED FILTERING DAN ALGORITMA FP-GROWTH. SINTAK, 3. Retrieved from https://www.unisbank.ac.id/ojs/index.php/sintak/article/view/7643](https://www.unisbank.ac.id/ojs/index.php/sintak/article/view/7643)

[Sadesty Rahmadhani, Lutfi Hakim, & Galih Hendra Wibowo. (2024). Sistem Rekomendasi Penelusuran Buku Berbasis Content-Based Filtering dengan Pembobotan TF-RF. Jurnal Informatika Polinema, 10(4), 491–500. https://doi.org/10.33795/jip.v10i4.5565](https://jurnal.polinema.ac.id/index.php/jip/article/view/5565)

---

## Business Understanding
### Problem Statements
- Berdasarkan data pengguna, bagaimana cara membangun sistem rekomendasi buku yang dipersonalisasi menggunakan teknik Content-Based Filtering?
- Dengan memanfaatkan data rating yang tersedia, bagaimana sistem dapat merekomendasikan buku lain yang mungkin disukai oleh pengguna dan belum pernah mereka baca sebelumnya?

### Goals
- Menghasilkan rekomendasi buku yang dipersonalisasi sesuai preferensi pengguna menggunakan teknik Content-Based Filtering.
- Menghasilkan rekomendasi buku yang sesuai dengan minat pengguna dan belum pernah dibaca sebelumnya menggunakan teknik Collaborative Filtering.

### Solution Approach
- **Content-Based Filtering:**
Pendekatan ini merekomendasikan buku berdasarkan kemiripan konten antara buku yang sudah diketahui disukai pengguna dengan buku lain dalam koleksi. Algoritma yang digunakan adalah Word2Vec untuk membangun representasi vektor dari fitur buku. Model kemudian dievaluasi dengan pendekatan perhitungan presisi secara manual untuk mengukur relevansi rekomendasi yang dihasilkan.
- **Collaborative Filtering:**
Pendekatan ini merekomendasikan buku dengan memanfaatkan pola interaksi antar pengguna. Sistem akan mengidentifikasi pengguna lain yang memiliki pola perilaku serupa dan merekomendasikan buku-buku yang disukai oleh pengguna tersebut. Teknik yang digunakan adalah matrix factorization, khususnya metode Singular Value Decomposition (SVD), untuk memprediksi rating atau ketertarikan terhadap buku. Untuk mengoptimalkan model, dilakukan hyperparameter tuning menggunakan metode grid search, sehingga diperoleh kombinasi parameter terbaik. Evaluasi model dilakukan menggunakan metrik Root Mean Squared Error (RMSE) untuk mengukur akurasi prediksi rating.
---

## Data Understanding
Dataset yang digunakan dalam proyek ini adalah Book-Crossing Dataset. Dataset ini berisi informasi tentang pengguna, buku, dan rating yang diberikan oleh pengguna terhadap buku. Dataset ini dapat diunduh melalui tautan berikut: 
[Book-Crossing Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset?select=Ratings.csv) atau https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset?select=Ratings.csv

**Jumlah data dan kondisinya:**
- **Users** 
Berisi 278.858 data pengguna dengan 3 kolom. Kolom Age memiliki banyak nilai kosong (sekitar 40% data tidak tersedia).
- **Books** 
Berisi 271.360 data buku dengan 8 kolom. Terdapat beberapa missing value pada kolom Book-Author, Publisher, dan Image-URL-L.
- **Ratings**
Berisi 1.149.780 data rating buku dengan 3 kolom. Tidak ditemukan missing value pada tabel ini.

Dataset ini terdiri dari tiga tabel utamay yakni Users, Books, dan Ratings, yang saling berelasi melalui User-ID dan ISBN.

**Variabel pada dataset**

| Dataset  | Variabel              | Deskripsi |
|:---------|:----------------------|:----------|
| Users    | User-ID                | ID unik untuk setiap pengguna |
|          | Location               | Lokasi pengguna|
|          | Age                    | Usia pengguna |
| Books    | ISBN                   | Nomor ISBN buku |
|          | Book-Title             | Judul buku. |
|          | Book-Author            | Nama penulis buku|
|          | Year-Of-Publication    | Tahun terbit buku. |
|          | Publisher              | Nama penerbit buku. |
|          | Image-URL-S            | URL gambar sampul berukuran kecil. |
|          | Image-URL-M            | URL gambar sampul berukuran sedang. |
|          | Image-URL-L            | URL gambar sampul berukuran besar. |
| Ratings  | User-ID                | ID pengguna yang memberikan rating. |
|          | ISBN                   | ISBN buku yang dinilai. |
|          | Book-Rating            | Rating yang diberikan (0 untuk implicit, 1-10 untuk explicit rating). |

### Exploratory Data Analysis (EDA) - Univariate Exploratory Data Analysis

**Dataset Buku**
- Variabel ISBN
Merupakan kode unik yang digunakan untuk mengidentifikasi setiap buku secara individual. Dalam dataset ini, seluruh 271.360 baris memiliki nilai ISBN yang unik tanpa nilai kosong.
- Variabel Book Title
Menunjukkan judul buku. Dari 271.360 data, terdapat 242.135 judul unik, menunjukkan adanya beberapa buku yang memiliki judul yang sama (kemungkinan edisi berbeda atau re-publish). Judul yang paling sering muncul antara lain Selected Poems, Little Women, dan Wuthering Heights.
- Variabel Book Author
Menampilkan nama penulis dari setiap buku. Terdapat 102.022 penulis unik dan hanya 2 nilai yang hilang. Penulis yang paling sering muncul dalam dataset ini adalah Agatha Christie, William Shakespeare, dan Stephen King, yang masing-masing memiliki ratusan judul.
- Variabel Year of publication
Merupakan tahun terbit buku yang awalnya bertipe objek dan telah dikonversi menjadi numerik. Rentangnya sangat luas (0 hingga 2050), menunjukkan adanya outlier atau data error. Tahun yang paling umum muncul adalah antara 1998 hingga 2002.
- Variabel Publisher
Menunjukkan penerbit buku. Terdapat 16.807 penerbit unik dengan hanya 2 data yang hilang. Beberapa penerbit yang paling banyak muncul adalah Harlequin, Silhouette, dan Pocket.
- Variabel Image URL
URL gambar sampul buku dalam tiga ukuran berbeda. Kolom ini tidak memiliki nilai kosong, kecuali Image-URL-L yang memiliki 3 missing value. Data ini bersifat pelengkap dan tidak terlalu dibutuhkan untuk sistem rekomendasi ini.

**Dataset Pengguna**
- Variabel User-ID
Terdapat 278.858 nilai unik pada kolom User-ID, menandakan bahwa setiap pengguna dalam dataset bersifat unik. Tidak ditemukan nilai yang hilang pada kolom ini.
- Variabel Location
Terdapat 57.339 lokasi unik dalam kolom Location, yang menunjukkan keragaman geografis pengguna. Lokasi yang paling sering muncul adalah London, England, United Kingdom sebanyak 2.506 kali, diikuti oleh Toronto, Ontario, Canada dan Sydney, New South Wales, Australia. Tidak terdapat data yang hilang pada kolom ini.
- Variabel Age
Dari total data, hanya 168.096 (sekitar 60%) yang memiliki nilai usia. Rata-rata usia pengguna adalah sekitar 35 tahun dengan simpangan baku 14,43 tahun. Terdapat nilai ekstrim seperti usia minimum 0 tahun dan maksimum 244 tahun yang kemungkinan merupakan kesalahan input atau outlier.

**Dataset Rating**
- Variabel User-ID
Terdapat 105.283 pengguna unik dalam dataset ini, dan tidak ditemukan nilai yang hilang pada kolom User-ID.
- Variabel ISBN
Jumlah ISBN unik yang tercatat adalah 340.556. ISBN yang paling sering muncul adalah 0971880107 sebanyak 2.502 kali, diikuti oleh 0316666343 sebanyak 1.295 kali. Tidak ada nilai kosong pada kolom ini.
- Variabel Book Rating
Rating buku memiliki 1.149.780 entri dengan nilai rata-rata 2.87 dan standar deviasi sebesar 3.85. Nilai rating berkisar antara 0 hingga 10. Sebanyak 716.109 entri (sekitar 62%) memiliki rating 0, yang biasanya menandakan tidak ada rating yang diberikan. Rating 10 muncul sebanyak 78.610 kali, menunjukkan sejumlah pengguna memberikan skor maksimal. Distribusi lainnya menunjukkan peningkatan jumlah data seiring naiknya rating, terutama dari nilai 5 hingga 8.

**Beberapa Variabel yang perlu perbaikan**

| Dataset          | Variabel               | Masalah Ditemukan                                | Tindakan Preprocessing                                 |
|------------------|------------------------|--------------------------------------------------|--------------------------------------------------------|
| Buku             | Book-Title            | Terdapat judul buku yang sama                         | Hapus judul buku yang sama                        |
| Buku             | Book-Author            | Terdapat 2 missing value                         | Hapus baris dengan nilai kosong                        |
| Buku             | Publisher              | Terdapat 2 missing value                         | Hapus baris dengan nilai kosong                        |
| Buku             | Year-Of-Publication    | Nilai tidak valid: 0 dan > 2025                  | Menggantinya dengan nilai median |
| Buku             |  Image-URL | Variabel tidak dibutuhkan                  | Menghapus variabel (drop) |
| Pengguna         | Age                    | Nilai outlier: 0 dan > 100                       | Menggantinya dengan nilai median |

--------------------------------------------------------------------------------

## Data Preparation
Pada tahap ini dilakukan serangkaian proses pembersihan dan penyiapan data sebelum masuk ke tahap pemodelan. Berikut adalah langkah-langkah data preparation yang dilakukan, beserta alasan di balik setiap tahapan:
- Menghapus Duplikasi Data Buku
Menghapus data buku yang memiliki judul sama untuk menghindari duplikasi yang dapat mempengaruhi hasil rekomendasi. Duplikasi dapat menyebabkan bias dalam proses training model.
- Menghapus Data Buku dengan Nilai Kosong pada Kolom Penting
Menghapus data yang tidak memiliki informasi Book-Author atau Publisher, karena kedua atribut ini merupakan informasi penting untuk keakuratan sistem rekomendasi berbasis konten.
- Menghapus Kolom URL Gambar
Menghapus kolom URL gambar yang tidak diperlukan dalam proses pemodelan, sehingga data menjadi lebih ringan dan fokus pada fitur yang relevan.
- Memperbaiki Tahun Publikasi yang Tidak Valid
Nilai tahun terbit 0 atau lebih dari tahun 2025 dianggap tidak valid, sehingga diganti dengan nilai NaN, kemudian diisi dengan median dari tahun publikasi yang valid. Ini bertujuan untuk menjaga konsistensi dan realisme data.
- Memperbaiki Data Usia Pengguna yang Tidak Realistis
Usia di bawah 5 tahun atau di atas 100 tahun dianggap tidak realistis untuk pengguna perpustakaan online. Data usia yang tidak valid ini diganti dengan NaN, lalu diisi menggunakan median usia agar tetap representatif terhadap populasi pengguna.

**Hasil Setelah Data Preparation**

| Dataset        | Jumlah Data | Fitur                                           | Keterangan                                      |
|----------------|-------------|-------------------------------------------------|-------------------------------------------------|
| Books          | 242.132     | ISBN, Book-Title, Book-Author, Year-Of-Publication, Publisher | Tidak ada missing value pada semua kolom |
| Users          | 278.858     | User-ID, Location, Age                         | Tidak ada missing value setelah pengisian median pada kolom Age |

---

## Modeling
Pada tahap ini, sistem rekomendasi dikembangkan menggunakan dua pendekatan berbeda, yaitu Content-Based Filtering dan Collaborative Filtering. Masing-masing pendekatan menghasilkan daftar rekomendasi Top-N buku untuk pengguna.

### 1. Content-Based Filtering dengan Word2Vec
Pada pendekatan **Content-Based Filtering**, model dikembangkan dengan memanfaatkan algoritma **Word2Vec** untuk membangun representasi vektor dari data buku. Data teks yang digunakan adalah gabungan dari kolom **Book-Title**, **Book-Author**, dan **Publisher**. Setiap buku kemudian direpresentasikan sebagai rata-rata vektor kata-kata pembentuknya.

**Tahapan yang dilakukan:**
- Menggabungkan `Book-Title`, `Book-Author`, dan `Publisher` menjadi satu kolom teks.
- Tokenisasi teks menjadi daftar kata-kata.
- Melatih model Word2Vec untuk menghasilkan vektor representasi setiap kata.
- Menghitung vektor rata-rata dari kata-kata dalam judul buku sebagai representasi buku.
- Menggunakan cosine similarity untuk mengukur kemiripan antar buku.
- Memberikan rekomendasi Top-5 buku dengan skor kemiripan tertinggi terhadap input judul.

**Output rekomendasi:**

| ISBN        | Title                                          | Author           | Publisher                | Similarity Score |
|-------------|------------------------------------------------|------------------|---------------------------|------------------|
| 0393045218  | The Mummies of Urumchi                         | E. J. W. Barber  | W. W. Norton & Company     | 1.00000          |
| 033037401X  | Midwinter of the Spirit                        | Phil Rickman     | Pan Publishing            | 0.99991          |
| 078624660X  | Fire Ice: A Novel from the Numa Files           | Clive Cussler    | Thorndike Press            | 0.99990          |
| 0872960382  | The Sound of Thunder                           | Chu-yong Kim     | Si-sa-yong-o-sa            | 0.99989          |
| 0316734500  | The Bookseller of Kabul                        | Asne Seierstad   | Little, Brown              | 0.99989          |

**Kelebihan:**
- Dapat memberikan rekomendasi berdasarkan konten tanpa bergantung pada interaksi pengguna lain.
- Cocok untuk skenario cold-start (buku baru).

**Kekurangan:**
- Tidak dapat merekomendasikan buku yang sepenuhnya berbeda dari preferensi awal pengguna (cenderung sempit).
- Membutuhkan representasi konten yang lengkap dan akurat.



### 2. Collaborative Filtering dengan SVD (Singular Value Decomposition)

Pada pendekatan kedua, dikembangkan sistem rekomendasi berbasis **Collaborative Filtering** dengan teknik **Matrix Factorization** menggunakan algoritma **Singular Value Decomposition (SVD)**. Model ini memanfaatkan pola interaksi (rating) antara pengguna dan buku untuk memberikan rekomendasi.

**Tahapan yang dilakukan:**
- Menggunakan library `Surprise` untuk memproses data rating.
- Membuat `Dataset` dari dataframe `User-ID`, `ISBN`, dan `Book-Rating`.
- Membagi data menjadi training dan testing set (80:20).
- Melakukan **Grid Search** untuk menemukan kombinasi parameter terbaik (`n_factors`, `lr_all`, `reg_all`) berdasarkan nilai RMSE terendah.
- Melatih model SVD terbaik pada data training.
- Membuat fungsi untuk merekomendasikan buku berdasarkan prediksi rating tertinggi untuk pengguna tertentu.

**Contoh Output Rekomendasi:**

| ISBN        | Book-Title                                              | Book-Author            | Year-Of-Publication | Publisher                   | Predicted-Rating |
|-------------|----------------------------------------------------------|-------------------------|---------------------|------------------------------|------------------|
| 0140067477  | The Tao of Pooh                                           | Benjamin Hoff           | 1983.0              | Penguin Books                | 7.71             |
| 0671003755  | She's Come Undone (Oprah's Book Club (Paperback))         | Wally Lamb              | 1996.0              | Washington Square Press      | 7.22             |
| 0064471047  | The Lion, the Witch, and the Wardrobe (The Chronicles...) | C. S. Lewis             | 1994.0              | HarperCollins                | 7.07             |
| 044651652X  | The Bridges of Madison County                            | Robert James Waller     | 1992.0              | Warner Books                 | 6.99             |
| 0385722435  | Ella Minnow Pea: A Novel in Letters                       | Mark Dunn               | 2002.0              | Anchor Books/Doubleday        | 6.93             |

**Kelebihan:**
- Dapat menemukan hubungan tersembunyi antar item melalui pola rating pengguna.
- Memberikan rekomendasi yang lebih beragam dibandingkan content-based.
- Mampu menyarankan item yang tidak pernah berhubungan langsung dengan preferensi awal pengguna.

**Kekurangan:**
- Membutuhkan banyak data interaksi agar performa optimal.
- Sulit mengatasi masalah cold-start (pengguna atau buku baru yang belum pernah dinilai).

---

## Evaluasi

Pada tahap evaluasi ini, digunakan metrik yang sesuai dengan pendekatan masing-masing model rekomendasi.

### 1. Evaluasi Content-Based Filtering (Word2Vec)

Pada model Content-Based Filtering menggunakan Word2Vec, dilakukan evaluasi dengan metode **Precision** secara manual. **Precision** digunakan untuk mengukur tingkat relevansi hasil rekomendasi berdasarkan penilaian manual.

#### Metrik yang Digunakan: Precision (Manual Evaluation)

**Formula Precision:**

\[
Precision@N = \frac{\text{Jumlah item relevan yang direkomendasikan}}{N}
\]

Keterangan:
- **Item relevan**: Item rekomendasi yang dinilai sesuai berdasarkan kesamaan konten (judul, penulis, atau penerbit).
- **N**: Jumlah total rekomendasi yang dievaluasi.

#### Metode Evaluasi:
1. Mengambil 5 hasil rekomendasi teratas dari model Content-Based Filtering.
2. Menilai secara manual kesesuaian setiap rekomendasi dengan input awal.
3. Menghitung rasio jumlah rekomendasi relevan terhadap total rekomendasi.

#### Hasil Evaluasi

Contoh evaluasi untuk buku input **"The Mummies of Urumchi"**:

| Judul Buku yang Direkomendasikan                        | Relevan? |
|:---------------------------------------------------------|:--------:|
| The Mummies of Urumchi                                   | ✔️       |
| Midwinter of the Spirit                                  | ✔️       |
| Fire Ice: A Novel from the Numa Files                    | ✔️       |
| The Sound of Thunder                                     | ✔️       |
| The Bookseller of Kabul                                  | ✔️       |

- **Jumlah rekomendasi relevan**: 5 dari 5
- **Precision**: **100%**

### 2. Evaluasi Collaborative Filtering (SVD)

Pada model Collaborative Filtering menggunakan algoritma SVD (Singular Value Decomposition), digunakan metrik evaluasi **Root Mean Squared Error (RMSE)**.

#### Metrik yang Digunakan: RMSE

**Formula RMSE:**



Keterangan:


RMSE mengukur seberapa jauh prediksi model dari nilai aktual; semakin kecil RMSE, semakin baik performa model.

#### Hasil Evaluasi

- **Best RMSE** dari hasil Grid Search: **3.486**.
- Parameter terbaik yang diperoleh:
  - **n_factors**: 100
  - **lr_all**: 0.005
  - **reg_all**: 0.1
 

## Kesimpulan

- **Content-Based Filtering** menghasilkan Precision yang sangat tinggi (100%), menunjukkan akurasi tinggi dalam merekomendasikan buku serupa.
- **Collaborative Filtering (SVD)** memberikan RMSE yang cukup kecil, menunjukkan ketepatan model dalam memprediksi preferensi pengguna berdasarkan pola rating.

Kedua pendekatan memiliki keunggulannya masing-masing:
- Content-Based Filtering lebih cocok untuk menemukan item serupa dari konten.
- Collaborative Filtering lebih efektif untuk personalisasi berdasarkan perilaku pengguna.
