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

**Membuat *Task*** bisa dilakukan dengan 3 tahap sebagai berikut :
1. Memanggil *Queue* berdasarkan nama
2. Membuat *Task* ( Nama *Task*, URL, dll )
3. Menambahkan *Task* kedalam *Queue*

Untuk menambahkan *queue*, kita mengambil ```Queue``` menggunakan ```QueueFactory```, lalu panggil dengan menambahkan *method* ```add()```. Kita bisa mendapat nama dari *queue* yang kita inginkan pada file ```queue.xml``` dengan menggunakan *method* ```getQueue()```, atau jika kita ingin menggunakan *default queue* bisa menggunakan ```getDefaultQueue()```. *Method* ```add()``` pada penambahan *queue* tadi bisa kita panggil dengan menggunakan ```TasOptions``` yang dihasilkan oleh ```TaskOptions.Builder```, atau kita bisa memanggilnya dengan tanpa *argument* untuk mendapatkan opsi *default* untuk queue tersebut.

Berikut ini contoh code menambahkan *task* pada *queue* :

Pada ```index.html``` :
```
<!-- A basic index.html file served from the "/" URL. -->
<html>
  <body>
    <p>Enqueue a value, to be processed by a worker.</p>
    <form action="/enqueue" method="post">
      <input type="text" name="key">
      <input type="submit">
    </form>
  </body>
</html>```

Pada ```Enqueue.java``` :
```
// The Enqueue servlet should be mapped to the "/enqueue" URL.
import com.google.appengine.api.taskqueue.Queue;
import com.google.appengine.api.taskqueue.QueueFactory;
import static com.google.appengine.api.taskqueue.TaskOptions.Builder.*;

public class Enqueue extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String key = request.getParameter("key");

        // Add the task to the default queue.
        Queue queue = QueueFactory.getDefaultQueue();
        queue.add(withUrl("/worker").param("key", key));

        response.sendRedirect("/");
    }
}```

Pada ```Worker.java``` :
```
// The Worker servlet should be mapped to the "/worker" URL.
public class Worker extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String key = request.getParameter("key");
        // Do something with key.
    }
}```

*Task* yang ditambahkan pada *queue* ini akan dieksekusi dengan memanggil *request handler* di URL ```/worker``` dengan parameter ```key```. App Engine akan mengeksekusi sesuai dengan konfigurasi yang ada di ```queue.xml```, atau dengan menggunakan konfigurasi *default* yakni 5 *tasks* per detik.

Kita bisa mengatur konfigurasi pada ```queue.xml``` seperti contoh berikut :
```
<queue-entries>
  <queue>
    <name>emailqueue</name>
    <rate>1/s</rate>
  </queue>
</queue-entries>```

#### 2. Pull Queue
**Pull Queue** dapat membuat *task consumer*, baik aplikasi kita sendiri maupun kode yang berada diluar aplikasi kita, untuk memakai perintah/*task* pada waktu tertentu untuk diproses dalam jangka waktu tertentu pula. *Pull queue* memberi kita kontrol yang lebih besar pada saat *task* diproses, dan juga mengizinkan kita untuk mengintegrasi aplikasi kita dengan kode selain dari App Engine ( menggunakan *Task Queue REST API *)
![](https://cloud.githubusercontent.com/assets/1955763/5466656/9937898e-85e2-11e4-81b6-eb251c4f9cc0.jpg)

Dalam menggunakan *Pull queues* aplikasi kita harus bisa melakukan beberapa fungsi dimana pada *Push queues* hal tersebut sudah otomatis dilakukan, yaitu :

* Aplikasi yang kita miliki harus memiliki jumlah *workers* yang dapat disesuaikan dengan besarnya volume *processing* yang dibutuhkan. Jika aplikasi kita tidak bisa menyesuaikan, maka aplikasi tersebut bisa menyia-nyiakan proses komputasi jika tidak ada perintah/*task* yang harus dilakukan dan juga bisa beresiko membengkaknya *latency* ketika begitu banyak *task* yang harus dilakukan.

* Aplikasi kita harus bisa menghapus *tasks* yang telah diproses. Pada *Push queue*, App Engine otomatis menghapusnya, namun tidak di *Pull queue*. Jika aplikasi kita tidak menghapus *pull queue tasks* setelah diproses, *workers* bisa jadi akan memprosesnya kembali. Hal ini tentu akan menyia-nyiakan proses komputasi dan bahkan bisa menimbulkan *error*.

Sama halnya dengan *Push queues*, untuk mengkonfigurasi *Pull queues* dilakukan pada file ```queue.xml``` seperti contoh dibawah ini :
```
<queue-entries>
  <queue>
    <name>pull-queue</name>
    <mode>pull</mode>
    <acl>
      <user-email>bar@foo.com</user-email>      <!-- can list, get, lease, delete, and update tasks -->
      <writer-email>user@gmail.com</writer-email> <!-- can insert tasks -->
      <writer-email>bar@foo.com</writer-email>  <!--  can insert tasks, in addition to rights granted by being a user_email above -->
    </acl>
  </queue>
</queue-entries>```

Kita bisa menspesifikkan suatu nama *pull queue* dengan menambah elemen ```<mode>pull</mode>``` di file ```queue.xml``` tersebut.

Dan jika kita menggunakan *Task Queue REST API* kita perlu membuat *Access Control List* (ACL) dengan menambahkan tag ```<acl>```. Tag ini untuk memberikan batasan akses pada alamat email yang tercantum didalamnya. Elemen ```acl``` memiliki 2 parameter :

* ```user-email``` : *users* dapat membuat *list, get, lease, delete, update tasks*
* ```writer-email``` : *users* dapat menambahkan *tasks*

Dalam *pull queues* apabila suatu *task* gagal diproses, kita bisa mengatur agar *task* tersebut dicoba diproses kembali. Banyaknya percobaan proses ulang ketika gagal tersebut bisa diatur menggunakan elemen ```<retry-parameters>``` dan ```<task-retry-limit>``` seperti pada contoh dibawah ini :

```
<queue-entries>
  <queue>
    <name>pull-queue</name>
    <mode>pull</mode>
    <retry-parameters>
      <task-retry-limit>7</task-retry-limit>
    </retry-parameters>
  </queue>
</queue-entries>```

####So, which queue - Push or Pull ?
|  | Push | Pull |
| -- | -- | -- |
| Package Delivery Service | &#10008; | &#10003;|
| Code Review | &#10008; | &#10003; |
| Batch processing images | &#10003; | &#10008;|


