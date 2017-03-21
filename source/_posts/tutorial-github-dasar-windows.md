---
title: Tutorial Github Dasar (Windows)
tags:
  - git
  - github
  - koding
id: 227
categories:
  - Catatan
  - Tool Pemrograman
date: 2015-09-19 14:13:18
---

Pada artikel [sebelumnya](http://fahrifirdaus.web.id/2015/02/github-curriculum-vitae-untuk-programmer/), saya  sudah menjelaskan manfaat menggunakan Github untuk membentuk curriculum vitae kita. Dan pada artikel kali ini, saya akan bertanggung jawab menjelaskan cara membuat project dan melakukan upload ke github dengan menggunakan console dan GUI.

## GUI Github for Windows

Cara yang paling cepat untuk melakukan koneksi dan interaksi dengan Github adalah dengan menginstall Github for Windows, dan langsung create project dan singkronisasi data dengan aplikasi tersebut. Langkah-langkahnya adalah sebagai berikut:<!--more-->

### Registrasi Akun github.com

1.  Gunakan browser favorit anda untuk mengakses github.com
2.  Pada tampilan awal github, kita akan disuguhi form registrasi. Lanjutkan dengan mengisikan username, email, dan password pada form registrasi tersebut dan klik tombol **Sign up for Github**.
![](/images/Github-0.jpg)
3.  Langkah kedua registrasi kita akan ditawari beberapa plan untuk registrasi, untuk basic-nya kita bisa memilih free plan di pilihan paling bawah, kemudian klik **Finish sign up** untuk melanjutkan registrasi.
![](/images/github-00.jpg)

### Instalasi Github for Windows

1.  Selamat anda telah berhasil melakukan registrasi akun pada github.com dan bisa melanjutkan untuk Download aplikasi Github for Windows di http://windows.github.com/
2.  Pasang dan jalankan aplikasi tersebut di komputer kita
3.  Untuk menghubungkan aplikasi github tersebut dengan akun github yang telah kita buat, klik pada menu settings (ikon gear) di kanan atas, dan pilih menu **Options...**
![](/images/github1.jpg)
4.  Klik link **+ Add accounts** untuk menambahkan akun anda
5.  Masukkan username dan password sesuai dengan data yang kita masukkan saat registrasi akun github.
![](/images/github-2.jpg)
6.  Selanjutnya adalah konfigurasi profile lokal dengan kembali lagi klik menu **Options...** kemudian isikan Full Name dan Email yang kita gunakan untuk daftar di github.com.
7.  Terakhir klik **Save**.

### Membuat Repositori Baru

1.  Untuk membuat repositori koding baru, klik ikon **+** yang ada di kiri atas aplikasi, dan pilih tab **create**
2.  Masukkan nama untuk repository kita, namun ingat karakter yang diterima hanya huruf, "-" (dash), dan "_" (underscore).
![](/images/github-3.jpg)
3.  Aplikasi github akan memilihkan tempat default untuk repository kita. Namun, jika kita ingin menempatkan di lokasi lain, kita dapat memilih browser pada pilihan **Local path**. Dalam kasus saya, karena saya ingin menuliskan koding untuk pemrograman PHP, maka saya meletakkan Local path pada direktori _htdocs_.
4.  Setelah itu klik **Create repository**
5.  Selamat anda berhasil membuat repository baru.

> Catatan: repository baru yang kita buat akan otomatis dibuatkan file .gitattributes dan .gitignore. Biarkan saja kedua file tersebut, karena sangat berguna untuk kita nantinya. Jika ingin mengetahui lebih lanjut fungsi dari 2 file tersebut silahakan lihat dokumentasi git tentang [gitattributes ](http://git-scm.com/docs/gitattributes)dan [gitignore](http://git-scm.com/docs/gitignore)

### Bekerja dan Sinkronisasi Pekerjaan

Istilah yang digunakan dalam git adalah add, commit, dan push. Berikut adalah contoh cara untuk sinkronisasi koding yang kita buat pada repositori lokal dengan repositori github.

1.  Buka direktori repositori yang barusan kita buat
2.  Dalam direktori tersebut buatlah sebuah file bernama **readme.md**, kemudian tuliskan teks di bawah ini serta Simpan dan buka kembali windows explore.

```
# Proyek Latihan Koding

Proyek ini digunakan untuk latihan koding PHP dan menyimpannya pada github.
```

3.  Buat satu lagi sebuah file dengan nama **index.php** dan tuliskan kode di bawah ini serta Simpan dan kali ini buka aplikasi github for windows. Terlihat kita memiliki **2 uncommitted changes**, artinya kita memiliki 2 file yang telah dirubah / ditambahkan tapi kita belum menyimpannya pada repositori lokal.

```php
<?php
	echo "<h1>Hello World Biasa</h1>";
?>
```

4.  Untuk menyimpannya isikan komentar commit pada isian **Summary** kemudian klik **Commit to master**.
![](/images/github-4.jpg)
5.  Kita telah berhasil menyimpan pada repository lokal, selanjutnya klik tombol **Publish** yang ada di bagian kanan atas di bawah ikon gear.
6.  Saat pertama kali kita melakukan publish kita akan diminta untuk memasukkan **Description** dari repositori ini. Isikan deskripsi pada isian tersebut dan klik **Publish [nama-repositori]**.
> Jangan mencentang pilihan private repository, karena pilihan tersebut hanya untuk akun berbayar. Jika kita centang maka kita akan diminta untuk meng-upgrade akun kita.
7.  Tunggu sebentar hingga proses upload selesai (saat publish harus punya koneksi internet tentunya).

Setelah proses upload berhasil, kita bisa mengecek hasil pekerjaan kita di github.com, seharusnya 2 file yang barusan kita buat sudah berhasil terunggah di repository github. Dan ya, sekarang kita tahu kode yang kita tuliskan pada **readme.md** menjadi dokumentasi resmi dari repository kita dengan menggunakan format _**markdown**_ (penjelasan lebih lanjut tentang format markdown github dapat dibaca di [halaman ini](https://help.github.com/articles/markdown-basics/)).

## Penutup

Github bukan hanya sekedar tool untuk menyimpan koding secara online, tapi tujuan utamanya adalah tool untuk memudahkan kolaborasi koding antar beberapa programmer. Pada artikel ini hanya dijelaskan pengenalan menggunakan github saja, sedangkan penjelasan lebih lanjut tentang penggunaan github untuk _colaborative coding_ akan dijelaskan pada artikel selanjutnya. Atau jika anda tidak sabar, silahkan baca artikel yang sangat istimewa dari nettuts yang berjudul [Team Collaboration With GitHub](http://code.tutsplus.com/articles/team-collaboration-with-github--net-29876).