---
title: Variabel & Tipe Data Pada Javascript
id: 154
categories:
- Akademis
- Algoritma
- Pemrograman Dasar
date: 2014-07-14 12:29:47
tags:
---

Pada artikel sebelumnya kita telah membahas bagaimana memulai pemrograman dengan menggunakan Javascript, tapi kita masih belum melakukan permainan logika apapun, hanya menampilkan teks saja. Pada artikel ini, kita akan membahas dan menguji coba lebih jauh konsep dasar pemrograman yaitu variabel dan tipe data.

## Variabel dan Konstanta

Variabel adalah tempat penampungan data yang akan digunakan dalam program. Disebut variabel karena data yang tersimpan di dalamnya dapat berubah-ubah sesuai kebutuhan, sedangkan penampung data yang tidak dapat berubah setelah diberikan nilai awal disebut Konstanta.<!--more-->

Sebagai contoh mari kita buat sebuah file dengan nama var.html dan tuliskan kode pemrograman di bawah ini:

```html
<html>
<head>
   <title>Belajar Variabel &amp; Konstanta</title>
</head>
<body>
   <script type="text/javascript">
      document.write("Seorang mahasiswa bernama Fahri Firdausillah.<br>");
      document.write("Fahri Firdausillah belajar di Universitas Dian Nuswantoro.<br>");
      document.write("Fahri Firdausillah memprogram dengan menggunakan Javascript");
   </script>
</body>
</html>
```

Tidak ada masalah teknis dengan kode program tersebut. Saat file tersebut dijalankan, browser akan menapilkan teks yang diberikan dengan baik. Hanya saja penulisannya kurang efektif, karena ada teks yang ditulis secara berulang 3 kali, yaitu nama "Fahri Firdausillah".

Untuk membuatnya lebih efektif, kita dapat menyimpan menyimpan teks tersebut ke dalam sebuah variabel <code>nama</code> dan memanggilnya berulang-ulang tanpa harus menulis ulang. Lebih jelasnya ubah kode yang ada pada file var.html menjadi seperti di bawah ini:

```html
...
<script type="text/javascript">
   var nama = "Fahri Firdausillah";
   document.write("Seorang mahasiswa bernama "+nama+".<br>");
   document.write(nama+" belajar di Universitas Dian Nuswantoro.<br>");
   document.write(nama+" memprogram dengan menggunakan Javascript");
</script>
...
```

Perhatikan, pada kode program yang telah diperbaharui kita menambahkan variabel `nama` dan memberikannya data (atau lebih sering disebut nilai/_value_) sebuah teks "Fahri Firdausillah". Selanjutnya saya dapat memanggil variabel tersebut berulang-ulang dan mendapatkan hasil yang sama persis dengan kode program sebelumnya.

> Tanda / operator "+" pada teks tersebut disebut operator konkatenasi, digunakan untuk menggabungkan dua teks atau lebih. Penjelasan lebih lanjut akan diberikan pada bagian tipe data dan Operator di bawah.

Ada tiga langkah pembuatan dan penggunaan variabel dalam pemrograman, yaitu:

1.  Deklarasi: mendefinisikan nama variabel menggunakan perintah `var nama_variabe`.
2.  Assignment: pengisian nilai/data ke dalam variabel menggunakan operator `=`.
3.  Using: penggunaan variabel dengan memanggil secara langsung.

Meski demikian, ketiga langkah tersebut tidak harus dijalankan satu-persatu tiap baris. Seringkali, langkah pertama dan kedua dijalankan sekaligus seperti pada contoh yang telah diberikan.

Variabel bukan hanya berfungsi untuk menyimpan teks saja, bahkan variabel akan lebih terlihat fungsinya saat digunakan untuk menyimpan data sementara yang akan digunakan untuk proses selanjutnya. Oke, untuk contoh kali ini, kita coba dulu dalam developer console. Bukalah developer console, kemudian ketikkan perintah berikut berturut turut:

*   `var nilai1 = 20;`↵
*   `var nilai2 = 50;`↵
*   `var jumlah = nilai1 + nilai2;`↵
*   `alert("Jumlahnya adalah :"+jumlah);`↵

Cukup puas dengan hasilnya? Sekarang setelah berhasil mencobanya dengan menggunakan developer console, saatnya menuliskannya dalam var.html yang sudah kita buat sebelumnya:

```html
<script type="text/javascript">
   var nama = "Fahri Firdausillah";
   document.write("Seorang mahasiswa bernama "+nama+".<br>");
   document.write(nama+" belajar di Universitas Dian Nuswantoro.<br>");
   document.write(nama+" memprogram dengan menggunakan Javascript");
</script>
<hr>
<script type="text/javascript">
   var nilai1 = 20;
   var nilai2 = 50;
   var jumlah = nilai1 + nilai2;
   document.write("Hasil Penjumlahan "+nilai1+" + "+nilai2+" adalah "+jumlah);
</script>
```

Kode yang di-highlight di atas adalah kode baru yang ditambahkan dan sudah diuji coba pada developer console. Silahkan coba mengubah-ubah angka yang diinputkan pada variabel nilai1 dan nilai2, kemudian amati hasilnya.

Kode tersebut menunjukkan kegunaan sebenarnya dari variabel, yaitu menampung data sementara dan menggunakannya untuk proses selanjutnya (dalam kasus tersebut untuk operasi penambahan dan menampilkan). Pada program yang lebih besar, variabel yang digunakan juga akan lebih besar dan lebih beragam tentunya. Baiklah kalau tidak ada pertanyaan, kita lanjutkan ke bagian selanjutnya yaitu tipe data.

## Tipe Data

Pada contoh program sebelumnya, kita lihat ada satu operator yang sama yaitu operator + namun efek yang ditimbulkan berbeda. Pada teks operator tersebut berfungsi untuk menggabungkan dua teks atau lebih, sedangkan pada angka operator tersebut berfungsi untuk menambahkan nilai dari dua pasang angka. "Teks" dan "angka" dalam pemrograman disebut sebagai tipe data.

Dalam bahasa pemrograman yang lebih _strict_ seperti C atau Java, ada banyak tipe data dasar pemrograman, antara lain string, integer, float, double, character, dan lain sebagainya. Dalam javascript untuk tipe data dasar kita cukup mengetahui dua jenis tipe data, yaitu string/teks dan number/angka.

Tipe data angka/number adalah semua semua data yang dapat dilakukan operasi aritmatik, sedangkan tipe data teks adalah untaian (string) karakter yang tersusun. Untuk menuliskan data dengan tipe data angka, kita tinggal menuliskan saja angka (dengan tambahan karakter "." untuk angka desimal), sedangkan untuk menuliskan data String kita harus mengapit karakter apapun dengan tanda petik.

Perbedaan tipe data mempengaruhi perbedaan hasil saat diberikan operator tertentu, malah seringkali perintah hanya dapat dijalankan pada satu tipe data dan tidak dapat dijalankan pada tipe data lain, contoh perintah perkalian atau pembagian. Kira-kira apa yang terjadi bila kita memaksakan operator pembagian untuk tipe data teks/string?

Untuk menjawab pertanyaan tersebut, kita tidak perlu repot-repot membuat halaman HTML baru yang berisi javascript, tapi cukup menggunakan developer console saja. Baiklah, langsung saja buka developer console dan jalankan instruksi berikut `"aku" * "cinta"  / "kamu"` ↵.

Jelas terlihat, hasilnya adalah `NaN` yang merupakan singkatan dari `_Not a Number_`.