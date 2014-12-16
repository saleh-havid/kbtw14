# Bagian 3
Elvanno Hatorangan  / 23213051 -- EL6240 Komputasi Bergerak dan Teknologi Web

### Lesson 3: Storing an Retrieving Data
Bagian Kedua - Video :24-39
## *Key Conflict*
#### Apa itu Key Conflict?

Seperti yang telah kita bahas sebelumnya, ketika kita akan menetapkan sebuah *key* pada Datastore ada 2 cara yang dapat kita gunakan yaitu:

1. Otomatis : Datastore secara otomatis akan menetapkan *key* untuk entitas kita.
2. Manual   : Menentukan sendiri nama ID didalam entitas kita yang akan digunakan untuk membangkitkan *key* pada Datastore.

Apabila kita membiarkan datastore menetapkan *key* secara otomatis, maka datastore akan memastikan bahwa *key* yang dihasilkan pasti berbeda antara satu entitas dengan yang lain.
seperti yang terlihat pada ilustrasi berikut.

![](https://lh6.googleusercontent.com/-f9D5bWsJIxQ/VI68mTxQSUI/AAAAAAAAEgE/-b7iNziMEYY/w1008-h583-no/Key%2BConflict_1.png)

Namun apabila kita memilih menetapkan *key* dengan cara manual, maka kita harus memperhatikan bahwa nama ID harus bersifat unik dan berbeda dengan nama ID di entitas lainnya. Hal ini perlu dilakukan karena apabila kita menggunakan dua ID yang sama maka akan terjadi *key conflict*. ilustrasi berikut menggambarkan terjadinya *key conflict* yang disebabkan karena nama ID yang sama.

![](https://lh3.googleusercontent.com/-tvpYkt5kdJ0/VI68l3aKn9I/AAAAAAAAEf8/xjUxNMpW6ns/w1044-h571-no/Key%2BConflict_2.png)

Pada aplikasi "conference central" yang kita buat, kita akan menggunaan userID google sebagai namaID untuk membangkitkan *key* di Datastore. keunikan UserID dapat dijamin oleh sistem autentikasi google sehingga *key conflict* dapat dihindari.

### Bagaimana menghindari Key Conflict?
*Key Conflict* umumnya terjadi ketika kita akan melakukan pembaruan data atau memasukan data yang baru. Pada contoh aplikasi yang kita buat untuk mengindari terjadinya *conflict*, maka langkah yang harus dilakukan adalah:
1. Mengambil entitas dari Datastore
2. Lakukan pemabaruan pada entitas tersebut
3. Simpan kembali entitas tersebut pada Datastore

Kode dibawah ini menunjukan bagaimana kita menghindari *key conflict* ketika melakukan pembaruan pada data didalam Datastore. Potongan kode berikut berasal dari method "saveProfile" didalam file "ConferenceApi.java"
```
  public Profile saveProfile(final User user, ProfileForm profileForm) throws UnauthorizedException {

        if (user == null){
    	 throw new UnauthorizedException("Authorization Required");
        	}

        String mainEmail = user.getEmail();
        String userId = user.getUserId();
        String displayName = profileForm.getDisplayName();
        TeeShirtSize teeShirtSize = profileForm.getTeeShirtSize();

     	Profile profile = ofy().load().key(Key.create(Profile.class, userId)).now();

     	if (profile == null){
     		if (displayName == null){
     			displayName = extractDefaultDisplayNameFromEmail(user.getEmail());
     		}
     		if (teeShirtSize == null){
     			teeShirtSize = TeeShirtSize.NOT_SPECIFIED;
     		}
     		profile = new Profile(userId, displayName, mainEmail, teeShirtSize);
     	}else{
     		profile.update(displayName, teeShirtSize);
     	}
        ofy().save().entity(profile).now();

        return profile;
    }
```
Mari kita bahas barisan kode diatas !
```
 if (user == null){
    	 throw new UnauthorizedException("Authorization Required");
        	}
```

Pertama ketika aplikasi memanggil method saveProfile, maka method akan memeriksa apakah user sudah *Log in*, apabila belum maka akan muncul *exception* yang memperingatkan user bahwa autorisasi pengguna dibutuhkan
```
    String mainEmail = user.getEmail();
    String userId = user.getUserId();
    String displayName = profileForm.getDisplayName();
    TeeShirtSize teeShirtSize = profileForm.getTeeShirtSize();
```
Setelah user berhasil diautorisasi, maka method akan mengambil data email, userID, displayname dan Teeshirtsize
```
Profile profile = ofy().load().key(Key.create(Profile.class, userId)).now();
```
Untuk memanggil entitas dari Datastore, maka kita menggunakan lagi bantuan library class "objectify()". "ofy()" berfungsi untuk memangil layanan objectify; load() berfungsi untuk mengambil entitas; "key" berfungsi mengambil key sesuai dengan ID yang dipakai
```
if (profile == null){
     		if (displayName == null){
     			displayName = extractDefaultDisplayNameFromEmail(user.getEmail());
     		}
     		if (teeShirtSize == null){
     			teeShirtSize = TeeShirtSize.NOT_SPECIFIED;
     		}
     		profile = new Profile(userId, displayName, mainEmail, teeShirtSize);
     	}else{
     		profile.update(displayName, teeShirtSize);
     	}
        ofy().save().entity(profile).now();

        return profile;
    }
```
Setelah entitas "profile" diambil dari Datastore, maka method "saveProfile" akan melakukan pemeriksaan terhadap entitas tersebut. apabila entitas tersebut kosong, maka akan dibuat profile baru, apabila sudah ada profile sebelumnya, maka akan dilakukan pembaruan terhadap entitas tersebut. setelah dilakukan pembaruan atau penambahan profile baru, maka kita dapat menyimpan kembali entitas tersebut kedalam Datastore.

Untuk melakukan pembaruan, maka kita harus menambahkan method "update" pada class "Profile.java" seperti kode dibawah ini.
```
public void update(String displayName, TeeShirtSize teeShirtSize){
		if (displayName != null){
			this.displayName = displayName;
		}
		if (teeShirtSize != null){
			this.teeShirtSize = teeShirtSize;
		}
	}
```
Kode diatas berarti mengembalikan nilai "displayName" dan "teeShirtSize" yang ditransfer dari class yang memanggil method "update".

Setelah kita melakuan pembaruan pada kode di file "ConferenceApi.java" dan "Profile.java" jangan lupa untuk mengunggah project "Conference Central" yang telah kita modifikasi ke account AppEngine kita. Cara mengunggah bisa kita lihat pada Bagian 2.


# *Web UI*
Sampai sejauh ini kita sudah bisa menggunakan google AppEngine untuk melakukan save, load dan update didalam Datastore. Berbagai fungsi yang telah kita buat sebenarnya merupakan bagian dari sebuah aplikasi yang lebih besar yaitu *Conference Central*. Gambar berikut menunjukan tampilan halaman depan dari aplikasi *Conference Central*.

![](https://lh4.googleusercontent.com/IzbBI-NOp6UGxYjl3D1udX3EyjxCLh-J8bmT6tVyCxE=w1044-h546-no)

Aplikasi *Conference Central* terdiri dari dua bagian, yang pertama adalah *Front End* API dan yang kedua adalah *Back End * API. *Front end* API merupakan aplikasi yang berhubungan dengan *user interface*, pada *Conference Central* kita menggunakan halaman web sebagai *Front End* API. Fungsi-fungsi yang telah kita buat sejauh ini merupakan bagian dari* Back End* API. *Front End* dan *Back End* API tidak harus berada pada satu aplikasi yang sama. *Back End* API dapat digunakan oleh lebih dari satu *Front End* API dari aplikasi yang berbeda.

![](https://lh6.googleusercontent.com/mcv2lkLfR_yu6db2qlKbLPmHzZn-ufWVZwHx1QEk4SY=w675-h528-no)

Setiap *Front End* API harus memiliki izin untuk menggunakan *Back End* API. Ada beberapa hal yang harus kita lakukan agar *Front End* API dapat menggunakan *Back End* API. Pertama kita harus memberi tahu *Back End* API mengenai aplikasi client mana yang akan menggunakannya.
```
@Api(name = "conference", version = "v1", scopes = { Constants.EMAIL_SCOPE }, clientIds = {
        Constants.WEB_CLIENT_ID, Constants.API_EXPLORER_CLIENT_ID }, description = "API for the Conference Central Backend application.")
```

Pada aplikasi *Conference Central* dapat kita lihat di "ConferenceAPI.java", diawal file tersebut kita  mendeklarasikan API endpoints dengan parameter sebagai berikut:
```
Constants.WEB_CLIENT_ID
Constants.API_EXPLORER_CLIENT_ID
```
Parameter yang pertama mendeklarasikan client ID untuk web application, sedangkan parameter yang kedua mendeklarasikan client ID untuk API Explorer. Semua client ID tersebut di simpan pada class "Constants.java" seperti berikut ini:
```
public class Constants {
    public static final String WEB_CLIENT_ID = "645006325075-f0ivm9iknc3obaji6nq72cfqlkq7a31u.apps.googleusercontent.com";
    public static final String ANDROID_CLIENT_ID = "replace this with your Android client ID";
    public static final String IOS_CLIENT_ID = "replace this with your iOS client ID";
    public static final String ANDROID_AUDIENCE = WEB_CLIENT_ID;
    public static final String EMAIL_SCOPE = Constant.API_EMAIL_SCOPE;
    public static final String API_EXPLORER_CLIENT_ID = Constant.API_EXPLORER_CLIENT_ID;
    public static final String MEMCACHE_ANNOUNCEMENTS_KEY = "RECENT_ANNOUNCEMENTS";
}
```
client ID tersebut didapatkan dari http://console.developers.google.com seperti tampilan berikut:

![](https://lh5.googleusercontent.com/-MDOW0XDnNVs/VI68m12Q4MI/AAAAAAAAEgM/UTOtoYIDPaA/w706-h362-no/WebUI_3.png)

Untuk mendapakan client ID, terlebih dahulu kita harus membuatnya dengan memasukan alamat web yang akan menggunakan client ID tersebut. seperti gambar berikut:

![](https://lh4.googleusercontent.com/-3QJRwFQVYaI/VI7K8_CFy0I/AAAAAAAAEhE/bMFxqZ0GOFo/w477-h583-no/webUI_6.png)

setelah memperoleh client ID, maka kita tinggal menyalinnya pada file "Constants.java" di baris "WEB_CLIENT_ID".

Aplikasi *Conference Central* menggunakan java script untuk mengaktifkan proses perizinan ketika user log in, maka kita perlu menambahkan Client ID yang telah kita buat kedalam file java script, seperti kode berikut.
```
app.factory('oauth2Provider', function ($modal) {
    var oauth2Provider = {
        CLIENT_ID: '645006325075-f0ivm9iknc3obaji6nq72cfqlkq7a31u.apps.googleusercontent.com',
        SCOPES: 'https://www.googleapis.com/auth/userinfo.email profile',
        signedIn: false
    };
```

Setelah kita menambahkan client ID, maka kita bisa mencoba aplikasi yang telah kita buat seperti gambar berikut

![](https://lh5.googleusercontent.com/co2Ra0YTOaH6ShGXLpBRHvBECmmZowfxoxd_QEg5w5k=w1044-h361-no)

Kita bisa memeriksa validasi data yang telah kita perbarui melalui  http://console.developers.google.com seperti gambar berikut

![](https://lh3.googleusercontent.com/-Uo1qTuCRtVo/VI68l5iUC5I/AAAAAAAAEf4/8zM86juwVRU/w1044-h377-no/WEubUI_4.png)



