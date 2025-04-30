# Laporan Proyek Machine Learning - Muh. Arsan Akbar

# Project Overview
Seiring dengan berkembangnya teknologi informasi, banyak aplikasi manajemen buku di perpustakaan kini telah menyediakan berbagai koleksi buku dalam format digital yang dapat diakses secara daring. Aksesibilitas ini mendorong kebutuhan akan fitur pencarian buku yang lebih cerdas, salah satunya melalui penerapan sistem rekomendasi. Sistem rekomendasi berfungsi untuk membantu pengguna menemukan buku yang sesuai dengan minat dan preferensi mereka, dengan cara memberikan saran berdasarkan masukan atau kriteria tertentu yang mereka tentukan. Seperti yang dijelaskan oleh Murti et al. (2019), sistem rekomendasi merupakan teknik yang bertujuan untuk menyarankan item pilihan yang paling relevan bagi pengguna.

Contohnya, pada aplikasi E-Library di Perpustakaan Politeknik Negeri Banyuwangi, sistem pencarian telah dibangun untuk memudahkan pengguna dalam menemukan buku yang diinginkan. Namun, dalam praktiknya ditemukan bahwa akurasi pencarian terkadang kurang optimal. Misalnya, ketika pengguna mengetikkan kata kunci berupa judul lengkap sebuah buku, sistem tidak selalu mampu menampilkan hasil yang sesuai, bahkan menampilkan pesan "Data Kosong", padahal buku tersebut sebenarnya tersedia dalam basis data. Permasalahan ini menunjukkan pentingnya penggunaan metode atau algoritma pencarian yang lebih efektif untuk meningkatkan kinerja sistem rekomendasi (Sadesty Rahmadhani et al. 2024).

Berkaitan dengan permasalahan tersebut, dalam proyek ini dilakukan pengembangan sistem rekomendasi buku berbasis Content-Based Filtering dan Collaborative Filtering menggunakan dataset Book-Crossing. Proyek ini bertujuan untuk mengoptimalkan proses pencarian dan penemuan buku, dengan memberikan rekomendasi yang lebih relevan berdasarkan konten buku maupun pola interaksi pengguna sebelumnya. Dengan pendekatan ini, diharapkan pengguna dapat menerima saran buku yang sesuai dengan preferensi mereka, meskipun tidak memasukkan kata kunci secara persis. Selain itu, proyek ini juga mengkaji penerapan model berbasis machine learning dan deep learning untuk meningkatkan akurasi serta kualitas rekomendasi yang dihasilkan.

Referensi: 

