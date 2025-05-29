# Laporan Proyek Machine Learning — Nelson Lau
Proyek akhir ini merupakan bagian dari submission Dicoding dan bertujuan untuk membangun sistem rekomendasi. Sistem ini menggunakan pendekatan **Content-Based Filtering** untuk menghasilkan rekomendasi anime top-N, serta memanfaatkan algoritma **K-Nearest Neighbors (K-NN)** sebagai bagian dari model rekomendasi.

![Anime Wallpaper](https://wallpaperaccess.com/full/538986.png)

*Sumber: [WallpaperAccess](https://wallpaperaccess.com/full/538986.png) melalui [Yahoo Image Search](https://id.images.search.yahoo.com/search/images;_ylt=AwrKB7ChTTdoIwIAC_rLQwx.;_ylu=Y29sbwNzZzMEcG9zAzEEdnRpZAMEc2VjA3BpdnM-?p=Anime+wallpaper+all&fr2=piv-web&type=E210ID91215G0&fr=mcafee#id=13&iurl=https%3A%2F%2Fwallpaperaccess.com%2Ffull%2F538986.png&action=click)*

### Latar Belakang

Anime merupakan bentuk animasi yang berasal dari Jepang dan telah memiliki basis penggemar yang besar di berbagai negara. Seiring berkembangnya teknologi, berbagai judul anime kini tersedia secara luas melalui platform streaming digital. Namun, melimpahnya pilihan ini kerap membuat pengguna kesulitan memilih tontonan yang sesuai dengan selera mereka [[1]](https://link.springer.com/book/10.1007/978-0-387-85820-3). Untuk menjawab permasalahan tersebut, penelitian ini mengembangkan sistem rekomendasi yang mampu memberikan saran tontonan berdasarkan minat dan preferensi pengguna. Sistem ini akan mempertimbangkan riwayat tontonan, genre yang sering ditonton, serta rating yang diberikan oleh pengguna. Selain itu, aspek lain seperti popularitas anime, ulasan pengguna lain, dan rekomendasi dari komunitas juga akan turut dianalisis [[2]](https://dl.acm.org/doi/10.1145/2843948).

Sistem rekomendasi ini dapat memberikan keuntungan baik bagi pengguna maupun penyedia layanan. Pengguna bisa lebih mudah menemukan anime yang relevan dengan minat mereka, mengeksplorasi genre baru, dan memilih tontonan sesuai dengan suasana hati. Di sisi lain, perusahaan dapat meningkatkan keterlibatan pengguna, memperkaya katalog tontonan yang ditawarkan, serta memperoleh wawasan lebih akurat tentang preferensi pengguna [[3]](https://link.springer.com/book/10.1007/978-3-319-29659-3).

## Business Understanding

Pengembangan sistem rekomendasi anime merupakan strategi yang tidak hanya mempermudah pengguna dalam menemukan konten favorit mereka, tetapi juga menjadi alat penting untuk meningkatkan performa bisnis platform streaming. Dengan sistem ini, platform dapat meningkatkan engagement, kepuasan pengguna, serta memaksimalkan distribusi konten yang sesuai dengan target audiens [[4]](https://doi.org/10.1016/j.dss.2015.03.008).

### Problem Statements
-	Bagaimana merancang sistem rekomendasi anime berdasarkan preferensi genre pengguna?
-	Bagaimana memanfaatkan data rating pengguna untuk memberikan rekomendasi terhadap anime yang belum pernah ditonton?
-	Bagaimana membangun sistem rekomendasi dengan metode Cosine Similarity dan K-Nearest Neighbor (KNN)?
-	Bagaimana mengevaluasi performa dari model sistem rekomendasi yang telah dibuat?

### Goals
- Menghasilkan rekomendasi anime (Top-N) berdasarkan genre favorit pengguna.
- Menyediakan rekomendasi anime yang belum pernah ditonton namun memiliki kecocokan dengan preferensi pengguna.
- Membangun model rekomendasi menggunakan pendekatan Cosine Similarity dan K-Nearest Neighbor.
- Mengukur performa model rekomendasi menggunakan metrik evaluasi yang tepat.

### Solution statements
Proses dimulai dengan analisis data menggunakan pendekatan Exploratory Data Analysis untuk memahami pola dan distribusi data. Tahap selanjutnya adalah pembersihan data, termasuk menghapus nilai kosong (missing values), data duplikat, karakter alfanumerik yang tidak relevan, serta link atau URL yang tidak diperlukan. Data kategorikal akan diubah ke format numerik menggunakan teknik one-hot encoding.
Setelah model dibangun, performanya akan dievaluasi menggunakan sejumlah metrik seperti Precision guna memastikan kualitas rekomendasi yang dihasilkan [[5]](https://dl.acm.org/doi/10.1145/3386252).


## Data Understanding

Dataset yang digunakan berasal dari [Kaggle - Anime Dataset](https://www.kaggle.com/datasets/dbdmobile/myanimelist-dataset), yang berisi data anime

### EDA - Deskripsi Variabel

| Kolom          | Deskripsi                                                                                 |
|----------------|--------------------------------------------------------------------------------------------|
| anime_id       | ID unik untuk setiap entri anime dalam dataset.                                           |
| Name           | Nama asli anime (biasanya dalam bahasa Jepang).                                           |
| English name   | Nama anime dalam bahasa Inggris, jika tersedia.                                           |
| Other name     | Nama alternatif, termasuk terjemahan atau singkatan.                                      |
| Score          | Skor rata-rata dari pengguna (perlu dikonversi ke numerik).                              |
| Genres         | Genre utama seperti Action, Comedy, Romance, dll.                                         |
| Synopsis       | Ringkasan cerita anime.                                                                   |
| Type           | Format anime (TV, Movie, OVA, ONA, Special).                                              |
| Episodes       | Jumlah episode (format string, mungkin perlu dibersihkan).                               |
| Aired          | Rentang tanggal penayangan anime.                                                         |
| Premiered      | Musim dan tahun rilis, contoh: Spring 2012.                                               |
| Status         | Status penayangan (Finished, Currently Airing, Not yet aired).                           |
| Producers      | Perusahaan/individu pendanaan anime.                                                      |
| Licensors      | Pihak lisensi distribusi anime.                                                           |
| Studios        | Studio animasi yang memproduksi anime.                                                    |
| Source         | Sumber cerita (Manga, Light Novel, Game, Original, dll).                                 |
| Duration       | Durasi rata-rata per episode, contoh: 24 min. per ep.                                     |
| Rating         | Kategori usia penonton (PG-13, R+, G, dll.).                                              |
| Rank           | Peringkat berdasarkan skor (perlu dibersihkan jika dianalisis).                          |
| Popularity     | Peringkat popularitas berdasarkan jumlah anggota.                                         |
| Favorites      | Jumlah pengguna yang menandai sebagai favorit.                                            |
| Scored By      | Jumlah pengguna yang memberi skor (string, perlu dikonversi).                            |
| Members        | Jumlah total pengguna yang menambahkan anime ke daftar.                                  |
| Image URL      | Tautan ke gambar atau poster anime.                                                       |


🧩 **Pemilihan Fitur dan Pendekatan Model**
Pada proyek sistem rekomendasi ini, meskipun dataset yang tersedia memiliki banyak fitur informatif, hanya beberapa fitur utama yang akan difokuskan untuk membangun model rekomendasi yang efisien dan sederhana. Fitur-fitur yang dipilih meliputi:

- **Name**: Sebagai identitas utama dari anime.
- **Score**: Merepresentasikan penilaian atau preferensi pengguna.
- **Genres**: Menjadi indikator utama konten dan tema dari anime.
- **Type**: Untuk mengetahui jenis rilis seperti TV, Movie, OVA, dll.
- **Studios**: Mempertimbangkan kualitas dan gaya produksi yang khas dari masing-masing studio.

Pemilihan fitur ini didasarkan pada asumsi bahwa karakteristik konten (seperti genre dan studio) serta persepsi pengguna (dari skor) merupakan faktor penting dalam menentukan kemiripan antar anime.


🔍 **Pendekatan Content-Based Filtering**
Dalam pendekatan **Content-Based Filtering**, sistem akan merekomendasikan anime berdasarkan kemiripan konten. Sebagai contoh, genre dari anime yang disukai pengguna sebelumnya akan digunakan untuk menemukan anime lain dengan karakteristik serupa.


🤖 **Pendekatan K-Nearest Neighbors (KNN)**
Pada pendekatan **K-Nearest Neighbors (KNN)**, sistem merekomendasikan anime berdasarkan kedekatan dalam ruang fitur. Fitur-fitur seperti **Name,** **Genres,** **Type,** **Score,** dan **Studios** menjadi kunci utama dalam proses ini, karena mereka merepresentasikan baik konten maupun kualitas persepsi pengguna.


🧪 **Potensi Pengembangan**
Walaupun terdapat fitur tambahan seperti **sinopsis, tanggal penayangan,** atau **jumlah episode** yang juga berpotensi meningkatkan performa model, fitur-fitur tersebut belum dimanfaatkan pada tahap awal ini untuk menjaga kompleksitas tetap rendah. Namun, fitur-fitur tersebut dapat dijadikan bahan eksplorasi lebih lanjut dalam pengembangan model di masa mendatang.


Sebelum melakukan visualisasi akan diperiksa apakah dataframe mengalami duplikat & missing value, karena tidak mengalami hal tersebut bisa dilanjutkan dengan tahap visualisasi.

### Visualisasi
Berdasarkan dataframe tersebut, dapat dibuat beberapa Interpretasi dan Insight:

![Top 10 Genre Anime Terpopuler](https://drive.google.com/uc?export=view&id=1IdCauPMRfoV0u7l3EwNAKnqa33tiZD7p)


**Interpretasi & Insight:**

- **Dominasi Genre Umum:** Terlihat jelas bahwa genre Comedy menempati posisi teratas sebagai genre yang paling sering muncul, diikuti oleh Action, Fantasy, dan Adventure. Ini menunjukkan bahwa sebagian besar anime dalam dataset cenderung memiliki elemen-elemen ini, yang mencerminkan popularitas genre-genre tersebut di kalangan penonton secara luas.
- **Keberagaman Menengah:** Genre seperti Drama, Sci-Fi, dan Romance juga memiliki frekuensi yang signifikan, menunjukkan keberagaman konten yang cukup baik meskipun ada dominasi genre teratas.
- **Genre Spesifik Populer:** Kehadiran School, Shounen, dan Slice of Life dalam 10 besar menandakan adanya segmen pasar atau tipe cerita yang kuat dan sering diproduksi dalam industri anime.


![Distribusi Skor Anime](https://drive.google.com/uc?export=view&id=1hwwkbrIl_kSnlMdjqKq7fxAU-KLHtYBv)


**Interpretasi dan Insight:**

- **Distribusi skor anime berbentuk normal**, dengan mayoritas skor berada di antara 6 hingga 7, dan puncaknya sekitar 6.5. Ini menunjukkan bahwa sebagian besar anime memiliki kualitas yang tergolong baik.
- **Anime dengan skor sangat rendah (< 4) dan sangat tinggi (> 8.5)** sangat jarang ditemukan, menunjukkan konsistensi kualitas dalam industri anime.
- **Skor dapat digunakan sebagai indikator awal dalam sistem rekomendasi**, dengan menetapkan threshold (misalnya skor ≥ 6) untuk memfilter anime yang layak direkomendasikan kepada pengguna.


![Top 10 Anime dengan Skor Tertinggi](https://drive.google.com/uc?export=view&id=1ohrLzROn107BA34Qd-IEBwu_7lHPyrNi)


**Interpretasi dan Insight:**
- **Fullmetal Alchemist:** Brotherhood menempati posisi teratas dengan skor tertinggi, yang menunjukkan popularitas dan kualitas cerita serta produksi yang sangat diakui oleh komunitas anime global.
- **Seri Gintama mendominasi daftar**, muncul sebanyak tiga kali dalam berbagai versi/sekuel, menunjukkan kekuatan fanbase yang besar dan konsistensi kualitas dari seri ini.
- **Genre action dan psychological** tampak dominan dalam daftar ini (misalnya: Steins;Gate, Attack on Titan, Hunter x Hunter), memberikan insight bahwa genre tersebut sangat disukai dan cenderung mendapat skor tinggi dari penonton.


![Distribusi Anime berdasarkan Type](https://drive.google.com/uc?export=view&id=14Qb7qNbwFX02uEINVQacPtBpFkXk16q4)


**Interpretasi dan Insight:**
- **Anime berformat TV** merupakan tipe paling dominan, dengan jumlah lebih dari 7.500 judul. Ini menunjukkan bahwa serial televisi adalah format paling umum dalam industri anime, kemungkinan karena memungkinkan pengembangan cerita yang lebih panjang dan mendalam.
- **Movie, OVA, dan ONA** juga cukup banyak diproduksi, menandakan bahwa format pendek seperti OVA dan ONA tetap memiliki pasar tersendiri, mungkin untuk eksperimen kreatif atau sebagai pendamping dari serial utama.
- **Kategori UNKNOWN** sangat sedikit dan bisa dianggap noise. Untuk keperluan analisis lanjutan, data dengan label UNKNOWN sebaiknya dipertimbangkan untuk dibersihkan atau dikecualikan agar tidak mengganggu hasil insight lainnya.


![Distribusi Durasi Anime per Episode](https://drive.google.com/uc?export=view&id=1lxD0JvakvZ7JVFDaubYf9d80lLOUfcoi)


**Interpretasi dan Insight:**
- **Durasi paling umum** adalah sekitar 1–5 menit dan 24 menit per episode, dengan dua puncak distribusi yang sangat jelas. Ini menunjukkan bahwa anime didominasi oleh dua format durasi utama: anime pendek (biasanya ONA/OVA/special) dan durasi standar siaran TV (sekitar 24 menit).
- **Distribusi menunjukkan bentuk bimodal**, menandakan adanya dua kategori produksi yang berbeda secara signifikan: anime pendek untuk konten eksperimental atau humor ringan, dan anime standar untuk cerita penuh.
- **Durasi di atas 30 menit sangat jarang**, menandakan bahwa episode panjang (misalnya film berdurasi penuh yang dibagi menjadi bagian-bagian) merupakan pengecualian dan bukan format umum di industri anime.


![Distribusi Jumlah Episode per Anime](https://drive.google.com/uc?export=view&id=170iNLLKW1SGtuSt1K_ig3WAHfdsnbMAc)


**Interpretasi dan Insight:**
- **Mayoritas anime memiliki episode sangat sedikit (1–12 episode)**, yang umumnya berasal dari format pendek seperti OVA, ONA, atau mini series. Ini menunjukkan tren dominasi anime pendek di industri.
- **Jumlah anime menurun drastis seiring bertambahnya jumlah episode**, menunjukkan bahwa anime dengan episode panjang (30 ke atas) adalah minoritas dan lebih jarang diproduksi karena biaya dan durasi produksi yang lebih besar.
- **Distribusi bersifat right-skewed (condong ke kanan)**, artinya anime dengan jumlah episode tinggi seperti One Piece atau Naruto adalah outlier dan tidak mencerminkan mayoritas tren produksi anime.


![Judul Anime dirilis per Tahun](https://drive.google.com/uc?export=view&id=1khuZON9xRO3D-itvESnMVecCttKPjZ_G)


**Interpretasi dan Insight dari Grafik:**
1. **Pertumbuhan Lambat (1900–1980-an):**
- Hampir tidak ada anime dirilis secara masif sebelum tahun 1980.
- Ini wajar karena industri anime modern baru benar-benar berkembang setelah teknologi animasi membaik dan TV menjadi umum.
2. **Peningkatan Signifikan (1980–2000-an):**
- Ada kenaikan stabil sejak 1980-an. Ini seiring dengan popularitas global anime dan pertumbuhan studio seperti Ghibli, Toei, dll.
3. **Puncak Produksi (2010–2021):**
- Jumlah rilisan melonjak pesat, bahkan mencapai lebih dari 600 judul per tahun.
- Bisa dikaitkan dengan era digital, streaming platform (Crunchyroll, Netflix), dan lonjakan minat global terhadap anime.
4. **Penurunan Tajam Setelah 2021:**
- Data tahun 2022 ke atas menunjukkan penurunan drastis, kemungkinan besar data belum lengkap atau belum diupdate, bukan karena produksi menurun drastis.


![Top 10 Anime dengan Komunitas Terbanyak](https://drive.google.com/uc?export=view&id=10oNyPuzSDCsjq4bBB52XNNSC7qiSrmoG)


**Interpretasi dan Insight:**
1. **Anime Paling Populer secara Komunitas:**
- Shingeki no Kyojin (Attack on Titan) dan Death Note berada di puncak dengan lebih dari 3,7 juta members, menunjukkan betapa besarnya komunitas penggemar mereka di platform seperti MyAnimeList.
- Keduanya memiliki dampak global yang besar, bahkan menembus pasar non-anime biasa.
2. **Anime Shounen Dominasi Daftar:**
- 90% dari anime yang masuk top 10 berasal dari genre shounen atau aksi, memperlihatkan daya tarik genre ini yang sangat kuat di kalangan komunitas global.
3. **Keberadaan Anime Lama dan Baru:**
- Ada anime lama seperti Naruto dan Death Note, tapi juga ada yang relatif baru seperti Kimetsu no Yaiba dan Boku no Hero Academia, yang menunjukkan pertumbuhan komunitas tidak hanya tergantung usia anime.


## Data Cleaning
- Handling duplicate values: tidak ditemukan adanya duplicate values
- Handling missing values: ditemukan adanya missing values akibat proses visualisasi data
- Handling Drop Column : membuat kolom tambahan akibat proses visualisasi data

**Alasan:**
- Handling missing values diperiksa untuk memastikan tidak ada data kosong yang dapat mengganggu hasil model atau analisis.
- Handling duplicate values diperiksa agar tidak ada duplikasi data yang bisa membuat bobot informasi menjadi tidak proporsional.
- Handling Drop Column agar data yang digunakan original seperti diawal


## Data Preparation
- Handling text cleaning
- Handling Drop Column
- Inisialisasi TfidfVectorizer
- Melakukan perhitungan idf pada data genre
- Drop NaN pada fitur numerik dan kategorikal (`Score`, `Type`, `Studios`)
- One-hot Encoding untuk kolom `Type` dan `Studios`
- Penggabungan fitur `Score` dengan hasil encoding
- Pengecekan NaN setelah penggabungan fitur
- Scaling fitur numerik

 
**Alasan:**
- Handling text cleaning untuk membersihkan teks dari tanda baca dan URL
- Handling Drop Column karna data yang akan di hapus tidak mempengaruhi model
- Inisialisasi TfidVectorizer dilakukan untuk mengubah data teks (dalam hal ini genre anime) menjadi representasi numerik dalam bentuk vektor. Dengan menggunakan TF-IDF, setiap genre diberi bobot berdasarkan frekuensi kemunculannya secara lokal (dalam satu anime) dan global (di seluruh dataset), sehingga genre yang lebih informatif akan memiliki pengaruh lebih besar dalam analisis dan pemodelan seperti sistem rekomendasi.
- Melakukan perhitungan idf pada data genre dilakukan untuk menghitung nilai Inverse Document Frequency (IDF) dari masing-masing genre. Tujuannya adalah untuk memberi bobot lebih besar pada genre yang lebih unik dan tidak terlalu sering muncul, serta menurunkan bobot genre yang terlalu umum.
- Drop NaN pada fitur numerik dan kategorikal penting (`Score`, `Type`, `Studios`) untuk memastikan data bersih sebelum transformasi.
- One-hot Encoding untuk kolom `Type` dan `Studios`, agar fitur kategorikal dapat diubah menjadi bentuk numerik menggunakan representasi biner.
- Penggabungan fitur `Score` dengan hasil encoding, agar seluruh fitur berada dalam satu DataFrame numerik yang siap untuk proses standarisasi atau pemodelan.
- Pengecekan akhir untuk memastikan tidak ada nilai kosong (NaN) dalam DataFrame `features` sebelum masuk tahap standarisasi atau modelling.
- Standarisasi semua fitur menggunakan `StandardScaler` untuk memastikan semua fitur berada dalam skala yang seragam sebelum digunakan dalam algoritma seperti KNN atau clustering.


## Model Development
Dalam proyek ini, digunakan algoritma Cosine Similarity dan K-Nearest Neighbor untuk menganalisis kemiripan antar data. Kedua metode ini bekerja dengan mempelajari seberapa mirip setiap entri berdasarkan fitur-fitur yang telah diproses.

### Cosine similarity
### 🔍 Cosine Similarity

**Cosine Similarity** adalah metode yang digunakan untuk mengukur tingkat kemiripan antara dua vektor dalam ruang berdimensi banyak. Alih-alih menghitung jarak absolut antar titik, metode ini mengukur sudut (kosinus) di antara dua vektor untuk menentukan seberapa mirip arah mereka. [[6](https://medium.com/geekculture/cosine-similarity-and-cosine-distance-48eed889a5c4)]

Metode ini sangat berguna dalam berbagai aplikasi seperti **pemrosesan teks**, **sistem rekomendasi**, dan **pengelompokan data**, karena mampu mendeteksi kesamaan meskipun data memiliki skala atau magnitudo yang berbeda.

Nilai Cosine Similarity berada dalam rentang antara **-1 hingga 1**:
- `1` → Kedua vektor sejajar dan mengarah sama (sangat mirip).
- `0` → Kedua vektor tegak lurus (tidak berkaitan).
- `-1` → Kedua vektor berlawanan arah (sangat tidak mirip).

Rumus Cosine Similarity dituliskan sebagai:

$$
\text{Cosine Similarity}(A, B) = \frac{A \cdot B}{\|A\| \times \|B\|}
$$

**Keterangan:**
- `A · B` → Menyatakan **dot product** antara vektor A dan B, yaitu hasil penjumlahan dari perkalian elemen-elemen yang bersesuaian pada kedua vektor.
- `||A||` → Menyatakan **norma Euclidean** (atau panjang/magnitudo) dari vektor A. Dihitung sebagai akar kuadrat dari jumlah kuadrat setiap komponennya.
- `||B||` → Menyatakan **norma Euclidean** dari vektor B.

Dengan pendekatan ini, kita dapat menilai kesamaan antar data berdasarkan orientasi fitur mereka, bukan berdasarkan skala nilainya. Inilah mengapa Cosine Similarity sangat cocok untuk membandingkan item dalam dataset seperti genre anime, ulasan pengguna, atau konten dokumen.


Untuk melakukan pengujian model, digunakan potongan kode berikut.
```python
nel[nel.Name.eq('Haikyuu')]
```
| Name     | Score | Genres  | Type | Studios        |
|----------|-------|---------|------|----------------|
| Haikyuu  | 8.44  | Sports  | TV   | Production I.G |


```python
rekomendasi_anime('Haikyuu')
```
| Name                                          | Genres  |
|-----------------------------------------------|---------|
| Ashita e Free Kick                            | Sports  |
| Diamond no Ace Act II                         | Sports  |
| Gekitou Crush Gear Turbo                      | Sports  |
| Tennis no Oujisama Sonzoku Yama no Hi        | Sports  |
| Baki Most Evil Death Row Convicts Special Anime | Sports  |

Table 1. Hasil Pengujian Model Content Based Filtering (dengan Filter Genres).

Berdasarkan *Tabel 1. Hasil Pengujian Model Content Based Filtering (menggunakan Filter Genres)*, sistem berhasil merekomendasikan anime dalam 5 persen teratas yang memiliki kemiripan dengan **Haikyuu**. Artinya, jika pengguna menyukai anime **Haikyuu**, sistem dapat menyarankan seri atau film lain yang terkait dalam waralaba tersebut. Pendekatan ini menggunakan kemiripan genre sebagai dasar proses rekomendasi, sehingga memungkinkan pengguna menemukan anime yang relevan dan sesuai dengan preferensi mereka berdasarkan minat awal pada **Haikyuu**.

### Keunggulan *Cosine Similarity*:
- Memiliki kompleksitas komputasi yang rendah, sehingga proses perhitungan menjadi cepat dan efisien.
- Sangat cocok diterapkan pada dataset berdimensi tinggi karena fokus pada arah vektor, bukan nilai absolut, sehingga skala data tidak memengaruhi hasil.

### Keterbatasan *Cosine Similarity*:
- Hanya mempertimbangkan sudut atau arah antar vektor tanpa memperhitungkan besar (magnitudo) vektor tersebut.
- Perbedaan nilai absolut yang signifikan bisa terabaikan, sehingga dua vektor dengan besaran yang sangat berbeda tetap bisa dianggap mirip jika arahnya sama.

### K-Nearest Neighbor
**K-Nearest Neighbor (KNN)** merupakan salah satu algoritma klasifikasi yang paling mudah dipahami dan sederhana. Algoritma ini mengelompokkan data dengan cara melihat kedekatan jarak antara sebuah data baru dengan data-data yang sudah ada. Pada KNN, kita menentukan sejumlah tetangga terdekat berdasarkan nilai parameter K, lalu kelas atau label data baru ditentukan berdasarkan mayoritas kelas dari tetangga-tetangga tersebut. Jika nilai K = 1, maka kelas data baru hanya ditentukan oleh satu tetangga terdekat yang paling dekat secara karakteristik.[[7](https://medium.com/bee-solution-partners/cara-kerja-algoritma-k-nearest-neighbor-k-nn-389297de543e)]

Rumus yang digunakan dalam KNN untuk mengukur jarak antar titik data adalah:

$$
\text{Euclidean Distance} (P, Q) = \sqrt{\sum (P_i - Q_i)^2}
$$

dimana:
- \(P_i\) adalah fitur ke-i dari titik data \(P\).
- \(Q_i\) adalah fitur ke-i dari titik data \(Q\) (data dalam kumpulan data \(D\)).
- \(\sum\) adalah simbol penjumlahan yang meliputi semua fitur dari data tersebut.

Untuk melakukan pengujian model, digunakan potongan kode berikut.
```python
recommend_anime('Death Note', n=5)
```
| Name                     | Score | Type | Studios  | Similarity  |
|--------------------------|-------|------|----------|-------------|
| Hajime no Ippo New Challenger | 8.66  | TV   | Madhouse | 0.999986    |
| Yojouhan Shinwa Taikei       | 8.57  | TV   | Madhouse | 0.999977    |
| Nana                        | 8.54  | TV   | Madhouse | 0.999942    |
| Sora yori mo Tooi Basho      | 8.52  | TV   | Madhouse | 0.999909    |
| One Punch Man               | 8.50  | TV   | Madhouse | 0.999869    |

Tabel 2. Hasil Pengujian Model K-Nearest Neighbor

Berdasarkan *Tabel 2. Hasil Pengujian Model K-Nearest Neighbor*, terlihat bahwa model K-Nearest Neighbor memberikan rekomendasi anime dengan mempertimbangkan kemiripan fitur seperti 'Name', 'Score', 'Type', dan 'Studios'. Model ini merekomendasikan anime yang memiliki karakteristik serupa dengan **Death Note**, yaitu *Hajime no Ippo New Challenger*, *Yojouhan Shinwa Taikei*, *Nana*, *Sora yori mo Tooi Basho*, dan *One Punch Man*. Seperti terlihat pada *Tabel 2*, tingkat kemiripan antar anime tersebut mencapai sekitar 99%. Dengan demikian, model ini sangat membantu pengguna dalam menemukan anime yang relevan dan mirip dengan **Death Note**.

### Kelebihan K-Nearest Neighbor:
- Proses pelatihan yang cepat.
- Algoritma yang sederhana dan mudah dipahami.
- Tahan terhadap data pelatihan yang mengandung noise (derau).
- Efektif digunakan pada dataset yang besar.

### Kekurangan K-Nearest Neighbor:
- Pemilihan nilai *k* dapat memengaruhi bias pada model.
- Proses komputasi menjadi berat terutama pada data yang besar atau memiliki dimensi fitur tinggi.
- Membutuhkan memori besar karena harus menyimpan seluruh data pelatihan.
- Rentan terhadap pengaruh fitur yang tidak relevan, yang dapat menurunkan akurasi klasifikasi.


## Evaluation
**Metrik evaluasi** berfungsi untuk mengukur seberapa efektif sebuah model dalam menjalankan tugasnya. Dalam konteks ini, berbagai metrik evaluasi umum dipakai untuk menilai performa model, salah satunya adalah Precision. Metrik-metrik tersebut memberikan gambaran mengenai kualitas kerja model dalam melakukan klasifikasi, klastering, atau tugas lainnya.

### Precision
_Precision_ merupakan metrik penting dalam menilai performa model klasifikasi, khususnya dalam mengukur ketepatan model dalam mengidentifikasi data positif. Nilai precision yang tinggi menunjukkan bahwa model jarang memberikan prediksi positif yang keliru, sehingga hasil prediksi positif tersebut dapat lebih diandalkan.[[7](https://esairina.medium.com/memahami-confusion-matrix-accuracy-precision-recall-specificity-dan-f1-score-610d4f0db7cf)]

Rumus untuk menghitung _Precision_ adalah sebagai berikut:

$$
Precision = \frac{TP}{TP + FP}
$$

Dimana:
- **TP (True Positive)**: jumlah data yang diprediksi positif dan memang benar positif.
- **FP (False Positive)**: jumlah data yang diprediksi positif tetapi sebenarnya negatif.

*Interpretasi* dari hasil presisi berdasarkan *Tabel 1. Hasil Pengujian Model Content Based Filtering (dengan Filter Genres)* menunjukkan bahwa model rekomendasi Top-5 memiliki presisi sempurna, yaitu 5 dari 5 atau 100%. Hal ini mengindikasikan bahwa model tersebut mampu menghasilkan rekomendasi dengan tingkat ketepatan yang sangat tinggi. Hasil pengujian memperlihatkan bahwa model berhasil merekomendasikan anime dengan nama dan genre yang sangat mirip dengan **Haikyuu**, terutama dalam genre Sports. Kelima rekomendasi yang ditampilkan semuanya memiliki genre yang sesuai dengan **Haikyuu**.


## Referensi
1. Ricci, F., Rokach, L., & Shapira, B. (2011). *Introduction to Recommender Systems Handbook*. Springer.  
   Diakses dari: [https://link.springer.com/book/10.1007/978-0-387-85820-3](https://link.springer.com/book/10.1007/978-0-387-85820-3)
2. Gómez-Uribe, C. A., & Hunt, N. (2016). *The Netflix Recommender System: Algorithms, Business Value, and Innovation*. ACM Transactions on Management Information Systems, 6(4), 1–19.  
   Diakses dari: [https://dl.acm.org/doi/10.1145/2843948](https://dl.acm.org/doi/10.1145/2843948)
3. Aggarwal, C. C. (2016). *Recommender Systems: The Textbook*. Springer.  
   Diakses dari: [https://link.springer.com/book/10.1007/978-3-319-29659-3](https://link.springer.com/book/10.1007/978-3-319-29659-3)
4. Lu, J., Wu, D., Mao, M., Wang, W., & Zhang, G. (2015). *Recommender System Application Developments: A Survey*. Decision Support Systems, 74, 12–32.  
   Diakses dari: [https://doi.org/10.1016/j.dss.2015.03.008](https://doi.org/10.1016/j.dss.2015.03.008)
5. Jannach, D., Adomavicius, G., & Tuzhilin, A. (2021). *Recommender Systems: Challenges, Insights and Research Opportunities*. ACM Transactions on Intelligent Systems and Technology (TIST), 10(4), 1–42.  
   Diakses dari: [https://dl.acm.org/doi/10.1145/3298689](https://dl.acm.org/doi/10.1145/3298689)
6. Geek Culture. (2021). *Cosine Similarity and Cosine Distance*.  
   Diakses dari: [https://medium.com/geekculture/cosine-similarity-and-cosine-distance-48eed889a5c4](https://medium.com/geekculture/cosine-similarity-and-cosine-distance-48eed889a5c4)
7. Esairina. (2021). *Memahami Confusion Matrix, Accuracy, Precision, Recall, Specificity, dan F1-Score*.  
   Diakses dari: [https://esairina.medium.com/memahami-confusion-matrix-accuracy-precision-recall-specificity-dan-f1-score-610d4f0db7cf](https://esairina.medium.com/memahami-confusion-matrix-accuracy-precision-recall-specificity-dan-f1-score-610d4f0db7cf)




