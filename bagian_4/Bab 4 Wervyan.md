#*Lesson 4 - Advanced Datastore Concepts*
-----
**Tugas Mata Kuliah EL6240 - Komputasi Bergerak dan Teknologi Web**
*oleh Wervyan Shalannanda - 23213145*

##***Consistency Models and Transactions in Datastore***

###***Datastore Commit Process***
*Datastore commit process* mendeskripsikan tentang konsistensi aturan-aturan untuk menyimpan data. *Datastore* memiliki dua jenis konsistensi model, yaitu:

 1. *Eventual Consistency*; dan
 2. *Strong Consistency*.

Perhatikan diagram *use case* berikut.
![](https://lh5.googleusercontent.com/-6SFFE__bdgE/VI8EbX6wdhI/AAAAAAAAAMM/4I5Jte_uhR4/s0/1+-+Datastore+Commit+Process.jpg "1 - Datastore Commit Process.jpg")
*Gambar 1. Entitas pada datastore commit process*

Dari diagram *use case* di atas, dapat terlihat tiga elemen, yaitu: aplikasi, *datastore API*, dan *datastore backend*. Ketika akan menyimpan data/menuliskan *entity* ke dalam *storage*, aplikasi akan melakukan suatu operasi kepada *datastore API* kemudian *datastore backend* menuliskan data tersebut ke suatu *log*. Perbedaan antara *eventual consistency* dan *strong consistency* terdapat pada prosedur *datastore backend* menuliskan data yang disimpan oleh aplikasi ke dalam *datastore*. Ilustrasi kedua konsistensi ini dapat dilihat pada bagan berikut.

![](https://lh3.googleusercontent.com/-yU4rjFGpY3I/VI8KbCXX3PI/AAAAAAAAANQ/sEzsHKo3DYM/s0/2+-+Eventual+Consistency.jpg "2 - Eventual Consistency.jpg")
*Gambar 2. Eventual Consistency*
![^gambar3](https://lh4.googleusercontent.com/-8e9g-jH6epQ/VI8IJ_0NDRI/AAAAAAAAAM4/Hy_D9QA4dSE/s0/3+-+Strong+Consistency.jpg "3 - Strong Consistency.jpg")
*Gambar 3. Strong Consistency*

Berdasarkan gambar 2, dapat terlihat bahwa setelah *datastore API* telah selesai menuliskan *log*, kontrol langsung dikembalikan ke aplikasi. Dengan demikian, *eventual consistency* memiliki keunggulan berupa *latency* yang rendah. Padahal, *entity* dari aplikasi belum selesai dituliskan oleh *datastore backend* ke dalam penyimpanan (data belum selesai disimpan). Pada tahap ini, *datastore backend* tengah bekerja untuk membuat semuanya konsisten dengan menggunakan informasi *login* untuk memperbarui *entity storage* serta memperbarui semua indeksnya. 

Perbedaan yang jelas antara kedua konsistensi tersebut dapat terlihat ketika aplikasi me-*request* *query* kepada *datastore API*. Apabila ada *query* sebelum *datastore backend* selesai melakukan pembaruan indeks, maka *datastore backend* akan mengembalikan *query* seperti sebelum aplikasi melakukan operasi pada *datastore API*.Sedangkan untuk *strong consistency*, ketika *datastore API* menerima *query*, *datastore API* akan melihat bahwa terjadi *pending updates* atas data yang di-*query*. *Query* akan dibiarkan menunggu hingga *datastore backend* telah selesai melakukan operasinya, yaitu menulis entitas dan memperbarui indeks. Dengan demikian, akan diperoleh *query* terbaru.

>Manakah *consistency* yang lebih baik?

Perhatikan dua kasus berikut.

 1. Komentar Blog
- tidak membutuhkan *immediate update*
- tidak ada *operation dependency* - tidak ada operasi lanjutan yang mengharuskan data *up to date*
--> lebih baik menggunakan *eventual consistency*
 2. Pengambilan uang di ATM
- saldo akhir harus langsung diperbarui
- terdapat *operation dependency* - misalnya ada pembayaran tagihan atas akun tersebut yang membutuhkan saldo terkini.
--> harus menggunakan *strong consistency*


>Apakah harus selalu menggunakan *strong consistency?*

Tidak. *Strong consistency* membuat sistem banyak menunggu dan mengakibatkan aplikasi memiliki *latency* cukup lama. Untuk membangun aplikasi yang *scalable*, sebaiknya *programmer* menggunakan *eventual consistency* sesering mungkin.

Datastore mendukung kedua model ini. *Eventual consistency* yang *scalable*dapat digunakan sebagai model *default*. Kemudian, apabila dibutuhkan, *strong consistency* dapat digunakan untuk memastikan integritas data pada sistem.

>Bagaimana cara memilih antara kedua model tersebut?

*Strong consistency* digunakan ketika terdapat dua kondisi, yaitu: terdapat *ancestor relationship*, dan *query* menggunakan *filter* yang berasal dari *ancestor* ini. Selain itu, dapat digunakan *eventual consistency*.


###***Transactions***
Sebelum membahas tentang transaksi, sebaiknya kita kembali sejenak ke bagian *data relationship*
![enter image description here](https://lh6.googleusercontent.com/-i48xLoC2q4c/VI8YiYsVvfI/AAAAAAAAANo/9zH8uHEPLXI/s0/4+-+Data+Relationship.jpg "4 - Data Relationship.jpg")
*Gambar 4 - Data Relationships*

Pada bagian sebelumnya, telah dijelaskan tentang *ancestor relationship* antara suatu profil dengan konferensi, yaitu profil yang membuat konferensi adalah *ancestor* atas konferensi tersebut. Pada bagian ini, terdapat *has-a relationship* antara profil dengan konferensi, di mana setiap entitas profil memiliki daftar konferensi yang akan dihadirinya.Properti ini disebut dengan "*conferences to attend*". Sebagai tambahan, diberikan penghitung numerik untuk setiap entitas konferensi yang menyatakan jumlah kursi yang tersedia. Properti ini disebut "*seats available*" yang nilainya akan berkurang apabila seorang pengguna mendaftar untuk menghadiri konferensi. Berdasarkan uraian tersebut, terdapat dua operasi pembaruan atas dua entitas berbeda yang saling dependen, yaitu: nama konferensi tersebut akan ditambahkan ke "*conferences to attend*"; dan jumlah kursi yang tersedia pada "*seats available*" akan berkurang satu. 
Sampai pada bagian ini, setiap operasi pembaruan *datastore* yang digunakan masih independen satu sama lain (lihat Gambar 5). Selanjutnya, akan dilakukan operasi yang saling dependen pada properti "*conferences to attend*" dan "*seats available*" (lihat Gambar 6).

![enter image description here](https://lh6.googleusercontent.com/-tmhgEw-WPdY/VI8gDzufPFI/AAAAAAAAAOA/yfu4AprbBws/s0/5+-+Transaction+1.jpg "5 - Transaction 1.jpg")
*Gambar 5. Operasi yang saling independen*

![enter image description here](https://lh4.googleusercontent.com/-00H_Pz4314Y/VI8jdAIUODI/AAAAAAAAAOY/JY1DOQ60nXs/s0/6+-+Transaction+2.jpg "6 - Transaction 2.jpg")
*Gambar 6. Operasi yang saling dependen*

Diperlukan metode khusus untuk menghindari hal yang tidak diinginkan ketika aplikasi melakukan operasi. Salah satu kasus yang mungkin terjadi adalah aplikasi berhasil melakukan pembaruan pada properti *add conference to attend* namun karena suatu hal, sistem mengalami *failure* sebelum nilai *seats available* dikurangi (lihat Gambar 7). Sistem mengalami masalah pada konsistensi data, yaitu salah satu profil berhasil mendaftar konferensi, namun jumlah kursi tersisa tidak dikurangi sehingga dapat menyebabkan jumlah pendaftar melebihi kursi yang tersedia.

![enter image description here](https://lh4.googleusercontent.com/-gB6odURtrY0/VI8lys_6QfI/AAAAAAAAAOw/O3KgcMMk4QY/s0/7+-+Transaction+3.jpg "7 - Transaction 3.jpg")
*Gambar 7. Salah satu contoh kegagalan sistem*

Kesalahan tersebut dapat diantisipasi dengan menggunakan*transactions*, yaitu dengan memberitahu *data store* untuk memulai transaksi, kemudian dilakukan operasi-operasi yang saling dependen dan akhiri transaksi setelah semua operasi selesai (lihat Gambar 8). Apabila transaksi berhasil, maka semua pembaruan juga telah selesai. Namun, apabila transaksi gagal, maka semua pembaruan akan dihapus seolah-olah tidak terjadi pembaruan sama sekali (lihat Gambar 9). Di dalam bahasa *transactions*, metode ini dikenal dengan nama *commit forward*.  Dengan demikian, dapat dinyatakan bahwa semua operasi di dalam *transactions* akan dieksekusi sebagai sebuah grup yang lengkap atau tidak sama sekali.

![enter image description here](https://lh4.googleusercontent.com/-zlC3EPhWFVw/VI8orV0DmRI/AAAAAAAAAPI/TeqNAx6Ep-A/s0/8+-+Transaction+4.jpg "8 - Transaction 4.jpg")
*Gambar 8. Commit Forward Transactions ketika semua operasi berhasil*

![enter image description here](https://lh4.googleusercontent.com/-LBrBiW4bu-c/VI8pNFq8OfI/AAAAAAAAAPc/cq1UtUcU_ig/s0/9+-+Transaction+5.jpg "9 - Transaction 5.jpg")
*Gambar 9. Commit Forward Transactions ketika sebagian operasi gagal*

Berikut beberapa *transaction rules*

 1. Snapshot Isolation: konsistensi pembacaan data
- semua operasi pembacaan pada transaksi akan mengembalikan nilai yang dimiliki oleh *datastore* ketika transaksi dimulai.
- pembaruan tidak akan ditampilkan selama transaksi, sehingga pembaruan hanya akan terjadi apabila seluruh operasi dalam transaksi berhasil atau pembaruan tidak terjadi sama sekali apabila ada operasi yang gagal sehingga membatalkan transaksi.
 2. Optimistic Concurrency: mengatur pembaruan *concurrent* (terjadi bersamaan)
- transaksi akan sukses jika nilai yang diperbarui oleh transaksi ini tidak berubah sejak awal transaksi. Jika ada nilai yang berubah sejak dimulainya transaksi, maka transaksi akan dibatalkan.

Selain aturan di atas, aturan di atas, sistem dapat mengimplementasikan aturan lain, seperti:
- Satu transaksi hanya dapat mengubah maksimum 5 grup *ancestor* (atau grup entitas);
- Satu transaksi harus selesai dalam 60 detik;
- Buat transaksi sesingkat mungkin, dll.
