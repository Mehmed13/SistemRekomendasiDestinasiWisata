# Laporan Proyek Machine Learning Sistem Rekomendasi Destinasi Wisata Indonesia - Muhammad Fadhil Amri

## Domain Proyek

Indonesia adalah negara yang memiliki banyak destinasi wisata. Banyak turis baik lokal maupun internasional datang untuk menikmati berbagai destinasi wisata tersebut. Destinasi wisata yang tersedia pun beragam, mulai dari yang alami hingga buatan. Setiap orang tentu memiliki preferensinya masing-masing dalam memilih destinasi wisata yang ingin ia kunjungi. Preferensi ini dapat dipengaruhi oleh faktor eksternal dan internal. Faktor eksternal yang dapat mempengaruhi preferensi di antaranya adalah karakteristik destinasi wisata dan rating orang lain atas destinasi wisata tersebut. Sementara itu, faktor internal yang dapat mempengaruhi preferensi di antaranya adalah usia dan daerah asal.

Faktor karakteristik destinasi wisata bisa dikatakan sangat menentukan kesesuaian preferensi dari turis [1] [2]. Misalnya, turis yang ingin mencari wisata alam tentu akan memilih destinasi wisata alami. Sementara itu, faktor rating terkadang cukup menggambarkan kualitas dari suatu destinasi wisata. Turis akan memiliki kecenderungan untuk mengunjungi destinasi wisata dengan rating tinggi. Di lain sisi, faktor usia juga memberikan pengaruh yang cukup signifikan. Misalnya, orang dewasa menuju tua memiliki kecenderungan untuk mencari destinasi wisata yang damai dan tidak membutuhkan banyak tenaga untuk menikmatinya. Sementara itu anak muda akan cenderung lebih mencari destinasi yang menarik, seperti memacu adrenalin atau futuristik dan tidak terlalu mempermasalahkan tenaga yang dibutuhkan untuk menikmatinya. Hal senada juga dapat dilihat dari faktor daerah asal turis. Turis yang tinggal di daerah pegunungan biasanya akan tertarik untuk mencari wisata pantai, begitupun sebaliknya. Selain itu, tak jarang masyarakat suatu daerah memiliki kecenderungan untuk mengunjungi destinasi wisata yang serupa.

Beragamnya destinasi wisata dan preferensi yang ada tak jarang membuat turis kebingungan dalam menentukan pilihan destinasi wisata yang akan mereka kunjungi. Akibatnya, turis terkadang salah memilih destinasi wisata sehingga dapat menyebabkan ketidakpuasan mereka dalam berlibur [3]. Ketidakpuasan ini dapat disebabkan oleh kurangnya kualitas destinasi maupun ketidaksesuaian destinasi dengan preferensi. Untuk itu, sistem rekomendasi destinasi wisata ini dibuat dengan harapan turis dapat terbantu dalam memilih destinasi wisata dengan tepat.

## Business Understanding

Pemiihan destinasi wisata yang kurang tepat dapat menimbulkan berbagai kerugian, baik secara materi, waktu, maupun emosi. Kerugian materi dapat dilihat pada kunjungan destinasi yang membutuhkan biaya, terutama destinasi dengan biaya yang besar, baik biaya untuk akomodasi ataupun biaya pada lokasi. Kerugian waktu tak jarang beriringan dengan kerugian materi, terutama kunjungan destinasi yang jauh dari daerah asal. Akumulasi dari kerugian materi dan waktu tadi juga dapat mempengaruhi kerugian emosi. Akibatnya adalah munculnya perasaan tidak senang saat liburan. Padahal, liburan diharapkan dapat menyenangkan hati. Dengan demikian, sistem rekomendasi wisata diharapkan dapat mengurangi kerugian-kerugian tersebut dan menciptakan pengalaman yang memuaskan turis saat berlibur.

Berdasarkan uraian latar belakang dari permasalahan pada domain dan latar belakang bisnis tersebut, dapat dijabarkan <i> problem statements </i> dan <i> goals </i> proyek ini sebagai berikut.

### Problem Statements

- Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik <i>content-based filtering</i>?
- Dengan data rating yang dimiliki, bagaimana destinasi wisata lain yang mungkin disukai dan belum pernah dikunjungi oleh pengguna dapat direkomendasikan?

### Goals

- Menghasilkan sejumlah rekomendasi destinasi wisata yang dipersonalisasi untuk pengguna dengan teknik <i>content-based filtering</i>
- Menghasilkan sejumlah rekomendasi destinasi wisata yang sesuai dengan preferensi pengguna dan belum pernah dikunjungi sebelumnya dengan teknik <i>collaborative filtering</i>

### Solution statements

