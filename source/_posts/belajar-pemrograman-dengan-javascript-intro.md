---
title: Pengenalan Pemrograman dengan Javascript
tags:
- dasar pemrograman
- javascript
- pemrograman
id: 138
categories:
- Akademis
- Pemrograman Dasar
date: 2014-07-13 23:00:52
---

Pemrograman ada proses memberikan perintah kepada komputer untuk melakukan sesuatu, seperti menampilkan gambar, mencetak laporan, melakukan perhitungan, dan lain sebagainya. Secara teknis memang komputer hanya memahami perintah berupa angka-angka abstrak yang susah dipahami oleh manusia, sehingga untuk membantu manusia berkomunikasi dengan komputer dibuatlah bahasa pemrograman yang mendekati natural bahasa manusia (bahasa inggris).

Ada banyak bahasa pemrograman di dunia ini, sedangkan yang akan digunakan untuk belajar pemrograman dalam seri tutorial yang saya buat ini adalah bahasa pemrograman berbasis browser "Javascript". _Semoga seri berikutnya bisa saya posting secara teratur tanpa hambatan._<!--more-->

Sebelum membahas lebih jauh, saya akan langsung saja menjawab pertanyaan yang mungkin banyak muncul di benak pembaca yang sedikit banyak pernah belajar pemrograman. Kenapa menggunakan Javascript? Kenapa tidak menggunakan C, Java, atau Pascal seperti yang banyak diajarkan di universitas?

Oke sebelum menjawab pertanyaan tersebut, terlebih dahulu kita harus sepakat bahwa Javascript adalah bahasa pemrograman yang tidak kalah ampuhnya dibandingkan bahasa pemrograman lain. Meskipun memang program yang dibuat menggunakan Javascript kebanyakan hanya dijalankan pada browser, tidak dijalankan langsung melalui executable file seperti program yang dibuat dengan bahasa pemrograman lain. Kalau sudah sepakat, baru saya akan menjelaskan beberapa alasan kenapa menggunakan Javascript untuk belajar pemrograman, kalau masih belum "ayo gelut sek sedelok".

Ehm ehm, oke jadi alasannya adalah sebagai berikut:

1.  **Murah**, untuk membuat program menggunakan javascript kita tidak perlu menginstall aplikasi tambahan apapun, cukup mengguanakan teks editor sederhana dan sebuah browser kita sudah dapat membuat dan mengeksekusi program yang kita buat.
2.  **Derivative C**, gaya syntax Javascript banyak mengambil gaya pemrograman bahasa C. Bagi anda yang belum tahu, bahasa C adalah bahasa pemrograman yang model syntaxnya sangat banyak ditiru oleh bahasa pemrograman lain. Java, C++, C#, PHP, Python adalah bahasa pemrograman yang paling banyak digunakan, dan itu semua merupakan turunan dari C. Diharapkan jika kita  sudah menguasai salah satu dialek syntax bahasa pemrograman bahasa C untuk berpindah ke bahasa pemrograman lain yang merupakan turunan C juga akan lebih mudah.
3.  **Mudah**, dibandingkan dengan bahasa C, Javascript relatif jauh lebih mudah terutama dalam hal pengelolaan variabel bertipe data String dan pengelolaan memori. Selain itu, tidak seperti Java yang harus dealing with Class dan Object sejak pertama kali menulis program, Javascript dapat digunakan secara prosedural dan Object Oriented. Oke saya tidak akan membandingkan dengan Python & PHP, karena semuanya bisa dibilang hampir sama mudahnya.
4.  **Sedang trend**, saat ini bahasa Javascript tidak hanya digunakan untuk browser saja, banyak aplikasi pemrograman yang menggunakan Javascript. Untuk membuat aplikasi mobile kita bisa menggunakan Phonegap & Appcelerator yang berbasis Javascript, untuk aplikasi server side ada Node.js yang cukup terkenal. Oke, untuk aplikasi desktop memang belum ada tools yang menggunakan Javascript murni (ada TideSDK tapi masih BETA). Tapi saya yakin, _in not so far future_ bakal ada pemrograman desktop yang menggunakan Javascript.
5.  **Harvard & Stanford**, Stanford's Computer Science 101 menggunakan Javascript untuk bahasa utamanya. Harvard's Introduction to Computer Science juga menggunakan Javascript sebagai bahasa pemrograman utama.

