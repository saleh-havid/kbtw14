# UDACITY/UD859-BAGIAN 2

Nadia Thereza/23213040

## Membuat Aplikasi dengan Menggunakan *App Engine* dan *Google Cloud Platform*
Tujuan umum dari bagian 2 adalah membuat aplikasi yang *scalable* dengan menggunakan *App Engine* dan *Google Cloud Platform*, yang nantinya aplikasi tersebut dapat dijalankan pada *localhost* dan *Google Data Center*. Secara spesifik ada empat buah target yang akan dicapai pada bagian 2 ini. Keempat terget tersebut adalah:

1. Membuat proyek *App Engine*

1. Mengeksplorasi *Cloud Enpoints*

1. Membuat 'Hello World' menggunakan *App Engine*

1. Memperkenalkan *Conferene Central*

Langkah pertama dalam pembuatan aplikasi menggunakan *App Engine* adalah Anda harus memiliki akun Google yang dapat digunakan pada berbagai macam layanan Google, seperti: Gmail, YouTube, dan lainnya. Akun Google inilah yang mengizinkan pembuat aplikasi untuk membuat aplikasi-aplikasi *App Engine* yang dapat dijalankan pada *Google Cloud*.

**1. Membuat Proyek *App Engine***


Setelah Anda memiliki akun Google, untuk membuat sebuah proyek Anda hanya perlu *log-on* pada *developers console* dan daftarkan aplikasi *App Engine* yang ingin Anda buat. *Developers console* dapat diakses pada alamat (https://console.developers.google.com). Project-ID harus unik dan terdiri dari minimal enam karakter, karakter yang diperbolehkan adalah angka, kata dengan huruf kecil, dan karakter *dash* (-).

![alt text](https://lh6.googleusercontent.com/-GQ1cVss4L1I/VI-hgtgv9JI/AAAAAAAAAKw/kUXTKnsvt68/w534-h297-no/pic1.png)

Setelah Anda selesai membuat sebuah ID-proyek, berikut adalah alamat yang dapat Anda gunakan untuk mengakses proyek tersebut: (http://[project-id].appspot.com).


**2. Mengeksplorasi *Cloud Enpoints***

Langkah selanjutnya, Anda dapat langsung melakukan *coding*. Namun, sebelum itu akan dibahas terlebih dahulu mengenai *Cloud Endpoints*.

Tentunya, aplikasi yang dibuat diharapkan dapat digunakan oleh berbagai macam alat dan dari berbagai macam vendor (*browser*, *smartphone*, tablet dan komputer). Seperti contoh, aplikasi yang dapat diakses menggunakan *web browser* nantinya dapat juga diakses pada *smartphone* tanpa harus membuat kembali tiap-tiap baris kodenya. Oleh sebab itu, *Cloud Enpoints* hadir sebagai solusi dalam permasalahan *different devices*.

![alt text](https://lh5.googleusercontent.com/-LFADLHgOqQA/VI-hjccGE-I/AAAAAAAAAL8/B70XI1hzUkU/w761-h441-no/pic2.png)

Dengan menggunakan *Cloud Endpoints*, Anda dapat membuat satu API (*Application Programming Interface*) saja untuk aplikasi server Anda, yang disebut *Cloud Endpoint API*. *Cloud Endpoints* akan menghasilkan API sisi klien (*client side API*) untuk berbagai macam alat dari berbagai macam vendor. Jadi, Anda sebagai pembuat aplikasi diizinkan untuk fokus pada *server side application*, *Cloud Endpoints* yang akan memastikan bahwa aplikasi tersebut dapat diakses oleh berbagai macam alat.

**3. Membuat 'Hello World' menggunakan *App Engine***

Melanjutkan dari beberapa target di atas, pada bagian 2 ini akan dilakukan beberapa hal, yaitu:

* Menjalankan aplikasi *App Engine* yang sangat sederhana dengan menggunakan *Endpoints*

* Mempelajari fungsi-fungsi dari *Endpoints*

* Mempelajari cara menggunakan *API explorer*

Pada tahapan ini, akan dilakukan latihan dalam menjalankan aplikasi *App Engine* sederhana. Aplikasi yang digunakan dalam latihan ini adalah aplikasi 'Hello World'. Kode dapat diunduh di repositori kode dengan alamat (https://github.com/udacity/ud859). Selanjutnya memasang program-program yang dibutuhkan dalam pembuatan aplikasi dengan menggunakan *App Engine*, program-program tersebut adalah sebagai berikut:


* **JDK 1.7**, dapat diunduh di alamat:

 (http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)


* **Maven**, dapat diunduh di alamat:

 (http://maven.apache.org/download.cgi)


* **Eclipse EE**, dapat diunduh di alamat:

 (https://www.eclipse.org/downloads/)

Catatan: Jika dalam pemasangan program-program di atas terdapat permasalahan maka dapat mengunjungi alamat

(https://docs.google.com/document/d/1EO3pQ53DQP3SW4LCnOLiUtgAKCWbqXVprBWj0TOlRUM/pub)

Setelah kode untuk aplikasi 'Hello World' diunduh dan program-program yang dibutuhkan telah dipasang, maka Anda dapat langsung membuat aplikasi 'Hello World'. Berikut adalah tahapannya:

* Impor 'helloworld-endpoints' dari folder repositori kode 'Lesson 2' pada Eclipse EE!

* Setelah proyek 'helloworld' muncul, pilih 'Run configuration' dengan Goals: 'appengine:devserver'!

![alt text](https://lh3.googleusercontent.com/-OdzWmVCKuWk/VI-hkLdh2ZI/AAAAAAAAALA/LsJ1Ro2x6k8/w798-h445-no/pic3.png)


Pembuatan pertama kali memerlukan waktu yang cukup lama, karena Maven akan mengunduh semua *dependency* (Google App Engine SDK, dll). Setelah proses berhasil, maka Anda telah dapat menelusuri aplikasi 'Hello World' pada localhost (http://localhost:8080).

![alt text](https://lh4.googleusercontent.com/-6z4DmYBrKlA/VI-hiZsmZ_I/AAAAAAAAAKk/6LVAE-q5iv8/w366-h368-no/pic14.png)

Selanjutnya, untuk membuat proyek 'Hello World' dapat dijalankan pada *Google Data Center*, maka tahapannya adalah sebagai berikut:

* 'Run Configuration' dengan Goals: 'appengine:update'!


![alt text](https://lh6.googleusercontent.com/-UKhX1Q1vcJQ/VI-hkZ2ClkI/AAAAAAAAALs/2cACdeYHXNw/w847-h490-no/pic4.png)


* Terima permintaan dari *Google App Engine appcfg*! (Pastikan App-ID Anda benar!)

![alt text](https://lh4.googleusercontent.com/-IZIony4OZPU/VI-n01u_X1I/AAAAAAAAAMU/bY3I5jYpj0w/w286-h274-no/pic15.png)

* Salin kode yang diberikan kemudian tempel pada console di Eclipse EE!


Jika proses berhasil, Anda dapat langsung menelusuri aplikasi tersebut pada alamat

(https://[app-id].appspot.com).

![alt text](https://lh5.googleusercontent.com/-PL3z5dD2dZk/VI-hktFwguI/AAAAAAAAALg/4S-E9kdXTzk/w379-h375-no/pic5.png)

Selanjutnya adalah mempelajari fungsi-fungsi (kode-kode) *Endpoints*. Namun sebelumnya dapat dilihat pada file web.xml konfigurasi yang menunjukkan bahwa aplikasi yang dijalankan memiliki fungsi *endpoints*, konfigurasi tersebut adalah sebagai berikut:

* SystemServiceServlet

* com.google.api.server.spi.SystemServiceServlet ('spi' menunjukkan *endpoints*)


Di bawah ini akan dijelaskan secara singkat mengenai fungsi-fungsi *Endpoints*:

* Security constraint: menunjukkan bahwa pada saat ingin mengakses appspot tanpa menggunakan https, akan tetap dapat terhubung langsung ke https

* Constant.java: berisi ID klien dan cakupan untuk memungkinkan klien menggunakan API Anda serta berisi fungsi Javascript yang digunakan oleh aplikasi

![alt text](https://lh6.googleusercontent.com/RutWYw2vunmYw7CS3jkTBkgAgH5VyGRBxokbPu9pynY=w690-h251-no)


* HelloClass.java: menunjukkan kode-kode aplikasi

![alt text](https://lh4.googleusercontent.com/-T6Ol-lYaumo/VI-hl7E6B-I/AAAAAAAAALc/P8m7VJLmaVA/w367-h309-no/pic8.png)


* HelloWorldEndpoints.java: menunjukkan fungsi API *endpoints*

![alt text](https://lh6.googleusercontent.com/-EHrnw0TScP4/VI-hnBiWC7I/AAAAAAAAALo/4WKb6l0xUd8/w550-h470-no/pic9.png)

* hello.js: berisi fungsi javascript untuk memanggil *backend endpoint*

![alt text](https://lh6.googleusercontent.com/-8HHhZnHbMpE/VI-hhDD4IjI/AAAAAAAAAKg/IAHURGAEhfo/w590-h470-no/pic10.png)

* index.html: *homepage* untuk aplikasi pom.xml  (contohnya: maven configuration, sdk option, dll)

![alt text](https://lh6.googleusercontent.com/-N3Yc18IZukw/VI-hg89Y1gI/AAAAAAAAAKs/v3r0mXfn3X4/w630-h470-no/pic11.png)

* appengine-web.xml: file konfigurasi *App Engine* (contohnya:app-id)

![alt text](https://lh3.googleusercontent.com/-HJ8pI2JHkSM/VI-hhRRLsRI/AAAAAAAAAKc/P9kO3PUS0cA/w833-h240-no/pic12.png)

* web.xml: file konfigurasi *webapp* (contohnya: welcome files, servlet, security constraint, dll)

![alt text](https://lh6.googleusercontent.com/-KMfRzGk90cg/VI-hhhOyG4I/AAAAAAAAAKY/fgWxGweuMDM/w832-h295-no/pic13.png)

Tahapan selanjutnya pada bagian 2 ini adalah mempelajari cara membuat fungsi *endpoint* dilanjutkan dengan mempelajari cara menggunakan *API explorer* dan diakhiri dengan memperkenalkan *Conference Central*. Tahapan-tahapan tersebut akan dilanjutkan pada pembahasan UDACITY/UD859-BAGIAN 2 berikutnya.







