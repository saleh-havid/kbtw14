#*Cron Jobs*
Mutsla Adlan (23213143)
## Apa Itu *Cron Jobs*?

*Cron* adalah sebuah fitur penjadwalan dalam *AppEngine* yang memungkinkan pengguna untuk dapat mengeksekusi kode aplikasi yang diinginkan secara rutin dan berkala.

*Cron* sangat sesuai digunakan untuk melakukan pekerjaan-pekerjaan di balik layar *(background task)*. Pekerjaan dibalik layar yang dimaksud adalah pekerjaan yang tidak langsung diinisiasi oleh aktivitas pengguna aplikasi. Sebagai contoh *batch*, *synchronization*, dan *data back up*.

*Cron* sangat mudah untuk dikonfigurasi. Ada 2 (dua) parameter yang harus dinyatakan pada kode cron yang akan dijalankan. Parameter tersebut antara lain :

1. **URL** : alamat kode yang diinginkan untuk dieksekusi dengan *cron*.
2. **Penjadwalan** : jadwal waktu kapan saja URL tersebut ingin dieksekusi.

Dalam menggunakan *cron* ada batasan 10 menit untuk melakukan *dinamic instances*, jika lebih dari batasan itu maka lebih baik menggunakan *residence instances*.

##Konfigurasi *Cron Jobs*

Untuk menambahkan fitur *cron* pada aplikasi yang diinginkan maka kita harus malakukan modifikasi kode *cron* dalam file .xml aplikasi kita. Contoh konfiguras kode cron.xml yang ditambahkan adalah sebagai berikut :
```
<?xml version="1.0" encoding="UTF-8"?>
<cronentries>
<cron>
<url>URL</url>
<description>Keterangan/Penjelasan</description>
<schedule>Penjadwalan</schedule>
</cron>
</cronentries>
```
Sebagai pengguna *AppEngine* dan ingin menggunakan fitur *cron* pada aplikasi kita maka kita tinggal modifikasi file cron.xml diatas. Isi elemen URL dengan alamat URL kode aplikasi yang ingin dieksekusi, isi elemen description dengan keterangan dari *cron* yang diinginkan, dan isi penjadwalan dengan waktu kapan saja kode aplikasi tersebut ingin dieksekusi.

Khusus untuk parameter penjadwalan maka format syntax yang digunakan harus sesuai dengan format dari *develop documentation AppEngine*. Berikut beberapa contoh format penjadwalan yang bisa digunakan :
```
every 12 hours
every 5 minutes from 10:00 to 14:00
2nd,third mon,wed,thu of march 17:00
every monday 09:00
1st monday of sep,oct,nov 17:00
every day 00:00
```
Hati-hati dengan penggunaan format *syntax* penjadwalan. Format harus benar-benar sesuai dengan contoh-contoh diatas. Sebagai contoh jika kita ingin membuat penjadwalan setiap 1 jam sekali maka *syntax* yang digunakan adalah **every 1 hours** bukan **every 1 hour**. Jika tidak sesuai format maka penjadwalan cron tidak akan terbaca oleh server *Google AppEngine* aplikasi kita.

Setelah semua parameter file cron.xml semua terisi sesuai dengan keinginan kita maka file tersebut bisa langsung kita unggah ke akun AppSpot kita. Sehingga kita tidak perlu lagi kesulitan untuk menjalankan aplikasi-aplikasi yang harus dijalankan secara rutin karena cron pada AppEngine kita sudah menjalankannya secara otomatis untuk kita.

##Hal-Hal yang Perlu Diperhatikan
Dalam menggunakan fitur *cron jobs* ini ada beberapa hal yang perlu diperhatikan agar sebagai pengguna *AppEngine* kita tidak dirugikan.

Pertama yang perlu diperhatikan adalah *cron* tidak bisa berjalan dalam *localhost* kita, hal ini dikarekanan dev.server pada *localhost* tidak memfasilitasi fitur *cron*. Oleh sebab itu file cron.xml tersebut sebaiknya langsung kita unggah ke akun AppSpot kita.

