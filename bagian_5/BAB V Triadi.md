# Lesson 5 - Advanced App Engine Topics
*by Triadi Wirawan - 23213370*
## Task Queue

####*Task queue concepts*

*Task queue* merupakan suatu fitur yang efisien dan ampuh untuk suatu proses yang dilakukan pada bagian *back-end* suatu aplikasi. Fitur ini memungkinkan kita untuk menentukan perintah, mengumpulkannya, dan memprosesnya secara keseluruhan. Kita dapat memberi nama pada *queues* dan mengatur isinya pada file bernama ```queue.xml```.

![](https://cloud.githubusercontent.com/assets/1955763/5456564/0ab23d0a-8577-11e4-8be8-01f0e066630a.jpg)

Ada dua tipe task queue, **Push Queue** dan **Pull Queue** :

#### 1. Push Queue

**Push queue** yang memiliki default task queue, sangat cocok digunakan pada aplikasi yang memiliki perintah/*task* yang hanya menggunakan perangkat atau layanan App Engine saja, karena push queue ini hanya dapat bekerja dalam lingkungan App Engine.

Dengan push queue ini, kita cukup mengatur antrian/*queue* dan menambahkan perintah kedalamnya, selebihnya dikerjakan oleh App Engine sendiri.

**Konfigurasi push queue** bisa dilakukan pada file konfigurasi bernama ```queue.xml``` pada folder ```WEB-INF/``` didalam WAR. Setiap aplikasi memiliki *default* *push queue* dengan beberapa konfigurasi *default*.



