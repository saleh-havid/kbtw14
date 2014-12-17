#*Lesson 4 - Advanced Datastore Concepts*
-----
**Tugas Mata Kuliah EL6240 - Komputasi Bergerak dan Teknologi Web**
*oleh Wervyan Shalannanda - 23213145*


##***Consistency Models and Transactions in Datastore***

[TOC]

###***Datastore Commit Process***
*Datastore commit process* mendeskripsikan tentang konsistensi aturan-aturan untuk menyimpan data. *Datastore* memiliki dua jenis konsistensi model, yaitu:

 1. *Eventual Consistency*; dan
 2. *Strong Consistency*.

Perhatikan diagram *use case* berikut.
![](https://lh5.googleusercontent.com/-6SFFE__bdgE/VI8EbX6wdhI/AAAAAAAAAMM/4I5Jte_uhR4/s0/1+-+Datastore+Commit+Process.jpg "1 - Datastore Commit Process.jpg")
*Gambar 1. Entitas pada datastore commit process*

Dari diagram *use case* di atas, dapat terlihat tiga elemen, yaitu: aplikasi, *datastore API*, dan *datastore backend*. Ketika akan menyimpan data/menuliskan *entity* ke dalam *storage*, aplikasi akan melakukan suatu operasi kepada *datastore API* kemudian *datastore backend* menuliskan data tersebut ke suatu *log*. Perbedaan antara *eventual consistency* dan *strong consistency* terdapat pada prosedur *datastore backend* menuliskan data yang disimpan oleh aplikasi ke dalam *datastore*. Ilustrasi kedua konsistensi ini dapat dilihat pada bagan berikut.

![](https://lh3.googleusercontent.com/-yU4rjFGpY3I/VI8KbCXX3PI/AAAAAAAAANQ/sEzsHKo3DYM/s0/2+-+Eventual+Consistency.jpg "2 - Eventual Consistency.jpg")
*Gambar 2. Eventual Consistency*
![^gambar3](https://lh4.googleusercontent.com/-8e9g-jH6epQ/VI8IJ_0NDRI/AAAAAAAAAM4/Hy_D9QA4dSE/s0/3+-+Strong+Consistency.jpg "3 - Strong Consistency.jpg")
*Gambar 3. Strong Consistency*

Berdasarkan gambar 2, dapat terlihat bahwa setelah *datastore API* telah selesai menuliskan *log*, kontrol langsung dikembalikan ke aplikasi. Dengan demikian, *eventual consistency* memiliki keunggulan berupa *latency* yang rendah. Padahal, *entity* dari aplikasi belum selesai dituliskan oleh *datastore backend* ke dalam penyimpanan (data belum selesai disimpan). Pada tahap ini, *datastore backend* tengah bekerja untuk membuat semuanya konsisten dengan menggunakan informasi *login* untuk memperbarui *entity storage* serta memperbarui semua indeksnya. 

Perbedaan yang jelas antara kedua konsistensi tersebut dapat terlihat ketika aplikasi me-*request* *query* kepada *datastore API*. Apabila ada *query* sebelum *datastore backend* selesai melakukan pembaruan indeks, maka *datastore backend* akan mengembalikan *query* seperti sebelum aplikasi melakukan operasi pada *datastore API*.Sedangkan untuk *strong consistency*, ketika *datastore API* menerima *query*, *datastore API* akan melihat bahwa terjadi *pending updates* atas data yang di-*query*. *Query* akan dibiarkan menunggu hingga *datastore backend* telah selesai melakukan operasinya, yaitu menulis entitas dan memperbarui indeks. Dengan demikian, akan diperoleh *query* terbaru.

>Manakah *consistency* yang lebih baik?

Perhatikan dua kasus berikut.

 1. Komentar Blog
- tidak membutuhkan *immediate update*
- tidak ada *operation dependency* - tidak ada operasi lanjutan yang mengharuskan data *up to date*
--> lebih baik menggunakan *eventual consistency*
 2. Pengambilan uang di ATM
- saldo akhir harus langsung diperbarui
- terdapat *operation dependency* - misalnya ada pembayaran tagihan atas akun tersebut yang membutuhkan saldo terkini.
--> harus menggunakan *strong consistency*


>Apakah harus selalu menggunakan *strong consistency?*

Tidak. *Strong consistency* membuat sistem banyak menunggu dan mengakibatkan aplikasi memiliki *latency* cukup lama. Untuk membangun aplikasi yang *scalable*, sebaiknya *programmer* menggunakan *eventual consistency* sesering mungkin.

Datastore mendukung kedua model ini. *Eventual consistency* yang *scalable*dapat digunakan sebagai model *default*. Kemudian, apabila dibutuhkan, *strong consistency* dapat digunakan untuk memastikan integritas data pada sistem.

>Bagaimana cara memilih antara kedua model tersebut?

*Strong consistency* digunakan ketika terdapat dua kondisi, yaitu: terdapat *ancestor relationship*, dan *query* menggunakan *filter* yang berasal dari *ancestor* ini. Selain itu, dapat digunakan *eventual consistency*.


###***Transactions***
Sebelum membahas tentang transaksi, sebaiknya kita kembali sejenak ke bagian *data relationship*
![enter image description here](https://lh6.googleusercontent.com/-i48xLoC2q4c/VI8YiYsVvfI/AAAAAAAAANo/9zH8uHEPLXI/s0/4+-+Data+Relationship.jpg "4 - Data Relationship.jpg")
*Gambar 4 - Data Relationships*

Pada bagian sebelumnya, telah dijelaskan tentang *ancestor relationship* antara suatu profil dengan konferensi, yaitu profil yang membuat konferensi adalah *ancestor* atas konferensi tersebut. Pada bagian ini, terdapat *has-a relationship* antara profil dengan konferensi, di mana setiap entitas profil memiliki daftar konferensi yang akan dihadirinya.Properti ini disebut dengan "*conferences to attend*". Sebagai tambahan, diberikan penghitung numerik untuk setiap entitas konferensi yang menyatakan jumlah kursi yang tersedia. Properti ini disebut "*seats available*" yang nilainya akan berkurang apabila seorang pengguna mendaftar untuk menghadiri konferensi. Berdasarkan uraian tersebut, terdapat dua operasi pembaruan atas dua entitas berbeda yang saling dependen, yaitu: nama konferensi tersebut akan ditambahkan ke "*conferences to attend*"; dan jumlah kursi yang tersedia pada "*seats available*" akan berkurang satu. 
Sampai pada bagian ini, setiap operasi pembaruan *datastore* yang digunakan masih independen satu sama lain (lihat Gambar 5). Selanjutnya, akan dilakukan operasi yang saling dependen pada properti "*conferences to attend*" dan "*seats available*" (lihat Gambar 6).

![enter image description here](https://lh6.googleusercontent.com/-tmhgEw-WPdY/VI8gDzufPFI/AAAAAAAAAOA/yfu4AprbBws/s0/5+-+Transaction+1.jpg "5 - Transaction 1.jpg")
*Gambar 5. Operasi yang saling independen*

![enter image description here](https://lh4.googleusercontent.com/-00H_Pz4314Y/VI8jdAIUODI/AAAAAAAAAOY/JY1DOQ60nXs/s0/6+-+Transaction+2.jpg "6 - Transaction 2.jpg")
*Gambar 6. Operasi yang saling dependen*

Diperlukan metode khusus untuk menghindari hal yang tidak diinginkan ketika aplikasi melakukan operasi. Salah satu kasus yang mungkin terjadi adalah aplikasi berhasil melakukan pembaruan pada properti *add conference to attend* namun karena suatu hal, sistem mengalami *failure* sebelum nilai *seats available* dikurangi (lihat Gambar 7). Sistem mengalami masalah pada konsistensi data, yaitu salah satu profil berhasil mendaftar konferensi, namun jumlah kursi tersisa tidak dikurangi sehingga dapat menyebabkan jumlah pendaftar melebihi kursi yang tersedia.

![enter image description here](https://lh4.googleusercontent.com/-gB6odURtrY0/VI8lys_6QfI/AAAAAAAAAOw/O3KgcMMk4QY/s0/7+-+Transaction+3.jpg "7 - Transaction 3.jpg")
*Gambar 7. Salah satu contoh kegagalan sistem*

Kesalahan tersebut dapat diantisipasi dengan menggunakan*transactions*, yaitu dengan memberitahu *data store* untuk memulai transaksi, kemudian dilakukan operasi-operasi yang saling dependen dan akhiri transaksi setelah semua operasi selesai (lihat Gambar 8). Apabila transaksi berhasil, maka semua pembaruan juga telah selesai. Namun, apabila transaksi gagal, maka semua pembaruan akan dihapus seolah-olah tidak terjadi pembaruan sama sekali (lihat Gambar 9). Di dalam bahasa *transactions*, metode ini dikenal dengan nama *commit forward*.  Dengan demikian, dapat dinyatakan bahwa semua operasi di dalam *transactions* akan dieksekusi sebagai sebuah grup yang lengkap atau tidak sama sekali.

![enter image description here](https://lh4.googleusercontent.com/-zlC3EPhWFVw/VI8orV0DmRI/AAAAAAAAAPI/TeqNAx6Ep-A/s0/8+-+Transaction+4.jpg "8 - Transaction 4.jpg")
*Gambar 8. Commit Forward Transactions ketika semua operasi berhasil*

![enter image description here](https://lh4.googleusercontent.com/-LBrBiW4bu-c/VI8pNFq8OfI/AAAAAAAAAPc/cq1UtUcU_ig/s0/9+-+Transaction+5.jpg "9 - Transaction 5.jpg")
*Gambar 9. Commit Forward Transactions ketika sebagian operasi gagal*

Berikut beberapa *transaction rules*

 1. Snapshot Isolation: konsistensi pembacaan data
- semua operasi pembacaan pada transaksi akan mengembalikan nilai yang dimiliki oleh *datastore* ketika transaksi dimulai.
- pembaruan tidak akan ditampilkan selama transaksi, sehingga pembaruan hanya akan terjadi apabila seluruh operasi dalam transaksi berhasil atau pembaruan tidak terjadi sama sekali apabila ada operasi yang gagal sehingga membatalkan transaksi.
 2. Optimistic Concurrency: mengatur pembaruan *concurrent* (terjadi bersamaan)
- transaksi akan sukses jika nilai yang diperbarui oleh transaksi ini tidak berubah sejak awal transaksi. Jika ada nilai yang berubah sejak dimulainya transaksi, maka transaksi akan dibatalkan.

Selain aturan di atas, aturan di atas, sistem dapat mengimplementasikan aturan lain, seperti:
- Satu transaksi hanya dapat mengubah maksimum 5 grup *ancestor* (atau grup entitas);
- Satu transaksi harus selesai dalam 60 detik;
- Buat transaksi sesingkat mungkin, dll.


###**Latihan Pemrograman**

Salah satu fungsi yang belum diisi pada aplikasi *Conference Central* adalah registrasi untuk menghadiri konferensi. Proses registrasi untuk suatu konferensi melibatkan beberapa aktivitas terpisah, termasuk melakukan *update* profil pengguna, mengetahui konferensi yang telah didaftarkan, dan mengurangi jumlah kursi yang tersedia pada suatu konferensi. Selain itu, dibuat juga aplikasi untuk menentukan *user* yang mendapatkan kursi apabila hanya tersisa satu kursi dan ada beberapa *user* yang mendaftar dengan menggunakan *transaction*. Berikut ini adalah struktur umum dari *transactions*.
    
    // Transactions
	<T> result = ofy().transact(new Work<t>(){
		public <t> run (){
			//do stuff
			//do more stuff
			return <T>;
		}
	};
    
Pada pemrograman ini akan digunakan properti "*conferences to attend*" untuk mengetahui konferenesi mana saja yang akan dihadiri oleh pengguna. Properti ini juga akan digunakan di *profile entity* yang ada di *class* profil pada *Profile.java*. Berikut *script*-nya.
####***Profile.java***
> pada bagian bertanda <i class="icon-pencil"></i>
     
    @Id String userId;
    
<i class="icon-pencil"></i>

    /**
     * Public constructor for Profile.
     * @param userId The user id, obtained from the email
     * @param displayName Any string user wants us to display him/her on this system.
     * @param mainEmail User's main e-mail address.
     * @param teeShirtSize The User's tee shirt size
     *
     */
    public Profile (String userId, String displayName, String mainEmail, TeeShirtSize teeShirtSize) {
        this.userId = userId;
        this.displayName = displayName;
        this.mainEmail = mainEmail;
        this.teeShirtSize = teeShirtSize;
    }

dengan menggunakan *script berikut*

        /**
     * Keys of the conferences that this user registers to attend.
     */
    private List<String> conferenceKeysToAttend = new ArrayList<>(0);

dan mengimport List

    import java.util.List;

####***ConferenceApi.java***
Selanjutnya, pada "*ConferenceApi.java*", tambahkan fungsi *create conference*  pada *class* *Conference* dan fungsi registerForConference pada *class* *WrappedBoolean*, tambahkan *script* berikut.

<i class="icon-trash"></i> ***Conference***

     @ApiMethod(name = "createConference", path = "conference", httpMethod = HttpMethod.POST)
    public Conference createConference(final User user, final ConferenceForm conferenceForm)
        throws UnauthorizedException {
        if (user == null) {
            throw new UnauthorizedException("Authorization required");
        }

        // TODO (Lesson 4)
        // Get the userId of the logged in User
        String userId = user.getUserId();

        // TODO (Lesson 4)
        // Get the key for the User's Profile
        Key<Profile> profileKey = Key.create(Profile.class, userId);

        // TODO (Lesson 4)
        // Allocate a key for the conference -- let App Engine allocate the ID
        // Don't forget to include the parent Profile in the allocated ID
        final Key<Conference> conferenceKey = factory().allocateId(profileKey, Conference.class);

        // TODO (Lesson 4)
        // Get the Conference Id from the Key
        final long conferenceId = conferenceKey.getId();

        // TODO (Lesson 4)
        // Get the existing Profile entity for the current user if there is one
        // Otherwise create a new Profile entity with default values
        Profile profile = getProfileFromUser(user);

        // TODO (Lesson 4)
        // Create a new Conference Entity, specifying the user's Profile entity
        // as the parent of the conference
        Conference conference = new Conference(conferenceId, userId, conferenceForm);

        // TODO (Lesson 4)
        // Save Conference and Profile Entities
         ofy().save().entities(conference, profile).now();

         return conference;
         }

<i class="icon-trash"></i> ***WrappedBoolean***

    //Start transaction 
        WrappedBoolean result = ofy().transact(new Work<WrappedBoolean>() {
            @Override
            public WrappedBoolean run() {
                try {

                // Get the conference key
                // Will throw ForbiddenException if the key cannot be created
                Key<Conference> conferenceKey = Key.create(websafeConferenceKey);

                // Get the Conference entity from the datastore
                Conference conference = ofy().load().key(conferenceKey).now();

                // 404 when there is no Conference with the given conferenceId.
                if (conference == null) {
                    return new WrappedBoolean (false,
                            "No Conference found with key: "
                                    + websafeConferenceKey);
                }

                // Get the user's Profile entity
                Profile profile = getProfileFromUser(user);

                // Has the user already registered to attend this conference?
                if (profile.getConferenceKeysToAttend().contains(
                        websafeConferenceKey)) {
                    return new WrappedBoolean (false, "Already registered");
                } else if (conference.getSeatsAvailable() <= 0) {
                    return new WrappedBoolean (false, "No seats available");
                } else {
                    // All looks good, go ahead and book the seat
                    profile.addToConferenceKeysToAttend(websafeConferenceKey);
                    conference.bookSeats(1);

                    // Save the Conference and Profile entities
                    ofy().save().entities(profile, conference).now();
                    // We are booked!
                    return new WrappedBoolean(true, "Registration successful");
                }

                }
                catch (Exception e) {
                    return new WrappedBoolean(false, "Unknown exception");

                }
            }

Untuk lebih jelasnya, silakan akses link berikut.
[Ud859 Lesson 4 - Video 48 - Register for conference in a transaction](https://www.udacity.com/course/viewer#!/c-ud859/l-1219418587/m-1497718613)

Selanjutnya, tambahkan fungsi *Query for conference* dan *Get conference to attend* pada *class* "*Collection < Conference >*", *Unregistering from a Conference* pada *class* *Wrapped Boolean*
<i class="icon-trash"></i>***Collection < Conference >***
***Query for a conference***

     /** Code to add at the start of querying for conferences **/


    @ApiMethod(
            name = "queryConferences_nofilters",
            path = "queryConferences_nofilters",
            httpMethod = HttpMethod.POST
    )
    public List<Conference> queryConferences_nofilters() {
        // Find all entities of type Conference
        Query<Conference> query = ofy().load().type(Conference.class).order("name");

        return query.list();
    }

   

     /**
         * Queries against the datastore with the given filters and returns the result.
         *
         * Normally this kind of method is supposed to get invoked by a GET HTTP method,
         * but we do it with POST, in order to receive conferenceQueryForm Object via the POST body.
         *
         * @param conferenceQueryForm A form object representing the query.
         * @return A List of Conferences that match the query.
         */
        @ApiMethod(
                name = "queryConferences",
                path = "queryConferences",
                httpMethod = HttpMethod.POST
        )
        public List<Conference> queryConferences(ConferenceQueryForm conferenceQueryForm) {
            Iterable<Conference> conferenceIterable = conferenceQueryForm.getQuery();
            List<Conference> result = new ArrayList<>(0);
            List<Key<Profile>> organizersKeyList = new ArrayList<>(0);
            for (Conference conference : conferenceIterable) {
                organizersKeyList.add(Key.create(Profile.class, conference.getOrganizerUserId()));
                result.add(conference);
            }
            // To avoid separate datastore gets for each Conference, pre-fetch the Profiles.
            ofy().load().keys(organizersKeyList);
            return result;
        }
	
***Get Conference to Attend***
	
    public Collection<Conference> getConferencesToAttend(final User user)
            throws UnauthorizedException, NotFoundException {
        // If not signed in, throw a 401 error.
        if (user == null) {
            throw new UnauthorizedException("Authorization required");
        }
        // TODO
        // Get the Profile entity for the user
        Profile profile = ofy().load().key(Key.create(Profile.class, user.getUserId())).now();
        if (profile == null) {
            throw new NotFoundException("Profile doesn't exist.");
        }
        
        // TODO
        // Get the value of the Profile's conferenceKeysToAttend property
        List<String> keyStringsToAttend = profile.getConferenceKeysToAttend();
        List<Key<Conference>> keysToAttend = new ArrayList<>();
        for (String keyString : keyStringsToAttend) {
            keysToAttend.add(Key.<Conference>create(keyString));
        }
        
        // TODO
        // Iterate over keyStringsToAttend,
        // and return a Collection of the
        // Conference entities that the user has registered to attend
        
        return ofy().load().keys(keysToAttend).values();
    }

Untuk lebih jelasnya, silakan akses link berikut.
[Ud859 Lesson 4 - Video 53 - Conferences to Attend](https://www.udacity.com/course/viewer#!/c-ud859/l-1219418587/e-1588338535/m-1497718617)

<i class="icon-trash"></i>***WrappedBoolean***

    public WrappedBoolean unregisterFromConference(final User user,
                                            @Named("websafeConferenceKey")
                                            final String websafeConferenceKey)
            throws UnauthorizedException, NotFoundException, ForbiddenException, ConflictException {
        // If not signed in, throw a 401 error.
        if (user == null) {
            throw new UnauthorizedException("Authorization required");
        }

        WrappedBoolean result = ofy().transact(new Work<WrappedBoolean>() {
            @Override
            public WrappedBoolean run() {
                Key<Conference> conferenceKey = Key.create(websafeConferenceKey);
                Conference conference = ofy().load().key(conferenceKey).now();
                // 404 when there is no Conference with the given conferenceId.
                if (conference == null) {
                    return new  WrappedBoolean(false,
                            "No Conference found with key: " + websafeConferenceKey);
                }

                // Un-registering from the Conference.
                Profile profile = getProfileFromUser(user);
                if (profile.getConferenceKeysToAttend().contains(websafeConferenceKey)) {
                    profile.unregisterFromConference(websafeConferenceKey);
                    conference.giveBackSeats(1);
                    ofy().save().entities(profile, conference).now();
                    return new WrappedBoolean(true);
                } else {
                    return new WrappedBoolean(false, "You are not registered for this conference");
                }
            }
        });
        // if result is false
        if (!result.getResult()) {
            if (result.getReason().contains("No Conference found with key")) {
                throw new NotFoundException (result.getReason());
            }
            else {
                throw new ForbiddenException(result.getReason());
            }
        }
        // NotFoundException is actually thrown here.
        return new WrappedBoolean(result.getResult());
    }

Untuk lebih jelasnya, silakan akses link berikut.
[Ud859 Lesson 4 - Video 54 - Unregistering From a Conference](https://www.udacity.com/course/viewer#!/c-ud859/l-1219418587/m-1497718619)
