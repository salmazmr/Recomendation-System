# Laporan Proyek Machine Learning - Salma Azmi Rasyidah
## Project Overview

Film sudah menjadi salah satu media hiburan yang populer di kalangan masyarakat. Sejak tahun 1874 sampai 2015, sebanyak 3,361,741 judul film telah dikeluarkan oleh industri perfilman (http://imdb.com). Banyak-nya judul-judul film yang telah beredar membuat masyarakat sulit untuk menemukan film yang mereka inginkan. Data-data rating film yang terdapat dalam suatu website dapat diolah dan dimanfaatkan untuk merekomen-dasikan film kepada user lain. Pertimbangan-nya adalah menemukan film berdasarkan hubungan antara satu film dan film lainnya yang sudah diberi rating oleh user untuk dijadikan rekomendasi kepada user lain. Oleh karena itu, diperlukan suatu sistem yang dapat merekomendasikan film kepada user.

Sistem rekomendasi adalah suatu mekanisme yang dapat memberikan suatu informasi atau rekomendasi sesuai dengan kesukaan user berdasarkan informasi yang diperoleh dari user (Sarwar dkk, 2001). Oleh karena itu, diperlukan model rekomendasi yang tepat agar rekomendasi yang diberikan oleh sistem sesuai dengan kesukaan user, serta mempermudah user mengambil keputusan dalam menentukan item (film) yang akan dipilih (McGinty dan Smyth, 2006). Metode rekomendasi yang digunakan dalam sistem rekomendasi adalah Collaborative Filltering dan Content Based Filtering. 


## Business Understanding

Problem Statements :
- Apakah sistem rekomendasi yang dibuat sesuai dengan genres film menurut pengguna (_user_)?
- Bagaimana cara sistem ini bekerja untuk memberikan rekomendasi film terhadap pengguna (_user_)? 

Goals :
Tujuan dituliskannya laporan ini adalah sebagai berikut:
- Untuk mengetahui pengaruh sistem rekomendasi dapat memberikan rekomendasi film yang sesuai dengan genres menurut pengguna (_user_).
- Untuk mengetahui cara kerja sistem rekomendasi yang telah dibuat dalam memberikan rekomendasi film terhadap pengguna (_user_).


Solution Approach :

Dalam merancang   dan    membangun    suatu sistem  rekomendasi  terdapat  beberapa  metode  atau pendekatan  yang  dapat  digunakan,  namun  secara garis   besar   terdapat duapendekatan   yang   dapat digunakan, yakni Collaborative-filtering dimana rekomendasi  dilakukan  berdasarkan  kebiasaan  dari pengguna    lainnyadan Content-based filtering dimana  rekomendasi  dilakukan  berdasarkan  tingkat kesamaan   antara itemyang   sebelumnya   pernah diberi like denganitemlain   oleh   pengguna   itu sendiri (Sumarlin,    dkk.,    2016).

Metode    atau pendekatan  yang  dipilih  pada  sistem  rekomendasi bergantung      pada      permasalahan      yang      akan diselesaikan.   Teknik   rekomendasi   yang   berbeda-beda dapat digunakan  untuk  aplikasi  yang  berbeda berdasarkan pada tujuan dan objektif dari aplikas iitu sendiri. Bahkan untuk satu  sistem  yang sama, dapat diterapkan lebih dari satu metode rekomendasi guna memberikan     hasil     rekomendasi     terbaik     bagi pengguna  sistem,  seperti  yang  dapat  dijumpai  di Netflix (Gomez-Uribe & Hunt, 2016).

## Data Understanding

Kumpulan data ini menjelaskan peringkat bintang 5 dan aktivitas penandaan teks bebas dari MovieLens, layanan rekomendasi film. Ini berisi 100836 peringkat dan 3683 aplikasi tag di 9742 film. Data ini dibuat oleh 610 pengguna antara 29 Maret 1996 dan 24 September 2018. Kumpulan data ini dibuat pada 26 September 2018.

Pengguna dipilih secara acak untuk dimasukkan. Semua pengguna yang dipilih telah menilai setidaknya 20 film. Tidak ada informasi demografis yang disertakan. Setiap pengguna diwakili oleh id, dan tidak ada informasi lain yang disediakan.

Data tersebut terdapat dalam file links.csv, movies.csv, ratings.csv dan tags.csv. Rincian lebih lanjut tentang isi dan penggunaan semua file berikut. Namun pada sistem rekomendasi kali ini file yang akan digunakan hanya movies.csv dan ratings.csv. 

### File dan variabel :
**Movies.csv**, adalah file yang berisi informasi tentang film yang ada pada dataset. varibael yang terdapat pada file ini diantaranya movieId, title, dan genres. Dengan penjelasan masing-masing variabel sebagai berikut :

 - movieId : Id dari film 
 - title : Judul film
 - genres : Genre film

**Ratings.csv**, adalah file yang berisi informasi tentang penilaian yang diberikan oleh pengguna. varibael yang terdapat pada file ini diantaranya movieId, userId, rating, dan timestamp. Dengan penjelasan masing-masing variabel sebagai berikut :
 - movieId : Id dari film 
 - userId : Id dari pengguna
 - rating : Penilaian dari pengguna
 - timestamp : waktu user memberi penilaian

Sumber : [link dataset](https://www.kaggle.com/datasets/shubhammehta21/movie-lens-small-latest-dataset)
### Data Loading
Tahapan yang diperlukan untuk memahami data adalah sebagai berikut:
Dilakukan dengan tujuan agar isi dataset dapat mudah dipahami. Tahapan melakukan data loading yaitu dengan meng-_import library_ yang dibutuhkan kemudian, _upoload_ data 'movies.csv', dan 'ratings.csv' pada _file storage_ google colab. Lalu, jadikan variabel baru untuk tiap _file_ dan baca dataset tersebut dengan menggunakan fungsi pandas.read_csv. Lalu kita drop kolom timestamp karena tidak digunakan pada proyek kali ini. 
```ruby
movies = pd.read_csv('/content/movies.csv')
ratings = pd.read_csv('/content/ratings.csv')
ratings = ratings.drop(columns='timestamp')
```
### Exploratory Data Analysis
Tahap eksplorasi penting untuk memahami variabel-variabel pada data serta korelasi antar variabel. Pemahaman terhadap variabel pada data dan korelasinya akan membantu kita dalam menentukan pendekatan atau algoritma yang cocok untuk data kita. 
- Movie Variable
  Pada file ini kita menjalankan fungsi info() yang mngehasilkan informasi terdapat 9742 entri dan terdapat 3 variabel, 2 bertipe object dan 1 integer.
- Ratings Variable 
  Pada file ini kita menjalankan fungsi info() yang mngehasilkan informasi terdapat 100836 entri dan terdapat 3 variabel, 1 bertipe float dan 2 integer. Selanjutnya menggunakan fungsi describe() kita mendapat banyak informasi salah satunya yaitu nilai minimum pada kolom rating adalah 0.5 dan maksimum 5.

#### Data Preprocessing
Pada tahap kali ini kita mendefinisikan ratings dalam variabel 'all_movie_rate' Lalu kita gabungkan 'all_movie_rate' dengan dataframe yang berisi data dari file movies.csv. 

## Data Preparation
### Menangani Missing Value

Gunakan fungsi isna().sum() untuk mengetahui apakah terdapat missing value pada dataset.
Selain itu, juga dapat menggunakan fungsi isnull() untuk mengembalikan nilai Boolean yang menunjukkan apakah ekspresi tidak berisi data yang valid (null). Isnull mengembalikan True jika Expression adalah null. Jika tidak, Isnull mengembalikan false.

#### Menyamakan ID film (movieId)
Selanjutnya, urutkan film berdasarkan 'MovieId' kemudian masukkan ke dalam variabel 'preparation' agar _dataframe_ terlihat lebih rapih. 

#### Konversi Data Series Menjadi List
Gunakan fungsi tolist() dari _library_ numpy untuk melakukan konversi _data series_ (MovieId, title, genres) menjadi list. Lalu, buat _dictionary_ untuk menentukan pasangan _key-value_ pada data 'movie_id', 'movie_title', dan 'movie_genres'. 


## Modeling
### Model Development - Content Based Filtering
Model yang kita gunakan kali ini adalah  Content Based Filtering. Ide dari sistem rekomendasi berbasis konten (content-based filtering) adalah merekomendasikan item yang mirip dengan item yang disukai pengguna di masa lalu. Misal, jika Anda menyukai film Ada Apa dengan Cinta, sistem akan merekomendasikan film dengan aktor utama Nicholas Saputra atau Dian Sastrowardoyo. Sistem juga akan merekomendasikan film dengan genre drama lainnya. Contoh lain, saat berbelanja di e-commerce, sistem akan merekomendasikan item yang sama dengan yang telah Anda lihat. Kita akan membahas lebih jauh mengenai teknik ini pada modul selanjutnya.

Content-based filtering mempelajari profil minat pengguna baru berdasarkan data dari objek yang telah dinilai pengguna. Algoritma ini bekerja dengan menyarankan item serupa yang pernah disukai di masa lalu atau sedang dilihat di masa kini kepada pengguna. Semakin banyak informasi yang diberikan pengguna, semakin baik akurasi sistem rekomendasi.Untuk membuat profil pengguna, dua informasi ini penting bagi sistem dengan pendekatan content-based filtering: 

 - Model preferensi pengguna.
 - Riwayat interaksi pengguna dengan sistem rekomendasi.

* TF-IDF Vectorizer
Membangun sistem rekomendasi sederhana berdasarkan genre film. Teknik TF-IDF Vectorizer akan digunakan pada sistem rekomendasi untuk menemukan representasi fitur penting dari setiap genre film. Pada proyek ini, kita juga menggunakan fungsi tfidfvectorizer() dari library sklearn.

* Cosine Similarity
Selanjutnya, kita akan menghitung derajat kesamaan (similarity degree) antar film dengan teknik cosine similarity. Di sini, kita menggunakan fungsi cosine_similarity dari library sklearn. Selanjutnya, kita akan menghitung derajat kesamaan (similarity degree) antar film dengan teknik cosine similarity. Pada tahapan ini, kita menghitung cosine similarity dataframe tfidf_matrix yang kita peroleh pada tahapan sebelumnya. Dengan satu baris kode untuk memanggil fungsi cosine similarity dari library sklearn, kita telah berhasil menghitung kesamaan (similarity) antar film. Kode di atas menghasilkan keluaran berupa matriks kesamaan dalam bentuk array.

* Mendapat Rekomendasi
Selanjutnya, kita membuat fungsi movie_recommendations dengan beberapa parameter sebagai berikut:
Nama_film : Nama film (index kemiripan dataframe).
Similarity_data : Dataframe mengenai similarity yang telah kita definisikan sebelumnya.
Items : Nama dan fitur yang digunakan untuk mendefinisikan kemiripan, dalam hal ini adalah ‘movie_name’ dan ‘genres’.
k : Banyak rekomendasi yang ingin diberikan.
Sistem rekomendasi menyatakan bahwa keluaran sistem ini adalah berupa top-N recommendation. Oleh karena itu, kita akan memberikan sejumlah rekomendasi film pada pengguna yang diatur dalam parameter k.

Selanjutnya, mari kita terapkan kode di atas untuk menemukan rekomendasi film yang mirip dengan Father of the Bride Part II (1995). Father of the Bride Part II (1995) masuk dalam kategori genre comedy. Tentu kita berharap rekomendasi yang diberikan adalah film dengan kategori yang mirip.

|title|genres|
|---|---|
|Andrew Dice Clay: Dice Rules (1991)|Comedy|
|Caddyshack (1980)|Comedy|
|Shakes the Clown (1992)|Comedy|
|Human Traffic (1999)|Comedy|
|Whipped (2000)|Comedy|

Dapat dilihat dari hasil diatas sistem kita memberikan rekomendasi 5 judul film bergenre comedy.

### Model Development - Collaborative Filtering
Teknik ini merekomendasikan item yang mirip dengan preferensi pengguna di masa lalu. Pada materi ini, kita akan menerapkan teknik collaborative filtering untuk membuat sistem rekomendasi. Teknik ini membutuhkan data rating dari user. 
* Data Understanding
Untuk memudahkan supaya tidak tertukar dengan fitur ‘rating’ pada data, kita ubah nama variabel rating menjadi df.

* Data Preparation
Pada tahap ini, melakukan persiapan data untuk menyandikan (encode) fitur ‘user’ dan ‘movieId’ ke dalam indeks integer. Selanjutnya, lakukan hal yang sama pada fitur ‘movieId’. Berikutnya, petakan userId dan movieId ke dataframe yang berkaitan. Terakhir, cek beberapa hal dalam data seperti jumlah user, jumlah film, dan mengubah nilai rating menjadi float.

* Membagi Data untuk Training dan Validasi
Acak datanya terlebih dahulu agar distribusinya menjadi random. Selanjutnya, kita bagi data train dan validasi dengan komposisi 80:20. Namun sebelumnya, kita perlu memetakan (mapping) data user dan film menjadi satu value terlebih dahulu. Lalu, buatlah rating dalam skala 0 sampai 1 agar mudah dalam melakukan proses training.

* Proses Training 
Di sini, kita membuat class RecommenderNet dengan keras Model class. Kode class RecommenderNet ini terinspirasi dari tutorial dalam situs Keras dengan beberapa adaptasi sesuai kasus yang sedang kita selesaikan. Selanjutnya, lakukan proses compile terhadap model. Model ini menggunakan Binary Crossentropy untuk menghitung loss function, Adam (Adaptive Moment Estimation) sebagai optimizer, dan root mean squared error (RMSE) sebagai metrics evaluation. Setelah dibuat _class_ 'RecommenderNet', dilakukan _compile_ terhadap model dengan learning_rate pada optimizer sebesar 0.001. Lalu, pada bagian _fit_ model, digunakan batch_size sebesar 64, dan 100 _epochs_. 

* Mendapatkan rekomendasi film
Sebelumnya, pengguna telah memberi rating pada beberapa film yang telah mereka tonton. Kita menggunakan rating ini untuk membuat rekomendasi film yang mungkin cocok untuk pengguna. Nah, film yang akan direkomendasikan tentulah film yang belum pernah tonton oleh pengguna. Oleh karena itu, kita perlu membuat variabel resto_not_visited sebagai daftar film untuk direkomendasikan pada pengguna.
Variabel movie_not_visited diperoleh dengan menggunakan operator bitwise (~) pada variabel movie_visited_by_user. Selanjutnya, untuk memperoleh rekomendasi film, gunakan fungsi model.predict() dari library Keras. 
Sebagai contoh, hasil di atas adalah rekomendasi untuk user dengan id 464. Dari output tersebut, kita dapat membandingkan antara film with high ratings from user dan Top 10 film recommendation untuk user.
Perhatikanlah, beberapa memiliki genre yang sesuai dengan rating user. Dan rekomendasi 10 film diantaranya :
Heidi Fleiss: Hollywood Madam (1995) : Documentary
Paths of Glory (1957) : Drama|War
Jonah Who Will Be 25 in the Year 2000 (Jonas qui aura 25 ans en l'an 2000) (1976) : Comedy
Stunt Man, The (1980) : Action|Adventure|Comedy|Drama|Romance|Thriller
Belle époque (1992) : Comedy|Romance
Trial, The (Procès, Le) (1962) : Drama
Adam's Rib (1949) : Comedy|Romance
Enter the Void (2009) : Drama
Bill Hicks: Revelations (1993) : Comedy
Band of Brothers (2001) : Action|Drama|War


## Evaluation

### Content Based Filtering
Model berbasis _content based filtering_ menggunakan _mectic precison_ sebagai _metrics evaluation_. Sesuai namanya, _mectic precison_ dihitung dengan cara membagi _item_ hasil rekomendasi yang relevan dengan kriteria _input_ dengan jumlah total hasil rekomendasi. Dari hasil rekomendasi di atas, diketahui bahwa Father of the Bride Part II (1995) masuk dalam kategori genre comedy. Dari 5 _item_ yang direkomendasikan, kelimanya memiliki kategori genre comedy (_similar_). Artinya, _precision_ sistem kita sebesar 5/5 atau 100% sehingga, dapat dikatakan bahwa akurasi dari model sistem rekomendasi berbasis _content based filtering_ sangat baik.

### Collaborative Filtering
Model berbasis _collaborative filtering_ menggunakan _root mean squared error_ (RMSE) sebagai _metrics evaluation_. Sesuai namanya, RMSE dihitung dengan cara mengkuadratkan _error_ (selisih antara nilai prediksi dan nilai sebenarnya), kemudian dicari rata-ratanya dengan menjumlahkan _error_ kuadrat lalu dibagi dengan banyaknya data (n). Terakhir, lakukan operasi akar agar satuan dari RMSE sama dengan satuan nilai sebenarnya. Secara matematis bisa dituliskan seperti:

![Plot](https://qph.cf2.quoracdn.net/main-qimg-57dd01d3e1e1fb051e50c162369980a5)

Berikut adalah visualisasi proses training model _Collaborative Filtering_ dari metrik evaluasi menggunakan matplotlib:

![Plot](https://raw.githubusercontent.com/salmazmr/Aplikasi-Login/main/Screenshot%20(119).png)

Dapat diliat bahwa _root mean squared error_ (RMSE) dari tiap _epoch_ pada _train_ dan _test_ data mengalami penurunan yang kurang stabil dan signifikan sehingga, akurasi dari model sistem rekomendasi berbasis _collaborative filtering_ kurang baik. 

## Kesimpulan 

Dari kedua model sistem rekomendasi tersebut (_content based filtering_ dan _collaborative filtering_) terdapat kekurangan dan kelebihan pada tiap modelnya. Model sistem rekomendasi berbasis _collaborative filtering_, kelebihanya yaitu data yang disajikan dari berbagai kategori dan sangat beragam karena dari penilaian user sebelumnya. Akan tetapi, untuk kekuranganya yaitu model terlalu kompleks dan memiliki akurasi yang kurang baik.

Sedangkan kelebihan model sistem rekomendasi berbasis _content based filtering_ adalah bentuk modelnya yang sederhana serta memiliki akurasi yang sangat baik dan untuk kekurangannya yaitu data yang disajikan kurang beragam karena hanya berdasarkan persamaan kategori saja. 

Sehingga, pada proyek kali ini saya lebih memilih menggunakan model berbasis _content based filtering_ untuk memberikan rekomendasi tempat wisata di Indonesia kepada _user_.

## Daftar Refrensi 

[1] Halim, Arwin dkk. "Sistem Rekomendasi Film menggunakan Bisecting K-Means dan Collaborative Filtering". 2017. Tersedia: [tautan](https://citisee.amikompurwokerto.ac.id/assets/proceedings/2017/TI08.pdf). Diakses pada 29 September 2022.





