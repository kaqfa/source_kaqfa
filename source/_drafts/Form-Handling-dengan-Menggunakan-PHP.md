---
title: Form Handling dengan Menggunakan PHP
id: 59
categories:
  - Catatan
tags:
---

Form merupakan elemen terpenting saat kita membuat website yg interaktif. Form adalah tempat end-user menginputkan data yang nantinya akan diolah dan ditampilkan melalui website. Apapun jenis website-nya, baik itu web aplikasi sistem informasi, ataupun web yang bersifat manajemen konten, semua memerlukan form untuk menginputkan data. Artikel ini akan menjelaskan cara membuat form, beberapa jenis inputan yg dapat dibuat menggunakan Form pada HTML, dan juga cara menangani data dari form tersebut.

Membuat Form menggunakan HTML

Aturan pertama dalam pembuatan form yg perlu diingat adalah, semua elemen input form harus ditulis diantara tag 

<form enctype="application/x-www-form-urlencoded" method="get">dan <form enctype="application/x-www-form-urlencoded" method="get">. Tag tersebut memiliki properti target yang isinya adalah alamat URL tujuan tempat pemrosesan form tersebut, dan juga method yang dapat diisi dengan menggunakan get atau post yaitu metode pengiriman data yang diinginkan. Kita akan membahas lebih lanjut kedua properti ini setelah menambahkan beberapa elemen input.Adapun elemen input yang dimiliki html meliputi :
</br>
- input type text (type password, type email, dan type url) untuk menampilkan kotak inputan text
</br>
- select yang diikuti option untuk menampilkan inputan drop down
</br>
- textarea untuk menampilkan inputan text multi baris
</br>
- input type submit atau type reset untuk menampilkan tombol
</br>
Berikut adalah contoh lengkap kode HTML untuk membuat form:

__________#######__________********___________

Data yang diisikan pada form di atas akan dikirimkan ke proses.php jika tombol Simpan ditekan atau bisa juga menggunakan Enter. Namun karena kita belum membuat file proses.php yangakan memanipulasi inputan tersebut,  maka setelah disubmit browser akan menampilkan pesan kesalahan. Oke, agar tidak berlama-lama, mari kita membuat halaman proses.php yang berguna untuk menampilkan nilai inputan tersebut secara sederhana. Kode lengkap untuk proses.php adalah sebagaimana ditampilkan pada kode dibawah ini :
</br>

</form>