Kedua yang perlu diperhatikan adalah ketika file cron.xml sudah berhasil diunggah dan berhasil dieksekusi maka aplikasi *AppEngine* kita tidak akan berada dalam keadaan *Idle*, hal ini karena cron selalu berjalan pada *AppEngine* kita. Hal ini bisa kita perhatikan pada *Console API* akun *AppEngine* kita. Setiap *cron* berjalan maka jumlah *Datastore Query Ops* dan *Datastore API Calls* akan bertambah. Oleh sebab itu perlu diperhatikan juga frekuensi dari *cron* yang kita unggah. Hal ini tidak akan menjadi masalah jika *cron* hanya mengakses memcache, tetapi jika cron mengakses datastore setiap dieksekusi maka akan memakan kuota harian *AppEngine* kita.

Ketiga yang perlu diperhatikan adalah ketika ingin menghapus fitur *cron* dari aplikasi AppEngine kita. Jika kita ingin menghapus *cron* dari aplikasi kita, kita tidak bisa hanya menghapus begitu saja kode *cron* tersebut seluruhnya. Maka lebih baik kita menunggah file cron.xml baru dengan mengosongkan elemen *cron*-nya seperti berikut :
```
<?xml version="1.0" encoding="UTF-8"?>
<cronentries>

</cronentries>
```
Dengan demikian maka *cron jobs* sebelumnya tidak akan aktif lagi. Atau ada cara lain untuk menghentikan *cron jobs* kita, yaitu dengan mengganti penjadwalan dengan jangka waktu yang sangat lama seperti 1 tahun sekali.

Demikian hal-hal yang perlu diperhatikan dari penggunaan *cron jobs* pada aplikasi *AppEngine* kita. Semoga kita tidak mengalami kerugian dari hal-hal yang berjalan tanpa kita sadari.

##Langkah-Langkah Praktek Langsung Menggunakan *Cron Jobs*

Praktek langsung menggunakan *cron jobs* dilakukan berdasarkan referensi dari materi yang didapatkan di Udacity (https://www.udacity.com/course/ud859).

Pada praktek langsung ini kita akan menggunakan fitur *cron* untuk mengatur agar mengirimkan pengumuman pada setiap pengguna yang mengakses konferensi yang sudah hampir mencapai batas pengunjung setiap 1 menit sekali.

Langkah-langkah yang dilakukan adalah sebagai berikut :

1. Buat file cron.xml
2. Konfigurasi file cron.xml untuk dapat mengeksekusi aplikasi pengumuman yang telah dibuat sebelumnya pada alamat URL **/crons/set_announcement**
3. Konfigurasi file cron.xml untuk dapat mengeksekusi aplikasi pengumuman tersebut setiap 1 menit (**every 1 minutes**). maka file cron.xml setelah dikonfigurasi maka akan seperti berikut :
```
<?xml version="1.0" encoding="UTF-8"?>
<cronentries>
<cron>
<url>/crons/set_announcement</url>
<description>pengumuman konferensi sudah hampir penuh</description>
<schedule>every 1 minutes</schedule>
</cron>
</cronentries>
```
4. Setelah selesai konfigurasi file cron.xml maka dapat diunggah pada *AppSpot* kita.
5. Agar mengetahui apakah *cron* berjalan dengan baik maka agar lebih mudah kita pertama sekali membuat konferensi baru dengan pengunjung maksimal lebih kecil dari 5 (lima) orang. Kenapa harus lebih kecil dari 5 (lima) orang, karena pada kode set_anouncement yang sebelumnya sudah dibuat, diatur agar pengumuman dikirimkan ketika ada pengunjung konferensi yang terdaftar sudah mendekati angka pengunjung maksimum (5 orang lagi mencapai angka pengunjung maksimum).
6. Periksa pada *admin console AppEngine* kita apakah *cron* sudah berjalan. Periksa setiap menit apakah data pada *Datastore Query Ops* dan *Datastore API Calls* bertambah, jika bertambah maka *cron jobs* yang kita unggah pada aplikasi kita telah berjalan dengan baik.
7. Jika cron sudah berjalan dengan baik, maka dengan alasan pembelajaran sebaiknya kita menghapus fitur *cron jobs* kita dengan mengunggah cron.xml dengan elemen *cron* yang dikosongkan seperti yang dicontohkan sebelumnya.

Demikianlah penjelasan tentang fitur c*ron jobs* pada *AppEngine* yang bisa mempermudah kita untuk mengeksekusi aplikasi-aplikasi yang dijalankan secara rutin. Selamat mencoba dan semoga bermanfaat.
