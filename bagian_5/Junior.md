# Memcache
*Junior Arthur Saleky : 23213054*

###Apa itu Memcache ???

Pada dasarnya ketika *user* mengakses data dalam jumlah yang banyak ke *server* maka proses *retrieve* akan berlangsung lama, tetapi apabila data tersebut tersimpan pada *memory* maka akan mengurangi *latency*, dari hal tersebut tercipta sebuah mekanisme yang disebut *memcache* (*memory cache*).

*Memcache* bertujuan untuk menyimpan data atau informasi yang peting dengan kapasitas terbatas, sehingga *user* tidak perlu lagi mengakses ke *server*/*cloud* terlalu lama, dengan tujuan mempercepat proses *retrieve* data.

![enter image description here](https://lh5.googleusercontent.com/YLp5jpAFxsfW2-9UIRcBX8bN-K80dRwKi3ga5bovJQ=s0 "1.png")

####Sifat *memcache* :

+ Bukan sebagai tempat penyimpanan data secara permanen
+ Tidak ada mekanisme *security* pada *memcache*
+ Semua data dapat disimpan pada *memcache*
+ *Low latency*

####Lesson 5 goals :

1.	*Save announcements in memcache*
2.	*Use push queue to send a confirmation email*
3.	*Run a cron job to generate an announcement*

###1.	Save Announcement in Memcache :

*Objectify* adalah cara bagaimana menggunakan *property value* dari sebuah *class* tertentu dengan menambahkan *@Cache* pada *@Entity* sehingga tidak perlu menulis setiap baris *code* pada *class* untuk melakukan proses tersebut.

*Objectify* dapat dilakukan dengan menambahkan *@Cache* pada *source code*

![enter image description here](https://lh4.googleusercontent.com/2hmmWqp8ha4ojBN_AeX2wW_vCH-Ck6BDp3y7sHoJyQ=s0 "2.png")

Penggunaan *memcache* pada *conference* dan *profile class*, ada beberapa langkah yang harus dilakukan :

+ *Deploy conference* ke *appspot*
+ *Open app engine account* → *memcache viewer* → *flush memcache*
+ *Create conference* → *show conference dan view details*

Sebelum melakukan *create conference*, *user* harus melakukan proses *copy* (web_client_id) kemudian paste pada *class* berikut :

+ Constants.java
+ App.js
+ Appengine-web.xml

Penggunaan *memcache* pada *announcement* dapat dilakuakn dengan menambahkan *class* baru yaitu Announcement.java. Proses *set announcement* dilakukan pada beberapa *class* sebagai berikut :

![enter image description here](https://lh4.googleusercontent.com/a3z_SpioPvFa_0JB1gOOB1PD-iu9hZRVW9Biy3hW598=s0 "3.png")

*Import memcache service* pada *class* SetAnnouncementServlet.java

![enter image description here](https://lh4.googleusercontent.com/ovLTYifr2gBZl1J13PR9-yKkkv2jNUJ8nD-0-FULqFI=s0 "4.png")

*Set announcement* untuk mengakses *class conference* pada web.xml

![enter image description here](https://lh6.googleusercontent.com/SbtJH79bdwUWgSzFYLpJW_sN6XSw25fwtt4fPbnQ1dM=s0 "5.png")

*Set get announcement* untuk mengakses *class conference* pada ConferenceApi.java 

![enter image description here](https://lh4.googleusercontent.com/6eAAd0SI6Z_0wjMetHnzh0F6Xy3mcHJ2BmV_pRDgFqI=s0 "6.png")

Proses *get announcement* dapat dilakukan sebagai berikut :

+ *Deploy conference* ke *appspot*
+ *Open* URL dengan format (projectname.appspot.com/cron/set_announcement)
+ *Open Api Explorer* (localhost:8080/_ah/api/explorer) pilih conference.getAnnouncemet
+ *Open memcache viewer* dan *search* RECENT_ANNOUNCEMENT dengan format *string*
