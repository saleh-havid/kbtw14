*Muhamad Bashori Alwi
NIM : 23213117*
# Datastore Indexes


## Pengertian Umum

Semua *query* pada *datastore* dilakukan dengan menggunakan indeks,dan mengetahui bagaimana indeks bekerja adalah penting ketika kita merancang dan membangun aplikasi, terutama jika bekerja dengan sistem manajemen database relasional, dan SQL. Perlu disadari bahwa indeks tidak sama dengan menyimpan data indeks, indeks dalam RDBMS adalah optimasi *query* yang digunakan untuk mengambil hasil lebih cepat. Sementara indeks di *datastore*, pada kenyataannya diperlukan untuk melakukan query pada seluruh data sekaligus.

Jadi jika kita ingin *query* untuk data pada *datastore*, harus ada indeks untuk data itu. Ketika menyimpan entitas, *datastore* juga akan melaksanakan update tabel indeks, kemudian digunakan untuk mencari entitas saat *query* dibuat. Tabel indeks ini terdiri dari nilai indeks, yang memetakan sebuah entitas dari *key* yang sesuai. Secara umum dapat dijelaskan bahwa  nilai indeks adalah kombinasi dari jenis, properti, dan nilai properti. Setiap nilai index menunjuk ke kunci entitas di mana nilai tersebut berada.

Sebagai contoh, apabila kota favorit adalah Paris dan kita ingin mendapatkan semua konferensi yang berada di Paris. Kemudian* datastore* akan melihat tabel indeks di mana kota adalah Paris, dan menemukan semua pencocokan entitas. Karena menyimpan data memerlukan tabel indeks
untuk melakukan *query*, setiap properti yang kita *query* akan memerlukan sebuah tabel indeks. Secara default, *datastore* akan bekerja secara otomatis. Tapi juga akan dibahas lebih lanjut bagaimana kita dapat mengontrol hal tersebut.