[Murti, H., Lestariningsih, E., & ., S. (2019). PERANCANGAN SISTEM REKOMENDASI BUKU PADA KATALOG PERPUSTAKAAN MENGGUNAKAN PENDEKATAN CONTENT-BASED FILTERING DAN ALGORITMA FP-GROWTH. SINTAK, 3. Retrieved from https://www.unisbank.ac.id/ojs/index.php/sintak/article/view/7643](https://www.unisbank.ac.id/ojs/index.php/sintak/article/view/7643)

[Sadesty Rahmadhani, Lutfi Hakim, & Galih Hendra Wibowo. (2024). Sistem Rekomendasi Penelusuran Buku Berbasis Content-Based Filtering dengan Pembobotan TF-RF. Jurnal Informatika Polinema, 10(4), 491–500. https://doi.org/10.33795/jip.v10i4.5565](https://jurnal.polinema.ac.id/index.php/jip/article/view/5565)

---

# Business Understanding
## Problem Statements
- Berdasarkan data pengguna, bagaimana cara membangun sistem rekomendasi buku yang dipersonalisasi menggunakan teknik Content-Based Filtering?
- Dengan memanfaatkan data rating yang tersedia, bagaimana sistem dapat merekomendasikan buku lain yang mungkin disukai oleh pengguna dan belum pernah mereka baca sebelumnya?

## Goals
- Menghasilkan rekomendasi buku yang dipersonalisasi sesuai preferensi pengguna menggunakan teknik Content-Based Filtering.
- Menghasilkan rekomendasi buku yang sesuai dengan minat pengguna dan belum pernah dibaca sebelumnya menggunakan teknik Collaborative Filtering.

## Solution Approach
### Content-Based Filtering
Pendekatan ini merekomendasikan buku berdasarkan kemiripan konten antara buku yang sudah diketahui disukai pengguna dengan buku lain dalam koleksi. Algoritma yang digunakan adalah Word2Vec untuk membangun representasi vektor dari fitur buku. Model kemudian dievaluasi dengan pendekatan perhitungan presisi secara manual untuk mengukur relevansi rekomendasi yang dihasilkan.
### Collaborative Filtering 
Pendekatan ini merekomendasikan buku dengan memanfaatkan pola interaksi antar pengguna. Sistem akan mengidentifikasi pengguna lain yang memiliki pola perilaku serupa dan merekomendasikan buku-buku yang disukai oleh pengguna tersebut. Teknik yang digunakan adalah matrix factorization, khususnya metode Singular Value Decomposition (SVD), untuk memprediksi rating atau ketertarikan terhadap buku. Untuk mengoptimalkan model, dilakukan hyperparameter tuning menggunakan metode grid search, sehingga diperoleh kombinasi parameter terbaik. Evaluasi model dilakukan menggunakan metrik Root Mean Squared Error (RMSE) untuk mengukur akurasi prediksi rating.

---

# Data Understanding
Dataset yang digunakan dalam proyek ini adalah Book-Crossing Dataset. Dataset ini berisi informasi tentang pengguna, buku, dan rating yang diberikan oleh pengguna terhadap buku. Dataset ini dapat diunduh melalui tautan berikut: 
[Book-Crossing Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset?select=Ratings.csv) atau https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset?select=Ratings.csv

## Jumlah data dan kondisinya
- Dataset user berisi 278.858 data pengguna dengan 3 kolom, Kolom Age memiliki banyak nilai kosong (sekitar 40% data tidak tersedia).
- Dataset book berisi 271.360 data buku dengan 8 kolom, terdapat beberapa missing value pada kolom Book-Author, Publisher, dan Image-URL-L.
- Dataset ratings Berisi 1.149.780 data rating buku dengan 3 kolom dan tidak ditemukan missing value pada tabel ini.

Dataset ini terdiri dari tiga tabel utama yakni Users, Books, dan Ratings, yang saling berelasi melalui User-ID dan ISBN.

## Variabel pada dataset

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

# Exploratory Data Analysis (EDA) - Univariate Exploratory Data Analysis

## Dataset Buku
- Variabel ISBN merupakan kode unik yang digunakan untuk mengidentifikasi setiap buku secara individual. Dalam dataset ini, seluruh 271.360 baris memiliki nilai ISBN yang unik tanpa nilai kosong.
- Variabel Book Title merupakan judul buku. Dari 271.360 data, terdapat 242.135 judul unik, menunjukkan adanya beberapa buku yang memiliki judul yang sama (kemungkinan edisi berbeda atau re-publish). Judul yang paling sering muncul antara lain Selected Poems, Little Women, dan Wuthering Heights.
- Variabel Book Author menampilkan nama penulis dari setiap buku. Terdapat 102.022 penulis unik dan hanya 2 nilai yang hilang. Penulis yang paling sering muncul dalam dataset ini adalah Agatha Christie, William Shakespeare, dan Stephen King, yang masing-masing memiliki ratusan judul.
- Variabel Year of publication merupakan tahun terbit buku yang awalnya bertipe objek dan telah dikonversi menjadi numerik. Rentangnya sangat luas (0 hingga 2050), menunjukkan adanya outlier atau data error. Tahun yang paling umum muncul adalah antara 1998 hingga 2002.
- Variabel Publisher menunjukkan penerbit buku. Terdapat 16.807 penerbit unik dengan hanya 2 data yang hilang. Beberapa penerbit yang paling banyak muncul adalah Harlequin, Silhouette, dan Pocket.
- Variabel Image URL merupakan URL gambar sampul buku dalam tiga ukuran berbeda. Kolom ini tidak memiliki nilai kosong, kecuali Image-URL-L yang memiliki 3 missing value. Data ini bersifat pelengkap dan tidak terlalu dibutuhkan untuk sistem rekomendasi ini.

## Dataset Pengguna
- Variabel User-ID terdapat 278.858 nilai unik pada kolom User-ID, menandakan bahwa setiap pengguna dalam dataset bersifat unik. Tidak ditemukan nilai yang hilang pada kolom ini.
- Variabel Location terdapat 57.339 lokasi unik dalam kolom Location, yang menunjukkan keragaman geografis pengguna. Lokasi yang paling sering muncul adalah London, England, United Kingdom sebanyak 2.506 kali, diikuti oleh Toronto, Ontario, Canada dan Sydney, New South Wales, Australia. Tidak terdapat data yang hilang pada kolom ini.
- Variabel Age, Dari total data, hanya 168.096 (sekitar 60%) yang memiliki nilai usia. Rata-rata usia pengguna adalah sekitar 35 tahun dengan simpangan baku 14,43 tahun. Terdapat nilai ekstrim seperti usia minimum 0 tahun dan maksimum 244 tahun yang kemungkinan merupakan kesalahan input atau outlier.

## Dataset Rating
- Variabel User-ID terdapat 105.283 pengguna unik dalam dataset ini, dan tidak ditemukan nilai yang hilang pada kolom User-ID.
- Variabel ISBN, jumlah ISBN unik yang tercatat adalah 340.556. ISBN yang paling sering muncul adalah 0971880107 sebanyak 2.502 kali, diikuti oleh 0316666343 sebanyak 1.295 kali. Tidak ada nilai kosong pada kolom ini.
- Variabel Book Rating memiliki 1.149.780 entri dengan nilai rata-rata 2.87 dan standar deviasi sebesar 3.85. Nilai rating berkisar antara 0 hingga 10. Sebanyak 716.109 entri (sekitar 62%) memiliki rating 0, yang biasanya menandakan tidak ada rating yang diberikan. Rating 10 muncul sebanyak 78.610 kali, menunjukkan sejumlah pengguna memberikan skor maksimal. Distribusi lainnya menunjukkan peningkatan jumlah data seiring naiknya rating, terutama dari nilai 5 hingga 8.

## Beberapa Variabel yang perlu perbaikan

| Dataset          | Variabel               | Masalah Ditemukan                                | Tindakan Preprocessing                                 |
|------------------|------------------------|--------------------------------------------------|--------------------------------------------------------|
| Buku             | Book-Title            | Terdapat judul buku yang sama                         | Hapus judul buku yang sama                        |
| Buku             | Book-Author            | Terdapat 2 missing value                         | Hapus baris dengan nilai kosong                        |
| Buku             | Publisher              | Terdapat 2 missing value                         | Hapus baris dengan nilai kosong                        |
| Buku             | Year-Of-Publication    | Nilai tidak valid: 0 dan > 2025                  | Menggantinya dengan nilai median |
| Buku             |  Image-URL | Variabel tidak dibutuhkan                  | Menghapus variabel (drop) |
| Pengguna         | Age                    | Nilai outlier: 0 dan > 100                       | Menggantinya dengan nilai median |

--------------------------------------------------------------------------------

# Data Preparation

Pada tahap ini dilakukan serangkaian proses pembersihan dan penyiapan data sebelum masuk ke tahap pemodelan. Data preparation penting untuk memastikan data yang digunakan sudah bersih, konsisten, relevan, dan siap mendukung performa model. Berikut langkah-langkah data preparation yang dilakukan:

## Handling Missing Value

### Book-Author dan Publisher

Baris data yang memiliki nilai kosong pada kolom `Book-Author` atau `Publisher` dihapus, karena kedua atribut ini mengandung informasi penting yang digunakan dalam sistem rekomendasi berbasis konten. Kehilangan informasi ini dapat menyebabkan penurunan akurasi rekomendasi.

```python
book = book[book['Book-Author'].notnull()]
book = book[book['Publisher'].notnull()]
```

### Year-Of-Publication

Nilai `Year-Of-Publication` yang tidak valid (bernilai 0 atau lebih dari 2025) diganti dengan `NaN`, lalu diisi menggunakan median tahun publikasi yang valid. Hal ini dilakukan untuk menjaga konsistensi data dan menghindari bias pada fitur tahun terbit yang dapat mempengaruhi kualitas rekomendasi.

```python
book.loc[(book['Year-Of-Publication'] == 0) | (book['Year-Of-Publication'] > 2025), 'Year-Of-Publication'] = np.nan
median_year = book['Year-Of-Publication'].median()
book['Year-Of-Publication'].fillna(median_year, inplace=True)
```

### Age

Data pengguna dengan `Age` di bawah 5 tahun atau di atas 100 tahun dianggap tidak realistis dan dapat menjadi outlier yang merusak analisis. Oleh karena itu, nilai yang tidak masuk akal diganti menjadi `NaN`, kemudian diisi dengan median usia agar distribusi umur pengguna tetap representatif.

```python
user.loc[(user['Age'] < 5) | (user['Age'] > 100), 'Age'] = np.nan
median_age = user['Age'].median()
user['Age'].fillna(median_age, inplace=True)
```

## Handling Duplicates

### Book-Title

Duplikasi data berdasarkan `Book-Title` dapat menyebabkan bias dalam proses training model, seperti pemberian bobot lebih terhadap buku tertentu. Oleh karena itu, dilakukan penghapusan data duplikat agar sistem rekomendasi tidak berat sebelah.

```python
book = book.drop_duplicates(subset=['Book-Title']).reset_index(drop=True)
```

## Feature Reduction

### Penghapusan Kolom Gambar

Kolom `Image-URL-S`, `Image-URL-M`, dan `Image-URL-L` tidak digunakan dalam proses pemodelan berbasis teks dan rating. Menghapus fitur yang tidak relevan membantu mengurangi noise dan mempercepat proses training model.

```python
book.drop(['Image-URL-S', 'Image-URL-M', 'Image-URL-L'], axis=1, inplace=True)
```

# Content-Based Filtering Preparation

## Ekstraksi Fitur Text Data

Untuk pendekatan content-based filtering, diperlukan representasi teks dari buku. Oleh karena itu, kolom `Book-Title`, `Book-Author`, dan `Publisher` digabungkan menjadi satu kolom `text_data`, yang nantinya diolah lebih lanjut menggunakan teknik pembelajaran representasi teks.

```python
text_data = book['Book-Title'] + ' ' + book['Book-Author'] + ' ' + book['Publisher']
```

## Tokenisasi dan Training Word2Vec

Model Word2Vec dilatih menggunakan data tokenisasi `text_data`. Word2Vec membantu merepresentasikan teks dalam bentuk vektor numerik yang menangkap hubungan semantik antar kata, sehingga model rekomendasi dapat memahami konteks konten lebih baik.

```python
sentences = [text.split() for text in text_data]
model = Word2Vec(sentences, vector_size=100, window=5, min_count=1)
```

# Collaborative Filtering Preparation

## Filter Data Ratings

Filtering data rating dilakukan untuk memastikan bahwa hanya data yang cukup sering muncul yang digunakan dalam training. User atau buku dengan sedikit rating kurang memberikan informasi berguna untuk membangun model collaborative filtering yang andal.

```python
filtered_ratings = rating[
    (rating['User-ID'].isin(rating['User-ID'].value_counts()[rating['User-ID'].value_counts() > 20].index)) &
    (rating['ISBN'].isin(rating['ISBN'].value_counts()[rating['ISBN'].value_counts() > 20].index))
]
```

## Encode Label

Data rating perlu dikonversi ke dalam format standar yang bisa diproses oleh library rekomendasi (dalam hal ini Surprise). Encoding label memastikan bahwa `User-ID` dan `ISBN` dalam format numerik, dan skala rating didefinisikan dengan benar (0-10).

```python
reader = Reader(rating_scale=(0, 10))
data = Dataset.load_from_df(filtered_ratings[['User-ID', 'ISBN', 'Book-Rating']], reader)
```

## Split Data

Data dibagi menjadi set pelatihan (`trainset`) dan pengujian (`testset`) untuk memungkinkan validasi model secara objektif. Dengan memisahkan data, kita dapat mengevaluasi performa model terhadap data yang belum pernah dilihat sebelumnya.

```python
trainset, testset = train_test_split(data, test_size=0.2, random_state=42)
```


## Hasil Setelah Data Preparation

| Dataset        | Jumlah Data | Fitur                                           | Keterangan                                      |
|----------------|-------------|-------------------------------------------------|-------------------------------------------------|
| Books          | 242.132     | ISBN, Book-Title, Book-Author, Year-Of-Publication, Publisher | Tidak ada missing value pada semua kolom |
| Users          | 278.858     | User-ID, Location, Age                         | Tidak ada missing value setelah pengisian median pada kolom Age |

---

# Modeling and Results

Pada tahap ini, sistem rekomendasi dikembangkan menggunakan dua pendekatan berbeda, yaitu **Content-Based Filtering** dan **Collaborative Filtering**. Setiap pendekatan menghasilkan daftar rekomendasi Top-N buku untuk pengguna, berdasarkan mekanisme kerja dan parameter model yang telah ditentukan.

---

## 1. Content-Based Filtering dengan Word2Vec dan Cosine Similarity

Pada pendekatan **Content-Based Filtering**, sistem rekomendasi dibangun berdasarkan kemiripan konten antar buku. Representasi buku dibentuk menggunakan teknik **Word2Vec**, dan kemiripan antar buku dihitung menggunakan **cosine similarity**.

### Cara Kerja Model

- Menggabungkan teks dari kolom `Book-Title`, `Book-Author`, dan `Publisher` menjadi satu kolom teks gabungan.
- Melakukan tokenisasi teks menjadi daftar kata.
- Melatih model **Word2Vec** untuk menghasilkan vektor representasi tiap kata dengan parameter utama:
  - `vector_size=100`: Dimensi vektor embedding.
  - `window=5`: Ukuran jendela konteks.
  - `min_count=1`: Kata dengan frekuensi minimal 1 disertakan.
- Untuk setiap buku, menghasilkan vektor representasi dengan mengambil rata-rata vektor kata-katanya.
- Mengukur kemiripan antar buku menggunakan **cosine similarity**.
- Menyusun rekomendasi **Top-5** buku dengan skor cosine similarity tertinggi terhadap buku input.

### Output Rekomendasi (Contoh)

| ISBN        | Title                    | Author            | Publisher                | Similarity Score |
|-------------|---------------------------|-------------------|---------------------------|------------------|
| 0393045218  | The Mummies of Urumchi     | E. J. W. Barber   | W. W. Norton & Company    | 1.00000          |
| 3791535714  | Die Schildbürger           | Erich Kästner     | Dressler Verlag           | 0.99991          |
| 033037401X  | Midwinter of the Spirit    | Phil Rickman      | Pan Publishing            | 0.99990          |
| 0965813509  | The Throne of Bones        | Brian McNaughton  | Terminal Fright           | 0.99990          |
| 0316734500  | The Bookseller of Kabul    | Asne Seierstad    | Little, Brown             | 0.99990          |

### Kelebihan
- Dapat memberikan rekomendasi hanya berdasarkan konten buku, tanpa bergantung pada data pengguna lain.
- Cocok untuk mengatasi masalah **cold-start** pada buku baru.

### Kekurangan
- Hanya dapat merekomendasikan buku yang mirip dengan buku yang sudah diketahui.
- Kualitas rekomendasi bergantung pada kelengkapan dan akurasi metadata buku.

---

## 2. Collaborative Filtering dengan SVD (Singular Value Decomposition)

Pada pendekatan **Collaborative Filtering**, sistem rekomendasi dikembangkan menggunakan teknik **Matrix Factorization** dengan algoritma **Singular Value Decomposition (SVD)**. Model ini memanfaatkan pola interaksi (rating) antara pengguna dan item.

### Cara Kerja Model

- Menggunakan library **Surprise** untuk memproses dataset `User-ID`, `ISBN`, dan `Book-Rating`.
- Membagi dataset menjadi **trainset** dan **testset** dengan rasio 80:20.
- Melakukan **Grid Search** untuk menemukan kombinasi parameter terbaik dengan evaluasi menggunakan **Root Mean Square Error (RMSE)**:
  - `n_factors`: Jumlah faktor laten (eksperimen 50 dan 100).
  - `lr_all`: Learning rate umum (eksperimen 0.005 dan 0.01).
  - `reg_all`: Regularisasi umum (eksperimen 0.02 dan 0.1).
- Melatih model **SVD** terbaik berdasarkan hasil Grid Search pada data training.
- Menggunakan model terlatih untuk memprediksi rating buku yang belum pernah dibaca oleh pengguna.
- Menyusun rekomendasi **Top-5** buku dengan prediksi rating tertinggi.

### Output Rekomendasi (Contoh)

#### Sample User ID: 11676

| ISBN         | Book Title                                                   | Book Author         | Year of Publication | Publisher                 | Predicted Rating |
|--------------|---------------------------------------------------------------|---------------------|---------------------|----------------------------|------------------|
| 193156146X   | The Time Traveler's Wife                                       | Audrey Niffenegger  | 2003.0              | MacAdam/Cage Publishing    | 10.00            |
| 0553582143   | Body of Lies                                                   | Iris Johansen       | 2003.0              | Bantam                    | 10.00            |
| 0345413881   | Dr. Death (Alex Delaware Novels (Paperback))                   | Jonathan Kellerman  | 2001.0              | Ballantine Books           | 10.00            |
| 0380720132   | The Mystery of the Cupboard (Indian in the Cupboard Adventures) | Lynne Reid Banks    | 1999.0              | HarperTrophy               | 9.99             |
| 2253044903   | Le Parfum : Histoire d'un meurtrier                             | Patrick Süskind     | 1988.0              | LGF                        | 9.95             |

### Kelebihan
- Dapat menemukan hubungan tersembunyi antar buku dari pola rating pengguna.
- Memberikan rekomendasi lebih beragam dibandingkan content-based filtering.

### Kekurangan
- Membutuhkan data interaksi pengguna yang cukup banyak untuk performa optimal.
- Kurang efektif pada kasus **cold-start** untuk pengguna baru atau buku baru.

---

# Evaluasi

Pada tahap evaluasi ini, digunakan metrik yang sesuai dengan pendekatan masing-masing model rekomendasi.

## 1. Evaluasi Content-Based Filtering (Word2Vec)

Pada model Content-Based Filtering menggunakan Word2Vec, dilakukan evaluasi dengan metode **Precision** secara manual. **Precision** digunakan untuk mengukur tingkat relevansi hasil rekomendasi berdasarkan penilaian manual.

### Metrik yang Digunakan: Precision (Manual Evaluation)

**Formula Precision:**

![Formula Precision](https://github.com/minggo-commits/book-recommendation/blob/main/Img/Formula%20Precision.PNG?raw=true)


Keterangan:
- **Item relevan**: Item rekomendasi yang dinilai sesuai berdasarkan kesamaan konten (judul, penulis, atau penerbit).
- **N**: Jumlah total rekomendasi yang dievaluasi.

### Metode Evaluasi
1. Mengambil 5 hasil rekomendasi teratas dari model Content-Based Filtering.
2. Menilai secara manual kesesuaian setiap rekomendasi dengan input awal.
3. Menghitung rasio jumlah rekomendasi relevan terhadap total rekomendasi.

### Hasil Evaluasi

Contoh evaluasi untuk buku input **"The Mummies of Urumchi"**:

| Judul Buku yang Direkomendasikan                        | Relevan? |
|:---------------------------------------------------------|:--------:|
| The Mummies of Urumchi                                   | ✔️       |
| Die SchildbÃ?Â¼rger.                                 | ✔️       |
| Midwinter of the Spirit                | ✔️       |
| The throne of bones                                  | ✔️       |
| The Bookseller of Kabul                                  | ✔️       |

- **Jumlah rekomendasi relevan**: 5 dari 5
- **Precision**: **100%**

## 2. Evaluasi Collaborative Filtering (SVD)

Pada model Collaborative Filtering menggunakan algoritma SVD (Singular Value Decomposition), digunakan metrik evaluasi **Root Mean Squared Error (RMSE)**.

### Metrik yang Digunakan: RMSE

**Formula RMSE:**

![Formula RMSE](https://github.com/minggo-commits/book-recommendation/blob/main/Img/Formula%20RMSE.PNG?raw=true)

Keterangan:

![Keterangan Formula RMSE](https://github.com/minggo-commits/book-recommendation/blob/main/Img/Ket%20Formula%20RMSE.PNG?raw=true)

RMSE mengukur seberapa jauh prediksi model dari nilai aktual; semakin kecil RMSE, semakin baik performa model.

### Hasil Evaluasi

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
