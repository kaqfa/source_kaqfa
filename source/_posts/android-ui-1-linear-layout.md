---
title: Android UI - 1. Linear Layout
tags:
  - android
  - linearlayout
  - tutorial
id: 76
categories:
  - Pemrograman Mobile
date: 2015-08-24 15:19:56
banner: /images/androi-UI.jpg
thumbnail: /images/androi-UI.jpg
---

Tutorial ini merupakan tutorial pertama seri desain dasar aplikasi mobile dengan Android. Semoga setelah ini sempat melanjutkan sampai target mengombinasikan beberapa prinsip layout menjadi aplikasi mobile berbasis android utuh. Oke, tidak perlu panjang lebar, langsung menuju ke topik pertama yaitu Linear Layout.

Desain tampilan antarmuka (UI) pada aplikasi mobile cukup berbeda dengan aplikasi desktop. Alasan terbesar yang membedakan keduanya adalah ukuran layar perangkat mobile yang relatif terbatas. Sebab itulah, kebanyakan layout dari aplikasi mobile cenderung sederhana agar tidak menghabiskan tempat. Diantara prinsip layout sederhana yang digunakan pada aplikasi mobile, salah satu yang paling mudah dibuat adalah Linear Layout

Pada prinsipnya Linear Layout adalah menempatkan komponen visual secara berjejer (baik secara horizontal, maupun vertikal) dengan ukuran yang sama. Dalam android caranya cukup mudah, yaitu buat View LinearLayout sebagai container, kemudian masukkan semua komponen di dalam View LinearLayout tersebut. Secara singkat, urutan penggunaan LinearLayout dalam Android adalah sebagai berikut:
<!--more-->

1.  Buat Activity baru (atau Android Project baru ).
2.  Akses file XML layout dalam direktori **res -&gt; layout**.
3.  Hapus semua isi pada file XML layout tersebut dan ganti dengan View `LinearLayout`.
4.  Untuk memastikan LinearLayout diterapkan pada seluruh area, pastikan atribut `layout_width` dan `layout_height` bernilai `match_parent`.
5.  Tentukan orientasi yang diinginkan, Horizontal atau Vertical.
6.  Tambahkan komponen yang diperlukan ke dalam Layout.

Jika anda pernah mengembangkan aplikasi dengan AndroidSDK sebelumnya, saya rasa instruksi singkat di atas sudah cukup jelas. Tapi jika memang belum jelas, atau anda belum memiliki pengalaman yang cukup dalam pengembangan Android, baiklah saya akan berbaik hati menjelaskannya.

### 1\. Buat Activity / Project Baru

Oke untuk yang bagian ini langsung _straight forward_ aja, pilih menu File - New - Android Application Project. Inputkan Application Name, Project Name, dan Package Name sesuai keinginan, kemudian klik Next - Next - Next -Next.

![New Android Linear](/images/New-Android-Linear.png)

Pada form inputan Activity Name isikan "LinearDemo" dan pada Layout Name isikan `activity_linear_demo`. Selesai klik Finish.

![Android Activity Form](/images/Android-Activity-Form.jpg)

### 2\. Akses file Layout

Semua file layout pada Android Project selalu disimpan pada direktori res - layout. Pada direktori tersebut, carilah file layout yang sesuai (dalam kasus ini, jika anda menurut pada saya berarti nama file-nya adalah `activity_linear_demo.xml`), double klik untuk membuka file tersebut.

![layout_file](/images/layout_file.jpg)

By default (jika anda menggunakan Eclipse), IDE Eclipse akan menampilkan file layout dalam mode grafis. Jika anda masih ingin membaca tutorial ini, saya sarankan anda untuk menggunakan mode XML editor saja. Saya malas kalau harus membuat tutorial drag and drop mode grafis, terlalu banyak capture membuat jari-jari capek.

### 3\. Hapus dan Ganti dengan LinearLayout

Sekali lagi, Eclipse emang sok pinter. By default, Eclipse langsung memberikan RelativeLayout sebagai layout dasar tampilan, padahal tidak setiap hari kita mau menggunakan RelativeLayout. Jadi pekerjaan selanjutnya adalah menghapus semua isi dari file XML tersebut dan menggantinya dengan LinearLayout seperti ditunjukkan kode di bawah ini :

```xml
<xml>
	
</xml>
```

### 4\. Width dan Height Match Parent

Pada kode sebelumnya memang sudah saya tuliskan `android:layout_width = "match_parent"` dan juga `android:layout_height="match_parent"`, tapi untuk memastikan aja barangkali anda kelewatan menuliskannya. Kode tersebut memastikan layout kita memang dipakai seluruh halaman.

### 5\. Tentukan Orientasi

Ya sama seperti poin sebelumnya, sepertinya saya memang terlalu paranoid. Pastikan anda sudah menambahkan atribut `android:orientation="vertical"` untuk membuat komponen yang ditambahkan berjajar secara urut dari atas ke bawah. Tapi kalau anda ingin urut dari kiri ke kanan, berarti ubah nilainya menjadi horizontal.

### 6\. Tambahkan Komponen Lain

Tahap terakhir adalah menambahkan komponen (apapun sesuka anda). Biasanya saya suka menambahkan beberapa button, tapi jika anda tidak memiliki selera yang sama dengan saya, anda boleh juga menambahkan komponen lain seperti `EditText`, `TextView`, atau bahkan `Spinner`.

Dalam project saya hasil akhir kode dari layout adalah seperti di bawah ini:

```xml
<xml>
	
</xml>
```

dan saat dijalankan hasilnya seperti di bawah ini:

![LinearLayoutApp](/images/LinearLayoutApp.jpg)

Loh kok cuman gitu?

Ya karena ini hanya tutorial awal cara menggunakan LinearLayout saja, kita belum sampai membahas menggunakan style dan komponen desain UI lain untuk mempercantik tampilan. Tunggu saja tutorial berikutnya.