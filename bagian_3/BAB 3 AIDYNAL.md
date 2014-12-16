#**DATA STORE**


**AIDYNAL MUSTARI**  -- **23213071**

----------

####**Apa yang dimaksud dengan data store?**

-------------

Google App Engine menyediakan sebuah fitur penyimpanan data yang disebut Data Store. Data Store dapat dianalogikan dengan program-program database server yang sudah umum dipakai, walaupun terdapat beberapa perbedaanyang cukup signifikan.
 
> - **Data store is Not Sequel**
Sequel adalah bahasa query yang digunakan untuk mengakses dan memodifikasi informasi dalam satu atau lebih table datadan deretan database.

> - **Data store is Not RDBMS**
RDBMS adalah Relational Database Management Systems, bahwa penyajian data sebagai kumpulan baris dan kolom.
> - **Data store is a KEY VALUE**
Disebut Key Value, karena proses pengambilan dan penyimpanan data mirip seperti hashdalam programming.  Fungsi Hash adalah digunakan untuk mempercepat pencarian dalam table data/pembandingan data seperti di dalam basis data, mencari duplikasi/kesamaan di sebuah arsip computer. Atau juga bias diartikan indexing dari data dalam array, dengan menggunakan tehnik ini proses pencarian dan penulisan data akan semakin cepat.
Ref: http://en.wikipedia.org/wiki/Hash_table

Data store adalah database yang berjalan diatas Google Cloud. Data store merupakan media penyimpanan tetap yang digunakan oleh AppEngine. Google mendesain khusus database ini untuk memenuhi kebutuhan utama dari AppEngine, yaitu yang bersifat BESAR (BIG), TERSEDIA (AVAILABLE), dan SKALABEL (SCALABLE).

>-  **BESAR**	:	berhubungan dengan sesuatu yang besar; seperti data yang besar, penggunan yang besar, access yang besar, dan kapatias yang besar.
>- **TERSEDIA** 	:	Ketersediaan layanan, artinya kapanpun, dimanapun pengguna dapat mengaksesaplikasi tersebut dan meskipun data center sedang down, pengguna tetap dapat mengakses aplikasi tersebut.
>- **SKALABEL** 	:	kemampuan system untuk menangani pertumbuhan jumlah data tanpa memberikan dampak pada kinerja system. Dengan kata lain, jika terjadi penambahan pengguna secara spesifik, aplikasi dapat meningkatkan kemampuan menangani jumlah request tersebut.


####**DATA STORE MODELLING**

Datastore mempunyai paradigma NoSQL atau database non-relational, semua data object yang disimpan dianggap sebagai suatu entitas, dan setiap entitas mempunyai satu atau lebih properties/atribut. Untuk membedakan satu entitas dengan entitas lain digunakan sebuah parameter bernama kind(jenis), dan juga key sebuah atribut unik yang membedakan antar kind.

+ *KINDS*
+ *ENTITIES*
+ *PROPERTIES*

>- KINDS	:	adalah jenis-jenis class. Class terdapat pada aplikasi server. Class-class tersebut tersebut nantinya digunakan sebagai media yang digunakan aplikasi untuk memuat informasi dan menyimpan informasi tersebut pada data store.
>- ENTITIES 	:	adalah semua data informasi yang disebut juga data objek yang disimpan di datastore. Untuk membedakan satu entitas dengan entitas lain digunakan sebuah parameter bernama kind(jenis). 
>- PROPERTIES : 	adalah semacam field-field atau colum-colum yang digunakan untuk menyimpan entitas.n setiap entitas mempunyai satu atau lebih properties/atribut.

![enter image description here](https://lh3.googleusercontent.com/-uzFV_rLomLM/VI8MBNzsg4I/AAAAAAAAAE0/pGIrL3_CGho/w600-h312-no/kinds+entities+properties.jpg)
 
####**BAGAIMANA CARA MENGGUNAKAN DATA STORE MODELLING?**

![enter image description here](https://lh4.googleusercontent.com/-bVXRMIAakbI/VI8MS3jAfyI/AAAAAAAAAFI/JX11vMGNY_I/w550-h420-no/Picture1.jpg)
 
Untuk mencoba menggunakan entity dan id, kita buka *Com.googlecode.objectify.annotation* di jendela eclips yang telah kita buka.

>- *Com.googlecode.objectify.annotation* adalah open source library yang dapat memberikan kemudahan ketika akan mendefinisikan adanya entity didalam class
>- *@entity* : menandakan bahwa class memiliki entity
>- *@Id* : Id dari sebuah entity. Pada sebuah entity hanya boleh ada 1 Id

####**THE KEY**	

![enter image description here](https://lh4.googleusercontent.com/-ZEVc6HiVkvc/VI8MiHbXqII/AAAAAAAAAFc/l6i7QWW2E2g/w493-h239-no/the+key+1.jpg)
 
 ![enter image description here](https://lh4.googleusercontent.com/-K71qgkbDeIE/VI8MnW252nI/AAAAAAAAAFw/BoZnrFgykMA/w750-h230-no/the+key.jpg)

Setiap Entity yang kita generate akan menghasilkan KEY pada data store. Setiap entitas pasti punya key, jika tidak didefinisikan, maka sistem akan menciptakan sebuah key secara otomatis. Seperti yang terlihat pada gambar diatas.


Id bukan lah key, melainkan sebuah kode unique untuk men-generate key. Sebuah key dapat menyimpan sebuah value, tanpa memperdulikan jenis value. Dengan kata lain, tidak ada skema table yang tetap, tetapi aplikasi client harus mendefinisikan semantic untuk mengetahui jenis data tersebut.

Bentuk dari key-value store berupa ordered tuple yang berisi key value. Nama lain dari key value ini adalah associative array, map, atau dictionary. Ketika menggunakan key-value store, sepasang data, key dan value, dimasukkan ke database berupa dictionary dan value dapat diambil kembali berdasarkan key yang dimiliki.
>-Ada beberapa hal yang harus diperhatikan dalam key-value store antara lain:
>- **Query**
Tidak ada proses query data dalam key-value store, kecuali berdasarkan key.
>- **Skema** 
Dalam key-value store, skema adalah string, semua value dianggap blop. Aplikasi client lah yang menentukan bagaimana mem-parsing data.
>- **Penggunan**
Key-value store sangat berguna ketika pengaksesan data dilakukan menggunakan key. 
Pengaksesan data menggunakan key  sudah cukup umum digunakan. Pada beberapa bagian aplikasi seperti user profiles,user sessions, shopping carts, dan lain-lain, data/value disimpan dalam sebuah data store sehingga mudah ditangani (one request to read, one request to write), hanya dengan menerjemahkan suatu key.


