# UDACITY/UD859-BAGIAN 2
Saleh Havid / 23213026
## Membuat Aplikasi dengan Menggunakan App Engine dan Google Cloud Platform

#### Bagian ini merupakan lanjutan dari bagian 2  sebelumnya, pada bagian ini akan dipelajari :
* Cara membuat fungsi endpoint
* Cara menggunakan API explorer
* Pengenalan tentang conference central

#### Cara membuat fungsi endpoint
Seperti yang telah dijelaskan pada bagian 2 sebelumnya, suatu proyek app engine terdiri dari beberapa file, pada bagian sebelumnya hello world hanya terdapat 2 buah fungsi yaitu *generic greeting* dan *greet by name*, tampilan muka dari proyek hello world tersebut adalah seperti gambar dibawah ini :

![](https://lh4.googleusercontent.com/Ob_rqHUYgT4NejFu-EFW6vv9vLB3OtihhWOWvjDzCJw9c53M1OcrD30qTqBF1ET8UjkUzkMwx0s=w1332-h547)

pada bagian ini kita akan menambahkan suatu fungsi *greet by periods* yaitu fungsi yang menyapa pengguna berdasarkan waktu yang dipilih oleh pengguna. tampilan muka dari proyek hello world + periods adalah seperti gambar dibawah ini :

![](https://lh3.googleusercontent.com/pk-eGzMmOmS3HeDrfKKoVqkkQkCe4DST8us2nQRaXtRpg0DCeTPkkXcvzEExvTpdUxVkmcTtFfk=w1332-h547)

Untuk menambahkan fungsi *greet by periods* kita perlu menambahkan kode pada beberapa file yaitu:

A. Membuat tambahan button dengan opsi waktu pada file index.html

```
<body>
<P>What time of day is it?
<select  id="period_select">
		    <option value="Day" selected>any time</option>
			<option value="Morning">Morning</option>
			<option value="Afternoon">Afternoon</option>
			<option value="Evening">Evening</option>
			<option value="Night">Night</option>
		</select>
<input id="input_greet_by_period" type="button" value="please wait"
  onclick="will_be_set_after_endpoints_apis_loaded"></P>
  </body>

```

B. Menambahkan kode javascript pada file node.js untuk memanggil fungsi endpoints ketika pengguna mengklik button html yang dibuat
```
function enableButtons () {
	// Set the onclick action for the third button
	btn = document.getElementById("input_greet_by_period");
	btn.onclick=function(){greetByTimeOfDay();};
}
function greetByTimeOfDay () {
	// Get the name from the name_field element
	var name = document.getElementById("name_field").value;
	var period = document.getElementById("period_select").value;

	// Call the greetByPeriod() function.
	// On success, pass the response to sayHelloCallback()
	var request = gapi.client.helloworldendpoints.greetByPeriod(
			{'name': name, 'period' : period});
	request.execute(sayHelloCallback);
}
```

C. Menambahkan fungsi endpoints *(greetByPeriods)* pada file HelloWorldEndpoints.java yang akan dipanggil oleh javascript dan memanggil HelloClass dengan parameter name + timeofday.
```
public class HelloWorldEndpoints {
 // Declare this method as a method available externally through Endpoints
    @ApiMethod(name = "greetByPeriod", path = "greetByPeriod",
            httpMethod = HttpMethod.GET)
    public HelloClass greetByPeriod (@Named("name") String name, @Named("period") String timeofday) {
        return new HelloClass(name, timeofday);
    }
}
```
D. Menambahkan class turunan dari HelloClass pada file HelloClass.java yang menerima parameter name + timeofday dan mengembalikan object message.

```
public class HelloClass {
    public HelloClass (String name, String period) {
        this.message = "Good " + period + " " + name + "!";
    }
}

```
Berikut adalah tampilan tatap muka ketika tombol yang berisi fungsi yang kita buat ditekan. ![](https://lh6.googleusercontent.com/VW7fPwd7u2uIbkBRfr_W_7ExYsSxD9So4iWfBHvCViYXt8b5-fYpho5W1CbcRx7SsdQMoPkzUN8=w1332-h547)


#### Cara menggunakan API explorer

Kita dapat mengecek fungsi endpoint yang kita buat dengan menjalankan applikasi yang kita buat dan membukanya dengan halaman tatap muka, namun kita juga dapat mengecek secara langsung tanpa halaman tatap muka fungsi endpoint yang kita buat dengan menggunakan API explorer. Dibawah ini dapat dilihat proyek hello world mempunyai 3 fungsi endpoints yaitu : greetByPeriods, sayHello dan sayHelloByName

![](https://lh5.googleusercontent.com/HTYcuXuZdJpcrx71FTLrtM1kv3RacShb-BEB31WeFZPQC5u2n66fga8JWqKpl4qffDbMduBG8Qg=w1332-h547)

Kita bisa mencoba fungsi yang baru kita buat yaitu greetByPeriods menggunakan Api explorer, fungsi greetByPeriods membutuhkan 2 parameter yaitu name dan timeofday

![](https://lh4.googleusercontent.com/PVN2eBJ7qmQNAgZ9Nb_3WKpaKb2fWRAddr7q4XG9PtlTjzPCjb5Hb4M3PmtCTQ17F-SjzGm5Zd8=w1332-h547)

Setelah kita mengisi parameter yang dibutuhkan dan mengexecute fungsi endpoint greetByPeriods maka dapat dilihat hasil dari fungsi endpoint greetByPeriods yaitu

![](https://lh6.googleusercontent.com/EHQyE3wbCxxF6CCZ1cVchBwfEWtrJ4DknKiB2HnpGqLkV_aNd02e3y_5y6ou3el7gE6u07g9lfE=w1332-h547)


#### Pengenalan tentang conference central

Bagian terakhir dari modul 2 adalah pengenalan tentang proyek conference central yang akan digunakan sebagai contoh dan target akhir dari pembelajaran seluruh modul.

Conference central adalah suatu website yang digunakan untuk mengatur kegiatan pertemuan, conference central memiliki beberapa menu utama yaitu:
* user dapat login mengggunakan account google
![](https://lh4.googleusercontent.com/Qd4eU0pkvbzrekZPBpTkvC5CblnIGJpVOV6GVjtvnFFhGz958FXagsYva1YbTzxvpKS2ebGGIuk=w1332-h547)
* user dapat melihat pertemuan yang sudah dibuat
![](https://lh6.googleusercontent.com/CghjGEResYcsj1bLAsZau5gpnRwKzF47DVxpe0X5-pefPIx1msKTy-UwBsYd466IQULIrhYO-h4=w1332-h547)
* user dapat membuat pertemuan baru
![](https://lh3.googleusercontent.com/HxssNmBaGS4WB3mbwMP7PsuckKO-JxlzChmWZEBPSNprlCqshADFKIK-eNqQdzLp2UttDFsYalI=w1332-h547)
* user dapat mengupdate profilenya.
![](https://lh5.googleusercontent.com/tVHLMD5fWkkfXT3va0iyvXb9CQmpGbcs8UfgmvFkOBLputPN-tZgX2iys1clEbCKgmBUonvIUvo=w1332-h547)


Module selanjutnya akan membahas cara pembuatan conference central menggunakan google app engine. anda dapat melihat conference central versi lengkap yang sudah dibuat oleh udacity pada halaman berikut :

[conference central udacity at appspot.com](https://conference-central-dot-udacity-extras.appspot.com/)