- Model Sistem Rekomendasi berbasis <i>Content-Based Filtering </i>. Performa diukur dengan cara menghitung nilai <i>precision</i> dari <i>Top N recommendation</i>. Model dikatakan baik apabila memiliki nilai <i> precision </i> minimal 80% rekomendasi destinasi yang relevan dibandingkan dengan N rekomendasi yang diberikan.
- Model Sistem Rekomendasi berbasis <i>Collaboration Filtering</i>. Performa diukur dengan menghitung nilai <i>Root Mean Squared Error</i>(RMSE) dari prediksi rating pengguna. Model dikatakan cukup baik apabila nilai RMSE minimal 0.35.

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah data tempat destinasi wisata, data rating destinasi wisata, dan data pengguna. Dataset yang digunakan terdiri atas data tempat destinasi wisata dengan 437 baris dan 9 kolom, data rating dengan 10000 baris dengan 3 kolom, dan data pengguna dengan 300 baris dan 3 kolom. Dataset ini diunduh dari Kaggle Platform melalui tautan [Dataset Destinasi Wisata Indonesia](https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination).

### Variabel-variabel pada Dataset Destinasi Wisata Indonesia

1. Data Tempat Destinasi Wisata

   - Place_Id: Id tempat destinasi wisata.
   - Place_Name: Nama tempat destinasi wisata.
   - Description: Deskripsi tempat destinasi wisata.
   - Category: Kategori tempat destinasi wisata.
   - City: Kota tempat destinasi wisata.
   - Price: Harga tiket masuk tempat destinasi wisata.
   - Rating: Rata-rata rating tempat destinasi wisata.
   - Time_Minutes: Rata-rata waktu berkunjung tempat destinasi wisata.
   - Coordinate: Koordinat tempat destinasi wisata dalam JSON.
   - Lat: Koordinat lintang tempat destinasi wisata.
   - Long: Koordinat bujur tempat destinasi wisata.

2. Data Rating Destinasi Wisata

   - User_Id: Id pengguna pemberi rating
   - Place_Id: Id tempat destinasi wisata yang diberi rating
   - Place_Ratings: Rating yang diberikan pengguna

3. Data Pengguna

   - User_Id: Id pengguna
   - Location: Daerah asal pengguna
   - Age: Usia pengguna

Fitur yang dibuang mulai dari EDA hingga tahap rekomendasi adalah 'Description' dan 'Coordinate'.

### <i>Exploratory Data Analysis </i> (EDA)

- Statitika deskriptif

  Menampilkan data-data deskriptif seperti tipe data dari variabel, mean, min, max, kuartil, dan standard deviasi

- <i> Missing Value Detection </i>

  Ditemukan 232 baris yang memiliki nilai kosong pada fitur 'Time_Minutes' pada data tempat destinasi wisata. Sementara itu, tidak ditemukan baris dengan nilai kosong pada fitur dan data yang lain.

- <i> Duplicate Value Detection </i>

  Tidak ditemukan adanya nilai yang duplikat pada ketiga dataset yang digunakan.

