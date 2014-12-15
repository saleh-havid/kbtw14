# Memcache
*Junior Arthur Saleky : 23213054*

###Apa itu Memcache ???

Pada dasarnya ketika user mengakses data dalam jumlah yang banyak ke server maka proses retrieve akan berlangsung lama, tetapi apabila data tersebut tersimpan pada memory maka akan mengurangi latency, dari hal tersebut tercipta sebuah mekanisme yang disebut memcache (memory cache).

Memcache bertujuan untuk menyimpan data atau informasi yang peting dengan kapasitas terbatas, sehingga user tidak perlu lagi mengakses ke server/cloud terlalu lama, dengan tujuan mempercepat proses retrieve data.

Gambar 1

Sifat memcache :

+ Bukan sebagai tempat penyimpanan data secara permanen
+ Tidak ada mekanisme security pada memcache
+ Semua data dapat disimpan pada memcache
+ Low latency

Lesson 5 goals :

1.	Save announcements in memcache
2.	Use push queue to send a confirmation email
3.	Run a cron job to generate an announcement

###1.	Save Announcement in Memcache :

Objectify adalah cara bagaimana menggunakan property value dari sebuah class tertentu dengan menambahkan @Cache pada @Entity sehingga tidak perlu menulis setiap baris code pada class untuk melakukan proses tersebut.

Objectify dapat dilakukan dengan menambahkan @Cache pada source code

Gambar 2

Penggunaan memcache pada conference dan profile class, ada beberapa langkah yang harus dilakukan :

+ Deploy conference ke appspot
+ Open app engine account → memcache viewer → flush memcache
+ Create conference → show conference dan view details

Sebelum melakukan create conference user harus melakukan proses copy (web_client_id) kemudian paste pada class berikut :

+ Constants.java
+ App.js
+ Appengine-web.xml

Penggunaan memcache pada announcement dapat dilakuakn dengan menambahkan class baru yaitu Announcement.java. Proses set announcement dilakukan pada beberapa class sebagai berikut :

Gambar 3

Import memcache service pada class SetAnnouncementServlet.java

Gambar 4

Set announcement untuk mengakses class conference pada web.xml

Gambar 5

Set get announcement untuk mengakses class conference pada ConferenceApi.java 

Gambar 6

Proses get announcement dapat dilakukan sebagai berikut :

+ Deploy conference ke appspot
+ Open URL dengan format (projectname.appspot.com/cron/set_announcement)
+ Open Api Explorer (localhost:8080/_ah/api/explorer) pilih conference.getAnnouncemet
+ Open memcache viewer dan search RECENT_ANNOUNCEMENT dengan format string

