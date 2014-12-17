# Lesson 6 - App Engine and Mobile Apps

_Oleh Reza Jatnika - 23212339_

Bagian ini memaparkan tentang penggunaan Conference App server (App Engine backend)
untuk mobile client (aplikasi Android pada course ini) dengan memanfaatkan
Google Cloud Endpoints.

## Google Cloud Endpoints
Google Cloud Endpoints merupakan kumpulan _tools_ dan _libraries_ yang dapat
membuat API dan client libraries dari aplikasi App Engine, biasanya disebut API backend.
Endpoints dapat dengan mudah membuat web backend untuk aplikasi mobile seperti Android
atau Apple iOS.

Untuk para mobile developer, Endpoints menyediakan cara sederhana untuk mengembangkan
web backend dan infrastuktur penting seperti OAuth 2.0 authentication. Karena API backend
adalah aplikasi App Engine, developer dapat menggunakan semua service yang ada di App Engine
seperti, Datastore, Cloud Storage, Mail, Task Queues, dan lainnya. Serta dengan menggunakan
App Engine, developer terbebas dari pekerjaan system admin seperti load balancing dan server
maintenance.

Perbedaan yang mendasar dari penggunaan Endpoints dengan protokol HTTP biasa adalah client
libraries dari Endpoint dapat langsung memanggil API.

### Basic Endpoints Architecture
Berikut ilustrasi dari arsitektur penggunaan Endpoints.
![Google Cloud Endpoints](https://cloud.google.com/appengine/docs/images/endpoints.png)

API backend merupakan aplikasi App Engine yang menjalankan business logic dan fungsi lainnya
untuk Android dan iOS client serta untuk JavaScript web client. Fitur-fitur backend ini dapat
dipergunakan oleh client dengan adanya Endpoints.

### Development Proccess
Alur dari pengembangan aplikasi dengan menggunakan Endpoint:

1. Membuat API backend project (menggunakan Maven).
2. Annotate kode API backend yang akan digunakan.
3. Membuat client library dengan menggunakan Maven atau Endpoints command line tool.

## Connecting the Conference Android App to App Engine Backend
Bagian ini akan membahas langkah-langkah penggunaan API backend pada aplikasi Conference Android.

### Create a Signing Key for Android
Aplikasi yang akan di-_install_ pada Android harus di-_signed_ dengan certificate digital,
namun certificate tidak perlu di-_signed_ oleh certificate authority.

Saat aplikasi Android di-_build_ pada debug mode, SDK akan secara otomatis membuat key dan menandai
(signing apps) aplikasi tersebut.

- Lokasi keystore: `$HOME/.android/debug.keystore`
- Keystore dan password keystore: `android`
- Key alias: `androiddebugkey`

Untuk release mode, dibutuhkan key baru. Untuk membuatnya jalankan command dibawah ini:

    $ keytool -genkey -v -keystore my-release-key.keystore -alias \
      <alias_name> -keyalg RSA -keysize 2048 -validity 10000

Ikuti langkah-langkah selanjutnya untuk mengisi informasi dari key tersebut.

### Client IDs in the Google Developers Console
Untuk menggunakan API dan authentication system pada app backend oleh Android client, dibutuhkan
Android Client ID pada developers console. Client ID ini berisi certificate fingerprint (SHA1)
yang diisi sesuai SHA1 dari key yang dibuat.

Untuk melihat SHA1 dari key:

    $ keytool -exportcert -alias <alias_name> -keystore $HOME/.android/my-release-key.keystore -list -v | grep SHA1

Contoh output dari command ini:

    SHA1: BB:75:F9:CB:23:D9:9A:5A:B4:B8:12:36:F0:8E:86:2A:24:C8:6D:2E

Buka Google Developers Console, kemudian buat Client ID yang baru seperti gambar di bawah ini.

![Android Client ID](https://lh5.googleusercontent.com/a2qPz8keywECShaBanqrQ2WhGNl35S4fujPJ_hlQrH9k0eZXJ4gJG-Y2nK3RQr1qLdNSywR15XK-I3MD9SiO6yvM01WqE6VHlbDemjT_iuhYrYV8U9PYgJ93aeLsfOsKQQ)

Isikan field sesuai dengan data yang dibutuhkan. Kemudian pada source code aplikasi App Engine, buka
`src/main/java/com/google/devrel/training/conference/Constants.java` dan ganti konstanta `ANDROID_CLIENT_ID` sesuai dengan yang telah dibuat.

### Generating a Client Library for Android Using Maven
Untuk mendapatkan client library jalankan command dibawah ini pada directory yang terdapat `pom.xml`.

    $ mvn appengine:endpoints_get_client_lib

Navigasi ke library client source code yang telah dibuat di `/target/endpoints-client-libs/conference-central`, jalankan command:

    $ mvn install

Library yang dibuat dalam bentuk JAR terdapat di `/target/<yourProjectVersion>.<versionString>-rc-SNAPSHOT.jar`.

### Android Application Source Code
Download [source code untuk aplikasi Android](https://github.com/udacity/conference-central-android-app/archive/master.zip) ini atau dengan menggunakan git.

    $ git clone git@github.com:udacity/conference-central-android-app.git
