

LESSON 4 : Advanced Datastore Concepts Video
--------------------------------------------
**BAGIAN 1 : Video 1-20**

Dwina Fitriyandini Siswanto / 23213311
EL6240 Komputasi Bergerak dan Teknologi Web



####**Data Relationship**####
Dalam pemodelan data, terdapat dua jenis *data relationship* yaitu *ancestor relationship* dan *has-a relationship*. 

#####**1. Ancestor Relationship**#####
 *Ancestor relationship* merupakan hubungan antara suatu *entity* dengan *ancestor entity*. *Ancestor relationship* memiliki aturan, antara lain harus ditetapkan saat *entity* dibuat dan tidak dapat diubah. Perlu diketahui bahwa *ancestor relationship* tidak sama dengan *is-a relationship* pada *object oriented modeling*. Pada *conference central*, *ancestor relationship* mendefinisikan hubungan antara *conference* dengan *user* yang membuat *conference* tersebut, dengan *user* yang membuat *conference* sebagai *ancestor* dan *conference* sebagai *descendant*. Karena suatu *user* teridentifikasi melalui *profile entity*-nya maka *conference* yang dibuat oleh *user* akan memiliki *profile key* yang sama dengan *profile key* *user* yang membuatnya. 

#####**2. Has-a Relationship**#####
Pada *conference central*, *has-a relationship* digunakan untuk memodelkan hubungan antara *user* dengan *conference* yang dihadiri oleh *user* dengan menambahkan suatu *property* ke *profile entity* yang menspesifikasikan *attended conference*. Karena user dapat melakukan registrasi ke suatu *conference* dan dapat pula melakukan deregistrasi, maka dapat dilihat bahwa *has-a relationship* tidak terikat oleh aturan ketat seperti pada *ancestor relationship*.

####**Create Conferences**####
Dalam *Conference Central* terdapat fitur *Create Conference* yang digunakan untuk membuat *conference* baru. Gambar dibawah ini adalah tampilan laman *Create Conference* yang ada pada *Conference Central*.

