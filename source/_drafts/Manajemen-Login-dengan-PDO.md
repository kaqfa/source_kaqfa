---
title: Manajemen Login dengan PDO
tags:
  - login
  - pdo
  - php
  - web
id: 318
categories:
  - Akademis
  - Pemrograman Web
---

Login dan Logout merupakan fitur wajib aplikasi yang memerlukan otentikasi pengguna. Kebanyakan tutorial yang ada menjelaskan manajemen login dan logout dengan menggunakan driver mysql_ yang saat ini sudah dianggap kedaluarsa (_deprecated)_ oleh PHP. Pada artikel ini akan dijelaskan manajemen login dan logout secara modular dengan menggunakan PDO (_PHP Data Object_).

Skema manajemen login yang akan diimplementasikan pada tutorial ini ditunjukkan sebagaimana gambar skema di bawah ini:

[![](http://fahrifirdaus.web.id/wp-content/uploads/2016/12/login-php.png)](http://fahrifirdaus.web.id/wp-content/uploads/2016/12/login-php.png)

*   **login.php**: adalah file yang berisi HTML untuk menampilkan form login dengan dua field username dan password.
*   **process.php**: adalah file yang bertugas mengeksekusi logika manajemen login yaitu mencocokkan username dan password yang di-submit melalui halaman login dengan data yang tersimpan pada tabel user di basis data. Jika sesuai (valid) maka sistem akan menyimpan informasi login pada session dan melakukan mengarahkan proses pada admin.php, jika tidak sesuai (tidak valid) maka sistem akan mengarahkan kembali ke halaman login.
*   **config.php**: file ini merupakan file yang berisi konfigurasi koneksi basis data dengan PDO. File ini akan dilampirkan (_include_/_require_) pada semua file yang memerlukan koneksi ke database. Dalam kasus ini karena kita hanya membuat manajemen login sederhana, yang melakukan _include_ file ini hanya process.php.
*   **admin.php**: merupakan file pengantar untuk mengarahkan sistem ke halaman admin yang sesuai. Dalam file ini kita tidak menuliskan kode HTML tapi hanya beberapa logika pengarahan halaman (_routing_) dengan menggunakan PHP dan inklusi file-file yang diperlukan sesuai kondisi yang ada.
Oke cukup teorinya sekarang saatnya beraksi dengan menuliskan kode:

### user_table.sql

<script src="https://gist.github.com/kaqfa/f4d030b2dd92490879c0651dd13c790b.js"></script>

Sebelum kita menuliskan kode pemrograman PHP, sebaiknya kita membuat tabel Users terlebih dahulu. Silahkan membuat database yang diinginkan dan eksekusi perintah SQL tersebut untuk membuat tabel Users yang akan digunakan pada tutorial ini.

### login.php

<script src="https://gist.github.com/kaqfa/569888708917303508bacd51977e1e6c.js"></script>

Tidak banyak yang bisa dijelaskan pada file login.php tersebut, karena sudah cukup jelas bahwa koding tersebut adalah tampilan untuk halaman Login.

### config.php

<script src="https://gist.github.com/kaqfa/762eb1fafdad25e26600f228aae36ec6.js"></script>

File ```config.php``` tersebut hanya berisi konfigurasi database dan koneksi yang digunakan pada tutorial ini.

### process.php

<script src="https://gist.github.com/kaqfa/168575dace4eb788be519aeb28b02782.js"></script>

Pada file tersebut ada 1 fungsi ```loginCheck()``` untuk mengecek username &amp; yang diinputkan dengan data yang tersimpan pada tabel users. Kemudian baris ke 19 dan selanjutnya melakukan pengarahan (redirection) sesuai dengan hasil pengecekan.

<script src="https://gist.github.com/kaqfa/ca151ece6dbe17b9d7ef633178cd97ba.js"></script>

Pada ```admin.php``` kita melakukan pengecekan data login yang tersimpan pada session (atau cookie jika diinginkan) dan menampilkan tampilan admin yang sebenarnya sesuai dengan kondisi variabel yang diberikan pada URL.

<script src="https://gist.github.com/kaqfa/a221d759d259ed7ebe815af910595d44.js"></script>

Terakhir ada pada file ```dashboard_view.php``` yang merupakan tampilan sebenarnya dengan melakukan eksekusi pada pengiriman variabel pada halaman admin.

Pada tutorial ini kita tidak banyak melakukan manajemen koding yang rumit, karena tujuannya adalah memberikan demo cara pembuatan manajemen login dengan menggunakan PDO. Meski demikian, dengan menggunakan alur program yang diberikan, kita sudah mendapatkan kemudahan penempelan halaman admin apapun sesuai dengan yang kita inginkan.