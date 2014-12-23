# UDACITY/UD859-BAGIAN 2
Saleh Havid / 23213026
## Membuat Aplikasi dengan Menggunakan App Engine dan Google Cloud Platform

#### Bagian ini merupakan lanjutan dari bagian 2  sebelumnya, pada bagian ini akan dipelajari :
* Cara membuat fungsi endpoint
* Cara menggunakan API explorer
* Pengenalan tentang conference central

#### Cara membuat fungsi endpoint
Seperti yang telah dijelaskan pada bagian 2 sebelumnya, suatu proyek app engine terdiri dari beberapa file, pada bagian sebelumnya hello world hanya terdapat 2 buah fungsi yaitu *generic greeting* dan *greet by name*, tampilan muka dari proyek hello world tersebut adalah seperti gambar dibawah ini :

![img1](https://lh5.googleusercontent.com/-26y0J4wD4jY/VJlhTnwWhOI/AAAAAAAAAN8/kcbTRF3y6tA/w479-h352-no/hello%2Bworld.png)

pada bagian ini kita akan menambahkan suatu fungsi *greet by periods* yaitu fungsi yang menyapa pengguna berdasarkan waktu yang dipilih oleh pengguna. tampilan muka dari proyek hello world + periods adalah seperti gambar dibawah ini :

![img2](https://lh6.googleusercontent.com/-3H1DoRBIUiY/VJlhTsiijUI/AAAAAAAAAN8/bZfYCS6QUI0/w568-h228-no/hello%2B%2B%2B.png)

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
Berikut adalah tampilan tatap muka ketika tombol yang berisi fungsi yang kita buat ditekan.

![img3](https://lh3.googleusercontent.com/-2Vq1_SEMIFY/VJlhTrZduWI/AAAAAAAAAN8/_4uzyw-6VXg/w770-h581-no/hello%2B%2B%2B%2B.png)


#### Cara menggunakan API explorer

Kita dapat mengecek fungsi endpoint yang kita buat dengan menjalankan applikasi yang kita buat dan membukanya dengan halaman tatap muka, namun kita juga dapat mengecek secara langsung tanpa halaman tatap muka fungsi endpoint yang kita buat dengan menggunakan API explorer. Dibawah ini dapat dilihat proyek hello world mempunyai 3 fungsi endpoints yaitu : greetByPeriods, sayHello dan sayHelloByName

![img4](https://lh5.googleusercontent.com/-vKwAHRfiHqY/VJlhTiX5d6I/AAAAAAAAAN8/d8MHjjwZApI/w749-h470-no/API%2Bexplorer.png)

Kita bisa mencoba fungsi yang baru kita buat yaitu greetByPeriods menggunakan Api explorer, fungsi greetByPeriods membutuhkan 2 parameter yaitu name dan timeofday

![img5](https://lh4.googleusercontent.com/-axUBtStwUCE/VJlhTr3apvI/AAAAAAAAAN8/-PzJXyGLfm0/w979-h527-no/API%2Bexplorer%2B1.png)

Setelah kita mengisi parameter yang dibutuhkan dan mengexecute fungsi endpoint greetByPeriods maka dapat dilihat hasil dari fungsi endpoint greetByPeriods yaitu

![img6](https://lh6.googleusercontent.com/-atWb5--wkIQ/VJlhTtpFUyI/AAAAAAAAAN8/yBVq2mH3E_I/w979-h533-no/API%2Bexplorer%2B2.png)


#### Pengenalan tentang conference central

Bagian terakhir dari modul 2 adalah pengenalan tentang proyek conference central yang akan digunakan sebagai contoh dan target akhir dari pembelajaran seluruh modul.

Conference central adalah suatu website yang digunakan untuk mengatur kegiatan pertemuan, conference central memiliki beberapa menu utama yaitu:
* User dapat login mengggunakan account google
![img7](https://lh4.googleusercontent.com/-oK8cEvVzcVQ/VJlhToqWxtI/AAAAAAAAAN8/d3jnbHE0-5Q/w835-h470-no/conference%2Bcentral%2B1.png)
* User dapat melihat pertemuan yang sudah dibuat
![img8](https://lh3.googleusercontent.com/-Hl8-hsMS6mQ/VJlhTiaukHI/AAAAAAAAAN8/aisSST0Nd6M/w979-h517-no/conference%2Bcentral%2B10.png)
* User dapat membuat pertemuan baru
![img9](https://lh4.googleusercontent.com/-PDf01BSy70w/VJlhTmUm6RI/AAAAAAAAAN8/fbTTjPievLo/w979-h497-no/conference%2Bcentral%2B11.png)
* User dapat mengupdate profilenya.
![img10](https://lh6.googleusercontent.com/-Ovh0MIPH6kc/VJlhTv0NmuI/AAAAAAAAAN8/PMizb1q5a6A/w979-h461-no/conference%2Bcentral%2B12.png)


Modul selanjutnya akan membahas cara pembuatan conference central menggunakan google app engine. anda dapat melihat conference central versi lengkap yang sudah dibuat oleh udacity pada halaman berikut :

[conference central udacity at appspot.com](https://conference-central-dot-udacity-extras.appspot.com/)