Oke sepertinya sudah cukup banyak alasan kenapa menggunakan Javascript sebagai bahasa pertama untuk belajar pemrograman. Selanjutnya mari kita mulai saja membuat aplikasi pertama kita dengan Javascript.

## Many Faces of HelloWorld

Semenjak Denis Ritchie menerbitkan bukunya tentang pemrograman bahasa C, komunitas programmer sering menggunakan program HelloWorld sebagai program pertama yang dibuat saat belajar bahasa pemrograman baru. HelloWorld adalah program yang sungguh amat sangat sederhana sekali, untuk menampilkan sebuah kalimat "selamat datang" ke dalam dunia pemrograman.

Dengan menggunakan Javascript, ada beberapa cara untuk membuat program HelloWorld (yang semuanya tentunya memerlukan browser sebagai tempat eksekusi). 1) Menampilkannya ke dalam halaman web, 2) Menampilkan dengan sebuah kotak dialog, 3) Menggunakan Developer Console yang ada pada browser.

### Menampilkan pada Halaman Web

Javascript adalah pemrograman yang datang sepaket dengan HTML dan CSS untuk membentuk web. Maka tidak heran jika penulisan javascript selayaknya diinputkan secara langsung ke dalam kode HTML dan ditampilkan pada halaman web. Langsung saja, buatlah file hello.html menggunakan editor yang terinstall di komputer anda (disarankan menggunakan SublimeText atau Notepad++), dan tuliskan kode berikut:

```html
<html>
<head>
   <title>Latihan Javascript</title>
</head>
<body>
   <script type="text/javascript">
      document.write("Selamat datang di dunia Pemrograman");
   </script>
</body>
</html>
```

Simpan dan double klik file yang telah anda buat untuk menampilkan ke dalam browser. Voila, anda resmi sudah berhasil menjadi programmer Javascript.

Katanya mudah, kok nampilin satu baris aja kodingnya panjang banget?

Sebenarnya, dalam koding tersebut terdapat konstruksi HTML lengkap. Bagian yang murni untuk menampilkan javascript hanya sebagian saja. Oke, kalau anda pengen yang lebih pendek, edit koding pada file hello.html tersebut sehingga menjadi seperti ini:

```html
<script type="text/javascript">
   document.write("Selamat datang di dunia Pemrograman");
</script>
```

Simpan dan sekali lagi jalankan pada browser. Hasilnya sama persis dan tulisannya jauh lebih pendek bukan? Meski demikian cara tersebut tidak disarankan, karena akan menghilangkan elemen HTML yang lain dan di dunia nyata kita tidak akan membuat program Javascript dengan cara seperti itu. Selanjutnya saya akan tetap mengajarkan dengan menggunakan cara yang pertama.

Mari kita lanjutkan ke pembuatan program HelloWorld dengan cara yang kedua.

### Menampilkan dengan Kotak Dialog

Kalau anda tidak suka hanya menampilkan dalam bentuk tulisan pada halaman browser, anda bisa juga menampilkannya dalam bentuk kotak dialog. Dengan javascript caranya sangat mudah sekali, buatlah satu file baru dengan nama hello_dialog.html dan tuliskan kode berikut:

```html
<html>
<head>
   <title>Latihan Javascript</title>
</head>
<body>
   <script type="text/javascript">
      alert("Selamat datang di dunia Pemrograman");
   </script>
</body>
</html>
```

Simpan, kemudian jalankan pada browser. Jika kode yang anda tuliskan betul, maka anda akan mendapatkan sebuah kotak dialog yang bertuliskan "Selamat datang di dunia Pemrograman" (tanpa tanda petik tentunya) dan kotak tersebut akan hilang saat kita mengklik tombol OK.

Sebenarnya bisa dibilang tidak mungkin kita akan membuat program Javascript yang berisi banyak sekali kotak dialog yang bermunculan untuk melaksanakan aksinya (kecuali kalau memang ingin ngerjain penggunannya). Tapi, karena pengamatan saya banyak sekali orang yang tertarik dengan menampilkan kotak dialog saat belajar Javascript, jadi saya ajarkan caranya di sini.

Kalau sudah puas menampilkan banyak kotak dialog, mari kita lanjutkan ke cara yang terakhir.

### Developer Console

