# Bagian 1

#####Heriansyah / 23213017 -- EL6240 Komputasi Bergerak dan Teknologi Web
#####**Lesson 1: Introduction of Developing Scalable Apps with Java (with Google App Engine)**

### Pengantar
Bagian ini merupakan awal dari kursus yang berjudul "*Developing Scalable Apps with Java (with Google App Engine)*" yang ada di situs resmi Udacity. Bagian ini akan menyampaikan pengantar tentang bagaimana cara membangun *scalable application* di infrastruktur Google. Kursus ini akan disampaikan langsung oleh pihak Google yaitu Magnus dan Jocelyn (Divisi Cloud Platform).

### Sejarah *Server* Aplikasi
Beberapa puluh tahun yang lalu, *server* aplikasi dijalankan di sebuah komputer fisik seperti yang terlihat pada gambar di bawah ini. Agar dapat melayani pengguna yang lebih banyak, maka cara yang harus ditempuh adalah menambah jumlah komputer yang ada. Untuk menghemat tempat, komputer-komputer tersebut disusun di sebuah rak sehingga terciptalah kumpulan rak yang disimpan di dalam sebuah ruangan yang disebut *data center*.

![Gambar Old Data Center](https://lh4.googleusercontent.com/-S6vqivt4CGA/VI7UCZkHfBI/AAAAAAAABmU/RzZ2MUS9D2o/w527-h282-no/server%2B.jpg)

Selain pertumbuhan pengguna *internet* yang terus meningkat, penyebab lain dari bertambahnya tumpukan komputer di ruangan tersebut adalah fakta bahwa satu aplikasi hanya bisa berjalan pada satu *server* saja. Dengan pertumbuhan *internet* yang melaju pesat seperti sekarang ini, keadaan di atas sudah tidak layak untuk diterapkan. Aplikasi-aplikasi yang dibangun oleh masing-masing pengembang semakin terkenal dan membutuhkan sumber daya yang lebih banyak sehingga dibutuhkan solusi baru yang dapat mengatasi masalah di atas.

![Gambar one application on one server](https://lh6.googleusercontent.com/-v_5G431AAAY/VI7Y6cO7o4I/AAAAAAAABoE/KAEjOQYQh3E/w567-h277-no/one%2Bserver.jpg)

### Solusi yang Dibutuhkan
Salah satu cara yang bisa dilakukan untuk mengatasi masalah di atas adalah merancang suatu arsitektur jaringan yang terdistribusi dan tidak berjalan hanya pada satu komputer saja. Arsitekur ini disebut "*Shared Nothing Architecture*". Arsitektur ini tidak bergantung pada *resource sharing* yang ada sehingga *bottleneck* dapat dihindari. Dengan cara ini, jika aplikasi yang kita bangun semakin berkembang maka yang harus kita lakukan hanya menambah beberapa komputer untuk meningkatkan kapasitas yang dibutuhkan dan proses penambahan kapasitas ini tidak menyebabkan *server* yang sudah berjalan *down* atau mati. Artinya, aplikasi yang kita punya akan terus berjalan meskipun pada saat bersamaan penambahan kapasitas sedang dilakukan.

![Gambar Shared Nothing Architecture](https://lh3.googleusercontent.com/-4UTLZa8ZSRc/VI7ZDRKjx2I/AAAAAAAABok/efAD1UyGyEk/w453-h270-no/VM.jpg)


Untuk membuat sistem di atas lebih fleksibel maka lahirlah ide baru untuk membuat sebuat mesin virtual. Mesin virtual dapat membantu kita untuk menciptakan komputer virtual dengan sistem operasi virtual dan perangkat keras virtual. Dengan mesin virtual, infrastuktur yang kita miliki telah berubah menjadi virtual sehingga kita dapat menjalankan aplikasi yang kita miliki cukup di mesin virtual saja. Dengan demikian, solusi dari permasalahan yang kita hadapi di atas telah terjawab. Selanjutnya muncul pertanyaan baru yaitu bagaimana cara kita mewujudkan sistem seperti ini.

![Gambar Virtual Machine](https://lh5.googleusercontent.com/-vNQWBCQBP1E/VI7Y_sc9B3I/AAAAAAAABoU/u0y8MzFyZ0A/w518-h260-no/shared%2Bnothing.jpg)

## *Infrastructure as a Service*
Saat ini layanan mesin virtual merupakan layanan utama yang disediakan oleh penyedia jasa layanan awan. Kita bisa menggunakan jasa mereka dengan cara membayar layanan yang mereka berikan per satuan waktu (per jam atau per menit). Sistem layanan seperti ini disebut sebagai *infrastucture as a Service* atau IaaS. Penyedia layanan akan menyediakan semua infrastruktur yang kita butuhkan untuk menjalankan aplikasi yang akan kita bangun seperti CPU, memori, media penyimpan data dan lain-lain. Komponen-komponen tersebut digunakan untuk membangun mesin virtual yang kita butuhkan. Salah satu keuntungan menggunakan layanan IaaS adalah tidak perlu membeli komputer fisik sehingga lebih menghemat biaya. Konfigurasi komputer virtual juga bisa diubah sesuai kebutuhan. Jika ingin menambah kapasitas tempat penyimpan data, cukup dengan menghubungi penyedia layanan maka kapasitas penyimpanan data yang kita punya akan bertambah dengan segera.

Kembali pada pertanyaan sebelumnya, dengan IaaS maka masalah di atas telah terpecahkan. Namun jika dilihat lebih jauh lagi, menerapkan arsitektur ini dalam mengembangkan aplikasi yang akan dibangun ternyata sangat rumit dan memakan banyak tenaga, waktu dan pikiran. Bayangkan, kita harus memikirkan tentang manajemen dari mesin virtual, manajamen *data base*, manajemen *file system*, *load balancing*, *application monitoring*, keamanaan dan lain-lain. Semua hal di atas harus kita kerjakan sendiri karena layanan di atas tidak termasuk dalam layanan yang sudah kita sewa sebelumnya. Selain hal di atas, para pengembang aplikasi juga dituntut untuk berkonsentrasi dalam mengembangkan aplikasi yang telah mereka bangun. Jadi dapat disimpulkan, jika kita tidak memiliki sumber daya yang cukup maka akan sulit untuk menerapkan sistem ini dikarenakan keterbatasan waktu yang kita miliki. Selanjutnya muncul pertanyaan baru, arsitektur seperti apa yang cocok untuk membangun aplikasi yang *scalable*?

![Gambar Manajemen IaaS](https://lh5.googleusercontent.com/-tTO3F8CkeiM/VI7XeZcJYlI/AAAAAAAABnk/KaxzF9YNEYg/w563-h267-no/complicated%2BIaaS.jpg)

### *Platform as a Service* (Google Cloud Platform: App Engine)
Solusi selanjutnya adalah menggunakan *Platform as a Service* atau PaaS. PaaS menawarkan layanan *platform* yang kita butuhkan untuk membuat aplikasi. PaaS telah menyediakan sistem operasi, *database*, *web server* dan *framework* agar kita dapat menjalankan aplikasi yang telah kita buat. Keuntungan layanan PaaS  bagi pengembang adalah mereka bisa fokus pada aplikasi yang mereka buat tanpa memikirkan tentang pemeliharaan di sisi *back-end*. Salah satu penyedia layanan PaaS yang ada saat ini adalah Google dengan Google Cloud Platform-nya. Produk dari Google CLoud Platform sendiri adalah Google App Engine. Pada kursus ini kita akan menggunakan Google App Engine dalam membangun aplikasi yang akan kita buat.

![Gambar Google App Engine as PaaS](https://lh5.googleusercontent.com/-2WtgE7uSrwY/VI7YQCWNXoI/AAAAAAAABn0/zqAacIgnXIg/w554-h259-no/PaaS.jpg)

Dengan menggunakan Google App Engine maka semua kerumitan yang ada pada layanan IaaS telah teratasi. Anda tidak perlu memikirkan tentang manajemen mesin virtual, keamanan, *load balancing* dan lain-lain. Jika aplikasi kita sedang sibuk, maka Google App Engine akan secara otomatis meningkatkan kemampuan dan kapasitasnya (*scaling up*) dan ketika aplikasi sedang tidak banyak diakses maka Google App Engine secara otomatis menurunkan kemampuan dan kapasitasnya untuk menghemat *resources* dan biaya (*scaling down*). Untuk mendapatkan keuntungan ini kita tidak perlu merancang atau menulis kode program apa pun karena semuanya telah dikerjakan dan diawasi sepenuhnya oleh Google sehingga para pengembang dapat fokus sepenuhnya pada aplikasi yang sedang mereka bangun (*front-end*). Google App Engine sangat cocok untuk pengembang baik dalam skala kecil, menengah ataupun besar. Anda dapat membangun aplikasi berskala kecil seperti situs portofolio pribadi atau membangun aplikasi berskala besar seperti situs berita New York Times atau aplikasi *video game*.

### Kekurangan Google App Engine
Selain banyak keuntungan di atas, ada beberapa hal yang perlu diperhatikan dalam membangun aplikasi di Google App Engine. Saat ini Google App Engine hanya mendukung beberapa bahasa pemrograman saja seperti Java, PHP, Python dan Go. Selain itu, tidak semua versi dari bahasa pemrograman tersebut bisa digunakan di Google App Engine. Artinya hanya versi-versi tertentu saja yang bisa digunakan.

Selanjutnya di sisi *low-level control*, pengaturan dasar seperti tipe dan versi dari sistem operasi, konfigurasi mesin virtual, *network control* dan lain-lain tidak dapat diatur sesuai dengan keinginan kita. Hal ini dilakukan karena Google App Engine memang dirancang sedemikian sederhana agar para pengembang dapat dengan mudah menggunakannya dan hanya fokus pada pengembangan aplikasinya saja. Hal ini mungkin akan mengecewakan bagi beberapa pengembang yang ingin mengatur bagian *low-level control*.


### Kisah Sukses Menggunakan Google App Engine
Kisah sukses pertama datang dari sebuah aplikasi *real-time* buatan NIED (National Research Institute for Earth Science and Earth Disaster Prevention) untuk memantau aktivitas gempa yang ada di Jepang. NIED mengumpulkan banyak gambar, mengamatinya lalu mengirimkan gambar-gambar tersebut ke Google App Engine setiap detik. Para pengguna dapat melihat gambar-gambar tersebut untuk mengetahui keadaan terkini dari suatu wilayah di Jepang. Pada saat tidak ada gempa maka pengguna yang mengakses aplikasi ini sangat sedikit, sebaliknya jika gempa terjadi maka aplikasi ini akan sangat sibuk melayani permintaan dari setiap penggunanya. Dengan kemampuan *scaling up* dan *scaling down* yang sangat cepat, Google App Engine dapat mengatasi masalah ini dengan baik.

![NIED](https://lh5.googleusercontent.com/-u4mLfZkFbqQ/VI7UAFfXN9I/AAAAAAAABl8/65FkkfPmFX8/w256-h269-no/Nied%2Bok.jpg)

Kisah sukses yang kedua datang dari aplikasi permainan tebak lagu atau penyanyi yang bernama Song Pop. Song Pop tumbuh dari 0 hingga 10000 *queries* per detik dalam waktu 6 bulan dan pada awal tahun 2013 pengguna aplikasi ini sudah mencapai 60 juta. Song Pop menggunakan Google App Engine karena kemampuannya yang cepat dalam *scaling up* dan *scaling down*.

![Song Pop](https://lh4.googleusercontent.com/NLIq2HFLe75Nf4nPaIp4HpIvpUhPBZa9WzoSc0b5Yrs=w332-h173-p-no)

Kisah sukses yang ketiga datang dari Rovio, perusahaan pembuat permainan terkenal Angry Birds. Ketika mereka membangun permainan Angry Birds untuk versi *web*, mereka memutuskan untuk menggunakan Google App Enginge dengan alasan yang sama seperti di atas. Selain ketiga kisah sukses di atas, masih banyak kisah sukses lainnya yang tidak bisa dicantumkan satu persatu di tulisan ini termasuk situs Udacity yang sedang kita gunakan untuk mengikuti kursus "*Developing Scalable Apps with Java* (*with Google App Engine*)".

![Angry Birds](https://lh4.googleusercontent.com/GkqvJiDz-D537WZid09TgzXcQoq36sjLWLqWRR5bkjw=w309-h173-p-no)