![Create Conference](https://lh3.googleusercontent.com/LU_Cpqn3WCdY51HGdAE5wsj78CT_l2xKt5nK8wWCga8=s0 "Create Conference.jpg")

Langkah-langkah selanjutnya adalah langkah-langkah untuk menambahkan fungsionalitas pada fitur *Create Conference* ini.

####**Registering an Entity Class**####
Pada *OfyService class* yang berada pada OfyService.java, kita akan melakukan registrasi *Entity class*. Registrasi *Entity class* dilakukan saat kita menggunakan *Objectify* untuk mendefinisikan *entity class*. Registrasi dilakukan dengan *static method*.

    // Entity Registration
    static {
	    factory().register(Profile.class);
	    factory().register(Conference.class);
    }

####**Code for Creating Conferences**####
Untuk mengaktifkan fungsionalitas *create conferences* ke *conference central application*, berikut adalah langkah-langkahnya

1. *Copy* Conference.java ke domain package
2. *Copy* ConferenceForm.java ke form package
3. Tambahkan createConferences() ke conference API *class*
4. *Refresh project*

ConferenceForm.java memiliki beberapa *property* yang dipetakan ke *property* pada Conference.java yaitu *name, description, topics, city, startDate, endDate dan maxAttendees*. 

####**Save Conferences**####
Perlu diperhatikan bahwa membuat *conference entity* berbeda dengan membuat *profile entity*. Untuk *profile entity* kita menggunakan suatu *user* ID yang unik sebagai ID untuk *identity key*. Namun untuk *conference*, kita tidak perlu mengetahui *key* sehingga kita dapat menggunakan *auto allocate* yang dilakukan oleh *App Engine*. Berikut ini adalah kode agar *App Engine* melakukan auto allocate key

    // Let App Engine allocate the key
    Key <T> key = factory().allocateId(Entity.Class)
    
    // Let App Engine allocate the key to an entity that has a parent
    Key <T> key = factory().allocateId(parentKey,Entity.Class);
    
    // Save an entity by calling ofy.save
    ofy().save().entities(entity1, entity2, ...).now();

Langkah berikutnya adalah melakukan modifikasi dari createConferences() pada conference API *class* dengan langkah-langkah sebagai berikut,

 1. *Get the userID of the logged in User*
 2. *Get the key for the User's Profile*
 3. *Allocate a key for the conference*
 4. *Get the Conference Id from the Key*
 5. *Get the existing Profile entity for the current user if there is one, otherwise create a new Profile entity with default values*
 6. *Create a new Conference Entity, specifying the user's Profile entity as the parent of the conference*
 7. *Save Conference and Profile Entities*
 8. *Return the profile*
 
Setelah melakukan modifikasi dari createConferences() pada conference API *class*, jalankan di *localhost*, kemudian buat satu atau dua *conference* di API's explorer atau laman *create conference* di WebUI. Terakhir cek *conference entities* di *Datastore Viewer* pada *admin console*.


####**Types of Query**####
Dalam *conference central*, terdapat 3 macam *query* yang dapat dilihat pada fitur *show conferences*. Ketiga macam *query* itu antara lain *query by kind* yang digunakan untuk menampilkan seluruh *conference* yang ada, *query by kind filter by ancestor* yang digunakan untuk menampilkan seluruh *conference* yang dibuat oleh *user* (yang sedang *log-in*), dan *query by kind filter by property* yang digunakan untuk menampilkan *conference* yang akan atau telah diikuti oleh *user*.

![Show Conference](https://lh4.googleusercontent.com/J7dTU6xWUDpitElkXl_W2Y9K4kp4dloFyr0x8QXtVmw=s0 "Show Conference.jpg")

#####**1. Query by Kind**#####
*Query by kind* digunakan pada fitur *show all conferences* untuk menampilkan seluruh *conference* yang ada. *Query* ini digunakan untuk mencari seluruh *entity* dari *conference kind*. *Objectify* digunakan untuk mendapatkan *entity* berdasarkan *key*, yaitu dengan menggunakan *load method*. Dibawah ini adalah kode untuk  medapatkan *entity* dengan *load method* jika kita mengetahui *key* dari *entity*.

    //Get entity by key
    Entity entity = ofy().load().key(key).now();

Namun untuk *conferences*, *key* dari *entity* tidak diketahui karena kita menggunakan *auto allocated ID* oleh *app engine*. Untuk kasus seperti ini dapat digunakan kode dibawah untuk melakukan *query by kind*.

    //Get entity by kind
    Query query = ofy().load().type(Kind.class);

    //Get entity by kind and sort the result based on property value
    Query query = ofy().load().type(Kind.class).order(“property”);

Pada kode untuk melakukan *query* seluruh *conference*, kita harus mendefinisikan *query conferences method* pada *conference* API *class*. Dibawah ini adalah kode untuk melakukan *query by kind*.

    @ApiMethod(
		    name = "queryConferences",
		    path = "queryConferences",
		    httpMethod = HttpMethod.POST
	)
	public List<Conference> queryConferences() {
		// Find all entities of type Conference
		Query<Conference> query = ofy().load().type(Conference.class).order("name");
		return query.list();
	}


APIMethod dideklarasikan dengan *name* “queryConferences”, *path* “queryConferences” dan *httpMethod* HttpMethod.POST. *Query* didefinisikan dengan memanggil *ofy method* serta menspesifikasikan *entity class* yaitu *conference class*. Kemudian hasil pencarian ditampilkan dengan urutan berdasarkan *property* “name”. *List method* dipanggil untuk mengeksekusi *query*.

#####**2. Query by Kind Filter by Ancestor**#####
*Query by kind filter by ancestor* digunakan pada fitur *show all conferences You've Created*. Dengan *ancestor relationship* yang telah didefinisikan, setiap *conference* memiliki *ancestor* yaitu *profile entity* dari *user* yang membuat *conference* tersebut. Hal ini dimanfaatkan untuk melakukan *query* terhadap seluruh *conference* yang dibuat oleh suatu *user* spesifik, yaitu *user* yang sedang *log-in*. Berikut ini adalah kode dasar untuk melakukan *query by kind filter by ancestor*, yaitu dengan menspesifikasikan *key* dari *ancestor* yang turunannya ingin diperoleh.

    //Get entity that have a particular ancestor
    Query query = ofy().load().type(Entity.class).ancestor(key);

Berikut ini adalah fungsi getConferencesCreated pada ConferenceAPI yang digunakan untuk melakukan *query by kind filter by ancestor* untuk fitur  *show all conferences You've Created* pada *Conference Central*.

    @ApiMethod(
		    name = "getConferencesCreated",
		    path = "getConferencesCreated",
		    httpMethod = HttpMethod.POST
    )
    public List<Conference> getConferencesCreated(final User user)
	    throws UnauthorizedException {
	    // If not signed in, throw a 401 error
	    if (user == null) {
		    throw new UnauthorizedException("Authorization required");
	    }
	    String userId = user.getUserId();
	    Key userKey = Key.create(Profile.class, userId);
	    return ofy().load().type(Conference.class).ancestor(userKey).order("name").list();
	}

APIMethod dianotasikan dengan *name* “getConferencesCreated”, *path* “getConferencesCreated” dan *httpMethod* HttpMethod.POST. *User* harus berada dalam keadaan *log-in* karena kita akan menggunakan *UserId* untuk membuat *userKey*, maka dari itu *UnauthorizedException* dispesifikasikan. *Query* didefinisikan dengan memanggil *ofy method* serta menspesifikasikan *entity class* yaitu *conference class* dan *userKey* sebagai *ancestor*. Kemudian hasil pencarian ditampilkan dengan urutan berdasarkan *property* “name”. *List method* dipanggil untuk mengeksekusi *query*.

#####**3. Query by Kind Filter by Property**#####
*Query by kind filter by property* digunakan pada fitur *show all conferences you'll attend (you've attended)*. Fitur ini dapat dilakukan karena pada profile entity terdapat suatu property yang menspesifikasikan *attended conference*, sehingga untuk melakukan *query* terhadap *conference* yang *user* akan hadiri atau sudah dihadiri, filter bekerja berdasarkan nilai *property* *attended conference*. Selain untuk fitur *show all conferences you'll attend (you've attended)*, jenis *query* ini juga dapat digunakan untuk melakukan filter terhadap nilai *property* yang lainnya. Nilai *property* dapat dievaluasi dengan beberapa jenis filter dan dapat pula dikombinasikan dengan operator boolean seperti *and* dan *or*. Jenis-jenis filter tersebut antara lain,

 - Equality Filter : =
 - Member of Filter : =
 - Inequality Filter :
	 - Not Equal to : !=
	 - Less Than : <
	 - Less Than or Equal to : <=
	 - Greater Than : >
	 - Greater Than or Equal to : >=

Berikut ini adalah kode dasar untuk melakukan *query by kind filter by property*, yaitu dengan menspesifikasikan *property*, *operator* filter dan nilai dari *property* yang akan difilter.

    //Get entity based on property value
    Query query = ofy().load().type(Kind.class).filter("property and operator","value");
    
    //Get entity based on value of multiple properties and sort the result based on property value
    Query query = ofy().load().type(Kind.class).order("property");
	    query = query.filter("property1 and operator","value");
	    query = query.filter("property2 and operator","value");

Berikut ini adalah contoh penulisan kode apabila kita ingin melakukan *query* terhadap seluruh *conference* yang berlokasi di London dan memiliki topik *Medical Innovations*.

    public List<Conference>filterPlayground() {
	    Query<Conference> query = ofy().load().type(Conference.class).order("name");
	    query = query.filter("city=","London");
	    query = query.filter("topics=","Medical Innovations");
	    return query.list();
    }
