# Memcache
*Junior Arthur Saleky : 23213054*

##Apa itu Memcache ???

Pada dasarnya ketika user mengakses data dalam jumlah yang banyak ke server maka proses retrieve akan berlangsung lama, tetapi apabila data tersebut tersimpan pada memory maka akan mengurangi latency, dari hal tersebut tercipta sebuah mekanisme yang disebut memcache (memory cache).

Memcache bertujuan untuk menyimpan data atau informasi yang peting dengan kapasitas terbatas, sehingga user tidak perlu lagi mengakses ke server/cloud terlalu lama, dengan tujuan mempercepat proses retrieve data.

Memcache proses dapat dilihat pada gambar dibawah ini
https://plus.google.com/u/0/photos/yourphotos?pid=6093042276768456610&oid=111167142726180373331&authkey=CNKI76yQjfGFfA

Sifat memcache :

+ Bukan sebagai tempat penyimpanan data secara permanen
+ Tidak ada mekanisme security pada memcache
+ Semua data dapat disimpan pada memcache
+ Low latency

Lesson 5 goals :

1.	Save announcements in memcache
2.	Use push queue to send a confirmation email
3.	Run a cron job to generate an announcement

##1.	Save Announcement in Memcache :

Objectify adalah cara bagaimana menggunakan property value dari sebuah class tertentu dengan menambahkan @Cache pada @Entity sehingga tidak perlu menulis setiap baris code pada class untuk melakukan proses tersebut.

Objectify dapat dilakukan dengan menambahkan @Cache pada source code
https://plus.google.com/u/0/photos/yourphotos?pid=6093042570949262962&oid=111167142726180373331&authkey=CKWA_PbfmfL9RQ

Penggunaan memcache pada conference dan profile class, ada beberapa langkah yang harus dilakukan :

+ Deploy conference ke appspot
+ Open app engine account → memcache viewer → flush memcache
+ Create conference → show conference dan view details

Sebelum melakukan create conference user harus melakukan proses copy (web_client_id) kemudian paste pada class berikut :
+ Constants.java
+ App.js
+ Appengine-web.xml

Penggunaan memcache pada announcement dapat dilakuakn dengan menambahkan class baru yaitu Announcement.java. Proses set announcement dilakukan pada beberapa class sebagai berikut :
https://plus.google.com/u/0/photos/yourphotos?pid=6093042649859868514&oid=111167142726180373331&authkey=CNvR_5Pwia7Z_QE

Melakukan import memcache service pada class SetAnnouncementServlet.java
https://plus.google.com/u/0/photos/yourphotos?pid=6093042743856830578&oid=111167142726180373331&authkey=CLzM2b3KhtjljAE

Melakukan set announcement 