![gambar datastore indeks](https://lh6.googleusercontent.com/cqFaKUZyBt1Ehdvm6Pjcw4ouY6aiNHuU1Pg1-8j6=w480-h191-p-no)

## Size of Index Tables
Jika sebelumnya kita sudah membahas mengenai *datastore index*, pembahasan berikutnya dalah tentang ukuran tabel indeks tersebut, karena ini penting ketika Anda membangun aplikasi *datastore*. Menggunakan konsol pengembang, anda dapat melihat ukuran indeks di bawah
*datastore* halaman *dashboard*. Di sini, sebagai contoh dashboard untuk aplikasi yang sangat sederhana. Dapat kita lihat, entitas sendiri yang mempunyai kapasitas 6,75 kilobyte. Namun, indeks *built-in* untuk entitas tersebut, mempunyai kapasitas 57,12 kilobyte. Hal tersebut sebenarnya adalah skenario umum.

Indeks sangat sering mengambil lebih banyak ruang daripada penyimpanan yang diperlukan hanya untuk menyimpan entitas. Indeks *built-in* adalah indeks yang dihasilkan untuk masing-masing individu properti, seperti yang kita bicarakan untuk konferensi dengan properti kota. Ada juga sesuatu yang disebut indeks komposit, tapi hal tersebut akan dibahas dalam pembahasan berikutnya. Seperti yang kita lihat, jika kita membiarkan menyimpan data dengan mempertahankan indeks untuk setiap individu properti, hal tersebut dapat menambah kapasitas memory yang ada. Oleh karena itu, penting untuk berpikir tentang properti yang kita butuhkan pada *query*.

![gambar index size](https://lh6.googleusercontent.com/-Shc_rz3LGr0/VI7AJDGCGWI/AAAAAAAAAHs/KvcV-6tTc0A/w509-h107-no/size%2Bindexes.JPG)

## Single Property Indexes in Objectivity
Dalam *Java Datastore API*, semua properti akan diindeks secara default. Jika kita tidak hati-hati dalam hal tersebut,maka akan mengakibatkan *index bloat*, yakni penggunaan data indeks secara berlebihan.Untuk menghindari *index bloat*, maka digunakan *objectify* yang akan membuat semua nilai tidak akan di indeks secara default.

Beberapa hal yang bisa dilakukan untuk menghindari *index bloat* dengan menggunakan *objectify* antara lain :

1. Jika kita ingin property yang diindeks, masukkan annotasi *@indeks* pada property.
2. Jika kita ingin secara default, masukkan annotasi *@unindex* pada masing-masing property yang tidak ingin diindeks.
3. Jika kita ingin sesuai kriteria tertentu, maka dapat digunakan conditional indexing sesuai kriteria yang kita inginkan.

## Composite Indexes
Jika sebelumnya kita bicara masalah indeks pada single property, sebagai contoh annotasi indeks pada *value*  kota paris. Hal tersebut merupakan indeks properti tunggal yang juga disebut dengan *built in index*. Dalam beberapa kasus, *datastore* mampu menggabungkan beberapa *built in index* untuk mengatasi permasalahan *query* indexes pada datastore tersebut. Namun jika hal tersebut tidak dapat dilaksanakan  maka kita menggunakan *composite indexes*. Bahkan kita telah menggunakan indeks komposit dalam *central conference* untuk *query* ini. Dengan mengambil data pada semua konferensi, berdasarkan filter pada kota dan topik dan mengurutkan berdasarkan nama. Ini merupakan indeks komposit, karena dibangun di indeks untuk kota, topik, dan nama serta  tidak bisa digabungkan untuk menghasilkan hasil. supaya hal ini dapat bekerja, tabel indeks yang terdiri dari beberapa nilai dan kombinasi harus diciptakan.

![](https://lh5.googleusercontent.com/Zfbny6ZubYLQ-saqCaQ5gLSTxECqFXrzAvonWt2f=w309-h191-p-no)

**Terdapat dua cara dalam membangun composite index  :**

1. Dengan menambahkan indeks ke dalam indeks file
2. Menjalankan aplikasi secara lokal pada *server development.*

**Cara membuat composite index :**

1. Buka *datastore-indexes-auto.xml*
2. Jalankan *query* pada *local host*
3. Cek *datastore-indexes-auto.xml*
4. jalankan pada *appspot*
5. Cek *Index viewer*
6. Jika indeks sudah ready, jalankan *query.*

![](https://lh3.googleusercontent.com/uvizW3Jg9qWMEMuD6nZu5en8W-rbwxIKzo_7Scsg=w398-h191-p-no)

**Cara memastikan bahwa query sudah bekerja pada appspot:**

1. Edit index config file secara manual
2. Jalankan query pada *local host* terlebih dahulu
3. Berikan annotasi pada property sebagai *@Index*

## Query Restrictions
Pada pembahasan sebelumnya, kita telah menggambarkan bagaimana menyimpan data menggunakan indeks untuk mengeksekusi *query*. pada pembahasan ini akan dijelaskan beberapa pembatasan permintaan yang ada. Seperti yang kita tahu, *datastore* dirancang untuk pengukuran secara menyeluruh. Oleh karena itu kinerja query hanya harus bergantung pada ukuran hasil set, dengan kata lain bergantung pada jumlah data yang dikembalikan dalam query dan tidak pada jumlah keseluruhan data yang disimpan dalam penyimpanan data.

Untuk mencapai tujuan ini, menyimpan data selalu menggunakan indeks untuk menemukan data yang cocok. Dan perlu diperhatikan adanya dua batasan *query* ketika
Anda merancang aplikasi Anda, yakni :

1. Filter yang tidak sama dapat diterapkan pada paling banyak satu properti dalam *query*.
2. Properties dengan filter yang sama  harus diurutkan terlebih dahulu.

Cara terbaik untuk menghindari ini adalah merencanakan  *query* yang kita butuhkan selama tahap desain. Dengan begitu, kita bisa desain sesuai yang kita butuhkan.
