---
title: 'Sugar ORM: Manipulasi Database pada Android Secara Mudah'
tags:
  - android
  - database
  - orm
  - sugarorm
  - tutorial
id: 117
categories:
  - Akademis
  - Catatan
  - Pemrograman Mobile
date: 2015-08-24 13:04:34
---

Pada waktu belajar untuk mengajar manipulasi basis data (SQL) lokal dengan menggunakan Android, saya membaca cukup benyak tutorial yang berhubungan dengan topik tersebut. Sekian banyak tutorial yang saya baca akhirnya saya menyimpulkan koneksi basis data lokal pada pemrograman Android bisa dibilang cukup (kalau bukan sangat) ribet.

Iseng-iseng saya mencari tutorial manipulasi basis data pada Android dengan menggunakan keyword yang lain, yaitu "Android ORM" dan hasilya ternyata cukup menggembirakan. Ada beberapa pilihan ORM untuk basis data android dan salah satunya yang menarik perhatian saya adalah [Sugar ORM](http://satyan.github.io/sugar/ "SugarORM official Github site"). _Insanely easy way to work with Android Databases_, itulah jaminan yang diberikan.<!--more-->

> Catatan: Singkatnya ORM adalah teknik pemrograman untuk memetakan basis data relasional ke dalam Object Oriented. Bagi yang ingin memahami lebih lanjut apa itu Object Relational Maping (ORM), bisa membaca deskripsi di halaman wikipedia [ini](http://en.wikipedia.org/wiki/Object-relational_mapping "object relational mapping").

Karena pada halaman resminya belum ada tutorial penggunaan secara penuh, artikel ini saya tujukan untuk memberikan penjelasan penggunaan Sugar ORM langkah demi langkah secara tepat dan jelas. Oke, untuk membuktikan Sugar ORM dapat digunakan secara _insanely easy_ saya akan membarikan perbandingan dengan manipulasi basis data pada android secara konvensional. Saya akan membuat aplikasi yang sama persis dengan aplikasi yang dibuat pada tutorial dari [AndroidHive](http://www.androidhive.info/2011/11/android-sqlite-database-tutorial/). Selanjutnya anda bisa memutuskan, mana cara yang lebih mudah.

## Persiapan Pembuatan Project

1.  Yang pertama kali dilakukan untuk menggunakan Sugar ORM tentu saja download _library_-nya di [sini](https://raw.github.com/satyan/sugar/master/dist/sugar-1.2.jar "sugarorm jar"). Sebagai catatan, sewaktu saya mendownload pustaka tersebut nama file-nya adalah sugar-1.2.jar.zip, untuk dapat digunakan kita harus me-rename-nya menjadi sugar-1.2.jar
2.  Selanjutnya mari kita membuat project baru pada eclipse ADT, dan saya tidak akan menjelaskan bagaimana caranya, kalau anda masih belum tahu cara membuat project android melalui Eclipse ADT, silahkan tutup dulu artikel ini dan baca dulu artikel dasar pembuatan aplikasi Android.
3.  Persiapan selanjutnya adalah memasukkan memasukkan _library _SugarORM yang telah didownload pada folder _libs_ pada project kita. Cara yang paling mudah adalah melakukan drag and drop dari file explorer OS kita (jika menggunakan windows berarti Windows Explorer) ke folder libs pada project kita. Cara yang sedikit ribet, klik kanan pada project kita, pilih _**Build Path → Add External Archives**, _kemudian pilih file sugar-1.2.jar yang sudah kita download.[![drag-drop](http://fahrifirdaus.web.id/wp-content/uploads/2014/04/drag-drop-272x300.png)](http://fahrifirdaus.web.id/wp-content/uploads/2014/04/drag-drop.png)
4.  Persiapan terakhir adalah menambahkan sedikit kode pada AndroidManifest.xml. Tidak banyak kok, cukup tambahkan `attribute android:name="com.orm.SugarApp"` dan beberapa elemen `meta-data` seperti ditunjukkan pada kode di bawah ini.

```xml
<application
   android:icon="@drawable/ic_launcher"
   android:label="@string/app_name"
   android:theme="@style/AppTheme"
   android:name="com.orm.SugarApp" >

  <meta-data android:name="DATABASE" android:value="contactsManager.db" />
  <meta-data android:name="VERSION" android:value="1" />
  <meta-data android:name="QUERY_LOG" android:value="true" />
  <meta-data android:name="DOMAIN_PACKAGE_NAME"
             android:value="fahri.lat.sugarcontact.tables" />

  ......
</application>
```

    **Penjelasan:**

    *   Attribut `android:name="com.orm.SugarApp"` untuk memberitahukan bahwa aplikasi ini adalah SugarApp.
    *   Metadata Database untuk memberikan nama file penyimpanan basis data.
    *   Metadata Version untuk memberitahukan versi basis data yang kita buat.
    *   Metadata Query_Log untuk mengindikasikan apakah kita ingin melakukan log terhadap query yang dieksekusi.
    *   Metadata Domain_Package_Name sebagai kontrak awal bahwa class ORM yang merepresentasikan tabel basis data disimpan pada package tersebut.

Oke, setelah selesai persiapannya, saatnya kita membuat class ORM untuk representasi tabelnya.

## Buat Class ORM

Seperti pada ORM lain, dalam Sugar ORM tiap tabel pada basis data direpresentasikan oleh sebuah class, sedangkan kolomnya direpresentasikan ke dalam attribute. Agar dapat bekerja dengan baik, class representasi tabel harus melakukan extends terhadap class SugarRecord.

![new class Contact](/images/new-class-Contact.png)Lebih jelasnya, buatlah Class baru (class biasa) dengan nama Contact, dan ingat sesuai kontrak kita pada android manifest, simpan semua class table pada package fahri.lat.sugarcontact.tables. Selanjutnya tuliskan kode berikut pada class Contact.

```java
import android.content.Context;
import com.orm.SugarRecord;

public class Contact extends SugarRecord<Contact> {

	public String name;
	public String phoneNumber;

	public Contact(Context arg0) {
		super(arg0);
	}

	public Contact(Context arg0, String name, String phoneNumber) {
		super(arg0);
		this.name = name;
		this.phoneNumber = phoneNumber;
	}

}
```

**Penjelasan:**

*   Agar class Contact yang kita buat merepresentasikan sebuah tabel bernama Contact pada basis data, kita harus melakukan extends SugarRecord seperti yang ditunjukkan pada baris 4\. Jangan lupa sebelumnya kita harus melakukan import library tersebut seperti yang ditunjukkan pada baris ke 2.
*   Dua atribut yang dituliskan pada baris ke 5 dan 6 akan di-_transform _menjadi kolom pada tabel Contact. Sengaja kedua attribute tersebut dibuat public untuk memudahkan pengaksesan.
*   Kita tidak perlu membuat kolom id, karena kolom tersebut akan di-generate secara otomatis oleh SugarORM.
*   By default, setiap kita meng-_extend _SugarRecord kita akan diminta untuk membuat satu constructor yang memiliki parameter Context. Di dalam contoh tersebut, kita membuat 2 parameter. Satu constructor yang hanya memiliki 1 parameter Context saja dan satu constructor dengan parameter Context dan attribute yang lain.

_That's it_, hanya cukup itu yang dilakukan dan kita sudah dapat memanipulasi basis data dengan satu tabel tersebut semau kita.

## Saatnya ORM Beraksi

Oke, sama seperti yang dicontohkan pada tutorial dari AndroidHive, saya tidak akan mengubah tampilan apapun pada activity, saya hanya akan langsung mengisikan data pada basis data dan menampilkan data tersebut. Tambahkan baris perintah berikut pada activity utama:

```java
public class MainActivity extends Activity {

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		Contact.deleteAll(Contact.class);

		Log.d("Insert: ", "Inserting ..");
		Contact contact1 = new Contact(this, "Ravi", "9100000000");
		Contact contact2 = new Contact(this, "Srinivas", "9199999999");
		Contact contact3 = new Contact(this, "Tommy", "9522222222");
		Contact contact4 = new Contact(this, "Karthik", "9533333333");
		contact1.save();
		contact2.save();
		contact3.save();
		contact4.save();

		Log.d("Reading: ", "Reading all contacts..");
		List<Contact> contacts = Contact.listAll(Contact.class);

		for (Contact cn : contacts) {
            String log = "Id: "+cn.getId()+" ,Name: " + cn.name + " ,Phone: " + cn.phoneNumber;
            Log.d("Name: ", log);
		}
	}

       ....
}
```

Pada Activity tersebut kita hanya melakukan penyimpanan data ke dalam basis data dan menampilkan data yang telah tersimpan ke dalam Log. _It's as simple as that_.

## Penutup

Oke, seperti itulah dasar manipulasi basis data pada aplikasi Android dengan menggunakan Sugar ORM. Menurut saya cara tersebut jauh lebih sederhana dibandingkan cara konvensional menggunakan DatabaseHandler dan lain sebagainya. Kalau menurut anda lebih sederhana cara konvensional, oke saya tidak akan memaksakan pendapat.

Ya betul, tutorial ini memang masih sangat sederhana, karena tujuan dari tutorial ini adalah menunjukkan dasar penggunaan Sugar ORM dan me-_rewrite_ aplikasi kontak yang didapatkan dari AndroidHive. Di tutorial selanjutnya (bila sempat) akan saya tunjukkan bagaimana melakukan operasi basis data yang lebih kompleks dengan menggunakan SugarORM, (bisa saja) meliputi relasi tabel, melakukan query lanjut, bekerja dengan ArrayAdapter, dan lain sebagainya.

Aplikasi penuh dapat di-_download_ melalui halaman github saya [[di sini]](https://github.com/kaqfa/kuliah_mobile/archive/master.zip "SugarContact").

Terima Kasih, Semoga Bermanfaat.