Tidak banyak programmer pemula yang tahu bahwa di semua browser modern terdapat satu tools yang cukup powerfull untuk menguji coba kode Javascript tanpa harus mengubah kode asli pada file. Memang kita tidak akan membuat program dengan menggunakan Developer Console ini, karena sifatnya sementara (begitu browser ditutup, data akan hilang). Namun tools ini sangat berguna sekali untuk mengedit program dan langsung mengetahui hasilnya tanpa harus bolak-balik buka editor dan browser.

Cara untuk menampilkan developer tools adalah :

*   jika anda menggunakan Google Chrome tekan Ctrl + Shift + J
*   jika anda menggunakan Mozilla Firefox tekan Ctrl + Shift + K
*   jika anda menggunakan Internet Explorer download dulu Chrome atau Firefox, eh (maaf bercanda) maksud saya tekan F12 dan klik tab "Console"

Untuk menguji coba cara ini, kita tidak perlu membuat file baru. Cukup buka file hello.html yang sudah kita buat kemudian ketikkan perintah berikut pada developer Console:

```javascript
document.write("ini juga HelloWorld lhooo.....");
```

Tekan enter untuk menjalankan, dan lihat tulisan pada halaman anda sudah berubah seperti yang kita tuliskan pada developer console.

Boleh juga anda mencoba menampilkan HelloWorld dalam bentuk kotak dialog dengan menuliskan perintah berikut dan tekan tombol enter:

```javascript
alert("ini juga HelloWorld lagi dan lagi")
```

##  Membenahi Kesalahan

Kemungkinan seorang pemula menuliskan kode program sederhana dengan betul tanpa mengalami error (kesalahan penulisan) adalah 10%. Sedangkan kemungkinan programmer professional menuliskan kode pemrograman yang kompleks tanpa pernah mengalami kesalahan penulisan adalah 0,001%. Mendapatkan error saat membuat program adalah sesuatu yang sangat biasa, jangan khawatir, temen anda yang salah banyak juga kok. Yang terpenting bukanlah menuliskan kode tanpa kesalahan, tapi mengidentifikasi dan segera mencari solusi dari permasalahan yang didapat.

Bagi yang pernah belajar bahasa C, Pascal, Java, VB, dan lain sebagainya, mungkin berpikir kalau kita salah tulis kode dengan menggunakan bahasa pemrograman tersebut kita akan diberi peringatan saat melakukan kompilasi. Namun, karena Javascript bukanlah bahasa pemrograman yang perlu dikompilasi pengecekan syntax pada saat kompilasi tidak akan terjadi. Untunglah pada browser modern ada developer tools yang dapat digunakan untuk mengecek kesalahan penulisan pada program Javascript. Marilah kita mencoba membenahi kesalahan pada kode berikut:

```html
<html>
<head>
   <title>Latihan Javascript</title>
</head>
<body>
   <script type="text/javascript">
      docment.write("Selamat datang di dunia Pemrograman");
   </script>
</body>
</html>
```

Perhatikan, pada kode tersebut bagian javascript seharusnya tertulis `document.write(" ... ");` tetapi tertulis `docment.write(" ... ");` tidak ada huruf "u" pada kata "docment". Jika dijalankan pada browser tidak akan ada kalimat yang ditampilkan, karena salah penulisan.

Sekarang buka developer console menggunakan salah satu cara yang dijelaskan sebelumnya (tergantung browser yang dipakai), dan terlihat ada peringatan kesalahan "<span style="color: #ff0000;">Uncaught ReferenceError: docment is not defined</span>". Pesan kesalahan tersebut mengatakan bahwa perintah "docment" tidak terdefinisi, tidak dapat dipahami. Dengan demikian kita mengetahui di mana letak kesalahan dan apa yang harus dibenahi.

## Penutup

Pada artikel ini penulis memberikan gambaran awal tentang membuat program dengan menggunakan Javascript. Selain itu dalam artikel ini juga dikenalkan sebuah tools developer console yang cukup efektif dan powerful untuk menguji coba kode pemrograman yang dibuat, termasuk menemukan kesalahan dalam penulisan kode pemrograman Javascript. Pada seri artikel selanjutnya akan dijelaskan beberapa aspek penting pemrograman yang patut dipertimbangkan saat membuat program.

Selamat mencoba, dan semoga berhasil.