- Distribusi Nilai

  ![Gambar Distribusi Nilai Fitur City ](https://i.ibb.co/3N7rLkv/Distribusi-Nilai-City.png)<br>
  Gambar 1. Distribusi Nilai Fitur City

  Pada gambar 1, fitur City hanya memiliki 5 nilai dengan distribusi cukup merata. Dapat dilihat juga Kota Yogyakarta dan Kota Bandung menjadi kota dengan destinasi terbanyak pada dataset.

  ![Gambar Distribusi Nilai Fitur Category](https://i.ibb.co/YWqyVcs/Distribusi-Nilai-Category.png)<br>
  Gambar 2. Distribusi Nilai Fitur Category

  Pada gambar 2, fitur Category memiliki 6 nilai dengan distribusi yang cukup timpang. Budaya, Taman Hiburan, dan Cagar Alam menjadi Category yang paling banyak dari destinasi wisata yang ada.

  ![Gambar Distribusi Nilai Fitur Price ](https://i.ibb.co/B44mLx5/Distribusi-Nilai-Price.png) <br>
  Gambar 3. Distribusi Nilai Fitur Price

  Pada gambar 3, fitur Price memiliki distribusi yang <i> right skew</i>. Dapat dilihat juga bahwa kebanyakan destinasi wisata yang ada itu gratis.

- <i>Outliers Detection </i>

  ![Gambar Box and Whisker Plot Fitur Price](https://i.ibb.co/TKrY5KF/Box-and-Whisker-Plot-Price.png) <br>
  Gambar 4. Box and Whisker Plot Fitur Price

  Pada Gambar 4, ditemukan adanya outlier pada data tempat destinasi wisata pada fitur Price yang ditandai dengan titik-titik di luar Q3. Namun, hal tersebut wajar karena harga tiket masuk semuanya masih di bawah 1 juta rupiah sehingga tidak perlu dilakukan penanganan lebih lanjut.

## Data Preparation

- <i> Missing Value Handling </i> <br>
  Data destinasi yang memiliki nilai kosong pada fitur Time Minutes diisi dengan nilai rata-rata nilai Time Minutes. Nilai rata-rata dipilih karena fitur memiliki nilai float. Selain itu, tempat-tempat yang memiliki nilai kosong adalah tempat yang terbuka dan terdapat variasi yang sangat besar dari lama waktu yang dihabiskan. Pengisian nilai kosong ini dilakukan untuk mempertahankan data agar tetap bisa digunakan dengan mendekati nilai yang paling memungkinkan.

- <i>Feature Selection</i> <br>
  Pemilihan fitur dilakukan untuk mengurangi dimensi dari data yang digunakan sehingga dapat mengurangi waktu train model. Selain itu, fitur-fitur yang akan dibuang adalah fitur-fitur yang tidak bermakna atau tidak memberikan informasi tambahan sehingga performa model dapat ditingkatkan. Fitur yang dibuang adalah Description, Rating, Coordinate, Lat, Long, Unnamed: 11, dan Unnamed: 12. Berikut justifikasi dari pembuangan fitur-fitur tersebut.

  - Description didrop karena hanya memberikan keterangan destinasi sehingga tidak berpengaruh dalam model sistem rekomendasi. Informasi penting dari description telah terdapat pada category dan city.
  - Rating didrop karena data rating yang lebih detail ada pada dataset rating sehingga data yang digunakan nantinya adalah data pada dataset rating yang mampu menggambarkan preferensi pengguna dengan lebih baik.
  - Coordinate, lat, dan long didrop karena telah terdapat fitur city yang lebih informatif untuk memberikan keterangan lokasi.
  - Unnamed: 11 dan Unnamed: 12 didrop karena tidak memberikan informasi apa-apa.

- <i>Feature Engineering </i> <br>
  Nilai pada fitur Category diubah agar memiliki nilai tanpa space. Ini dilakukan agar matriks yang dihasilkan setelah proses TF-IDF Tokenizing tidak menganggap Category yang mengandung space sebagai category yang composite. Selain itu, pada tahap <i>Feature Engineering</i> ini juga dilakukan scaling pada fitur Price dan Place_Ratings. Scaling dilakukan dengan tujuan mempercepat konvergensi dari model dan menghindari bias dari model. Secara umum, tahap <i>Feature Engineering</i> dilakukan dengan tujuan untuk meningkatkan performa model dengan memberikan data yang sesuai untuk model.

- <i> TF-IDF Vectorizer </i> <br>
  Data destinasi yang akan digunakan untuk model di-encode menggunakan one hot encoding. Pada dasarnya TF-IDF Vectorizer akan membuat nilai dari fitur akan dipecah berdasarkan space, tetapi nilai dari fitur yang ada sudah atomic sehingga perilaku TF-IDF Vectorizer akan seperti one hot encoder. Tahap ini dilakukan untuk mengolah data agar bisa digunakan untuk proses penghitungan cosine similarity.

- <i> Encoding </i> <br>
  Encoding dilakukan pada data Place Id dan User Id. Proses ini dilakukan untuk menghasilkan Id yang dense dan cocok dengan jumlah nilai unik yang ada. Nilai ini nantinya akan memudahkan tahap modeling pada model berbasis <i>collaborative filtering</i>.

## Modeling and Result

1. Model Content-Based Filtering<br>
   Pembangunan model dilakukan dengan menghitung cosine similarity dari matriks fitur. Setelah itu dibuat sebuah fungsi untuk memberikan rekomendasi dengan memanfaatkan data cosine similarity dan data masukan berupa nama tempat serta jumlah rekomendasi. Fungsi rekomendasi ini mengurutkan tempat-tempat selain masukan berdasarkan nilai cosine similarity yang terbesar. Fungsi rekomendasi ini kemudian akan mengembalikan dataframe dari top-n recommendation.

   Kelebihan: <br>

   - Rekomendasi yang diberikan memiliki kemiripan yang tinggi dengan masukan
   - Mudah diinterpretasikan
   - Waktu training yang cepat

   Kekurangan: <br>

   - Tidak mampu menangkap preferensi pengguna
   - Overhead tinggi apabila fitur memiliki banyak nilai unik

   <b>Hasil</b>

   ![Gambar Destinasi Input](https://i.ibb.co/Z8WV96z/Destinasi-Input.png) <br>
   Gambar 5. Destinasi Input <br>
   ![Gambar Top-5 Recommendation Model Content-Based Filtering](https://i.ibb.co/8YG6hmL/Top-5-Recommendation-Content-Based-Filtering.png) <br>
   Gambar 6. Top-5 Recommendation Model Content-Based Filtering

   Pada gambar 5 dan 6, dapat dilihat bahwa rekomendasi yang diberikan oleh model content-based filtering sangat relevan dengan destinasi input yang diberikan.

2. Model Collaborative Filtering <br>
   Pembangunan model diawali dengan inisialisasi struktur layer pada kelas yang merupakan turunan dari kelas ff.keras.model. Inisialisasi dilakukan dengan membuat layer bias dan layer embedding dari fitur yang digunakan dalam collaborative filtering. Fitur yang digunakan adalah Place Id dan User Id. Setelah itu, definisikan method Call pada kelas untuk menerima input dan mengembalikan prediksi nilai rating dari user terhadap suatu tempat. Tahap ini melibatkan proses perkalian dot dari vector kedua atribut yang ditambah dengan kedua biasnya. Setelah siap, model akan dicompile agar dapat digunakan. Setelah itu, dilakukan training pada model menggunakan data train.

   Kelebihan: <br>

   - Kemampuan untuk memberikan rekomendasi berdasarkan preferensi pengguna
   - Robust terhadap jumlah data dan karakteristik data yang memiliki nilai beragam

   Kekurangan: <br>

   - Waktu training yang lama
   - Membutuhkan banyak data
   - Kemiripan rekomendasi tidak sebagus model content-based filtering

<b>Hasil</b>

![Gambar Top-5 Recommendation Model Collaborative Filtering](https://i.ibb.co/LhmPV4r/Top-5-Recommendation-Collaborative-Filtering.png) <br>
Gambar 7. Top-5 Recommendation Model Collaborative Filtering

Pada gambar 7, dapat dilihat bahwa rekomendasi yang diberikan oleh model content-based filtering tidak terlalu relevan dengan destinasi input yang diberikan, tetapi mungkin lebih sesuai dengan preferensi pengguna jika ditinjau terhadap preferensi pengguna-pengguna lain yang serupa.

## Evaluation

### Metrik Evaluasi

Metrik Evaluasi yang digunakan pada proyek ini adalah <i>precision</i> dan <i> Root Mean Square Error </i> (RMSE). <br>

Formula Precision:

$$ Precision = \frac{Jumlah Rekomendasi Relevan}{Jumlah Rekomendasi} $$

Metrik Precision bekerja dengan cara menghitung persentase rekomendasi relevan yang diberikan oleh model, yaitu dengan membagi jumlah rekomendasi relevan dengan jumlah rekomendasi yang diberikan.

Formula RMSE:

$$ RMSE = \sqrt {\frac{1}{n} \sum_{i=1}^n (y*{i}-y\_{pred,i})^2}$$

Metrik RMSE bekerja dengan cara merata-ratakan jumlah dari selisih antara nilai sebenarnya dengan nilai prediksi (<i>error</i>) lalu rata-rata tersebut diakarkan sehingga nilai yang dihasilkan metrik tidak memiliki skala yang besar.

### Hasil Evaluasi Model Content-Based Filtering

Model Content-Based Filtering memiliki performa yang sangat baik dengan nilai precision 0.994

### Hasil Evaluasi Model Collaborative Filtering

![Gambar Evaluasi Model Collaborative Filtering](https://i.ibb.co/34152PF/Evaluasi-Model-Collaborative-Filtering.png)<br>
Gambar 8. Evaluasi Model Collaborative Filtering

Model Collaborative Filtering memiliki performa yang cukup baik dengan nilai RMSE pada data training 0.328 dan RMSE pada data validation 0.351

## REFERENSI

[1] &nbsp;&nbsp;&nbsp;&nbsp; Yulianto, Atun & Hadi, Wisnu & Yulianto, “Analisis Preferensi Wisatawan Terhadap Pilihan Berwisata Di Sendang Sombomerti Depok Sleman Yogyakarta”, Journal of Tourism and Economic, vol. 6, no. 2, pp. 143–152, Dec. 2023.

[2] &nbsp;&nbsp;&nbsp;&nbsp; Yusendra, M. Ariza Eka, "ANALISIS FAKTOR-FAKTOR YANG MEMPENGARUHI KEPUTUSAN PEMILIHAN DESTINASI WISATA BAGI WISATAWAN DOMESTIK NUSANTARA", Jurnal Magister Manajemen, Vol.01, No.1, Jan 2015.

[3] &nbsp;&nbsp;&nbsp;&nbsp; Sari, Putri & Lestari, Yuliani, "Determinants of Tourist Satisfaction and Dissatisfaction on Tourism Village", Jurnal Pendidikan Ekonomi Dan Bisnis (JPEB), vol. 09, pp. 09-24, Mar 2021.
