---
title: Seni Pemrograman Modern
tags:
  - framework
  - pemrograman
  - plugin
  - web service
id: 193
categories:
  - Catatan
date: 2015-02-09 17:19:33
banner: /images/url.jpeg
---

### Intro ...

Tahun lalu saya pernah diminta untuk mengisi kuliah umum di Universitas Muria Kudus, saat itu materi yang saya tawarkan (dan diterima oleh panitia) adalah "_The Art of Modern Programming_". Sebenernya kalau dipikir juga nggak modern-modern amat sih, materi yang saya sampaikan sudah ada sejak lama dan sudah jamak yang menggunakan. Hanya saja saya berpikir kebanyakan mahasiswa (kuper) di kampus masih banyak yang mengenali pemrograman hanya sebatas CRUD dengan teknik konvensional, sehingga saya sepertinya teknik tersebut bakal terlihat modern &amp; menarik buat mereka.

Ada tiga materi utama yang saya sampaikan yaitu:

1.  Software Development Framework
2.  Web Service (API)
3.  dan Plugin Development

Ketiga materi tersebut menurut saya sangat penting untuk diketahui semua mahasiswa (dan selain mahasiswa) yang sedang mendalami dunia pemrograman. Meski demikian, seringkali materi tersebut tidak masuk ke dalam kurikulum perkuliahan mata kuliah apapun dengan berbagai macam alasan. Alasan pentingnya (setidaknya pengenalan pada) materi tersebut adalah untuk memudahkan kita (software developer) mengembangkan aplikasi secara lebih cepat dan lebih efisien dengan menggunakan komponen program yang telah dibuat _excelently_ oleh developer lain, sehingga kita bisa lebih berkonsentrasi pada pengembangan modul khusus yang merupakan kekuatan software kita.<!--more-->

Ketiga sub materi tersebut  akan saya tuliskan secara singkat pada artikel ini dan (_InsyaAllah_) akan saya kupas lebih dalam pada artikel lain untuk menghindari kebosanan dan tulisan yang terlalu panjang.

### 1\. Software Development Framework

Penggunaan software development framework sudah dimulai lebih dari 10 tahun yang lalu, skripsi saya tahun 2008 pun sudah bertemakan pengembangan web application framework, dan sudah banyak yang merasakan manfaat dari penggunaan framework. Tapi sampai sekarang masih banyak mahasiswa yang tidak tahu (bahkan tidak pernah mendengar) apa itu framework dan bagaimana cara menggunakannya.

Kalau anda pengguna PHP (yang bahasa pemrograman, bukan yang <del>pemberi harapan palsu</del>) ada Code Igniter, Yii, Laravel, Symfony, dll. Kalau anda fans-nya Java ada juga Spring, Struts, JFS, dll. Di bahasa pemrogram Python juga ada Django, Pyramid, Web2Py, dan Flasks. Apalagi di Ruby seharusnya sudah sangat hapal dengan yang namanya Ruby on Rails.

Semua yang disebutkan di atas adalah nama-nama Web Application Framework sesuai dengan bahasa pemrogramannya, dan kesemuanya dibuat dengan tujuan (hampir) sama, yaitu memudahkan programmer untuk men-_develop_ program. Ya, kita nggak harus belajar semua framework itu, cukup pilih satu atau dua yang paling cocok di hati untuk digunakan sebagai main arsenal saat mengembangkan aplikasi.

Meskipun jalan yang dilalui berbeda-beda, namun secara umum ada 2 fitur utama yang ditawarkan oleh hampir semua framework yaitu pemisahan logic dengan menggunakan design pattern MVC (Model-View-Controller), dan abstraksi manipulasi basis data dengan ORM (object relational mapping) atau sekedar dengan Database Access Layer. Kedua fitur tersebut akan sangat mudah dikuasai jika memang kita sudah memahami Pemrograman Berorientasi Obyek (PBO) dengan baik. Pada artikel ini saya hanya akan sedikit membahas tentang MVC saja, untuk ORM dan fitur-fitur framework yang lain akan dibahas pada artikel terpisah.

MVC yang ada pada framework akan memaksa kita memisahkan program ke dalam 3 bagian penting, yaitu Model untuk logika manipulasi data dan interaksi basis data, Controller untuk mengatur alur kerja program, dan View untuk memanipulasi tampilan. Pemisahan ini dapat mengoptimalkan reusability dan extensibilty komponen program seperti yang selama ini didengung-dengungkan saat kita belajar PBO.

Sebagai contoh perhatikan gambar ilustrasi aplikasi di bawah ini:

![MVC-Graphic](/images/MVC-Graphic.jpg)

Pada ilustrasi aplikasi tersebut kita memiliki 2 class model yaitu Articles dan Question, 2 controller, dan 4 layout. Misalkan kita ingin membuat halaman manajemen artikel yang menampilkan artikel dalam bentuk tabel maka kita tinggal memanggil fungsi getLatestArticle yang mengembalikan list of article (dalam bentuk array misalkan) dan menampilkan list tersebut dengan menggunakan table based layout pada view. Kemudian di bagian aplikasi lain kita ingin menampilkan list article yang sama dalam bentuk list (seperti pada tampilan front-end blog), yang kita lakukan kita tinggal memanggil fungsi model yang sama namun memanfaatkan div based layout yang ada pada view. Jika kita ingin melakukan hal yang sama untuk model Questions, kita tidak perlu menulis ulang semua komponen program tersebut, kita tinggal memanggil fungsi dan layout yang cocok pada controller. **_Lebih irit koding bukan?_**

Sebagai bonus, beberapa framework seperti Django, Yii, dan Ruby on Rails juga menyediakan _auto generated_ admin atau model _scaffolding_ untuk otomatisasi pembuatan halaman administrasi untuk model yang dibuat. Halaman admin yang biasanya memerlukan waktu berhari-hari untuk pembuatannya, dapat di-_generate_ secara otomatis hanya dalam waktu beberapa menit atau bahkan detik saja.

### 2\. Web Service (API)

Semakin banyak bahasa pemrograman, semakin banyak tool pemrograman dan sayangnya kebanyakan tidak kompatibel (tidak bisa berkomunikasi dengan mudah) satu sama lain. Sebagai solusinya, dibuatlah antarmuka universal yang dapat digunakan oleh sebuah aplikasi untuk berinteraksi dengan aplikasi lain, yaitu Web Service.

Gambaran dari web service adalah aplikasi menyediakan representasi data dan perintah dalam bentuk antarmuka yang mudah di-_parsing_ oleh semua bahasa pemrograman (biasanya berupa XML atau JSon). Aplikasi yang menyediakan web service menyediakan format XML/JSon yang jika dipanggil oleh program pihak ketiga akan membuat aplikasi tersebut melakukan suatu aksi. Jika aksi yang dijalankan menghasilkan data, maka data tersebut akan kembali dikirimkan pada program pihak ketiga yang memanggil (sekali lagi) dalam bentuk XML/JSon, sehingga data tersebut dapat di manipulasi oleh program pemanggil tersebut.

![Gambar diambil dari http://www.twitip.com/use-apigee-to-learn-about-the-twitter-api/](/images/ScreenshotA.jpg)

Dengan menggunakan web service, saya yang tidak ada hubungan kekeluargaan apapun dengan Twitter, bisa membuat program yang mengirimkan pesan JSon untuk meminta data siapa saja yang berteman dengan saya dan data tweet yang sudah dibuat oleh teman-teman saya tersebut. Setelah Twitter mengirimkan kembali hasil operasi dalam bentuk JSon, program yang saya buat dapat melakukan mining data untuk mengetahui barang kesukaan semua teman-teman saya. Finally, setelah program yang saya buat mengetahui informasi barang kesukaan teman-teman, program tersebut juga akan mengambil data dari Lazada untuk dipromosikan kepada semua teman saya sesuai preferensinya masing-masing.

Tanpa memanfaatkan web service, kita akan kesulitan (bener bener setengah mati) mengumpulkan sekian banyak data dari teman-teman dan sekian banyak data barang dari Lazada, dengan menggunakan web service yang perlu kita pikirkan hanya logika untuk melakukan mining data, that's all. Fungsi untuk mengambil data tweet dan data barang sudah disedikan oleh aplikasi lain, yang perlu kita lakukan hanya memanggilnya.

Saat ini ada ratusan web/aplikasi di internet yang menyediakan service yang sebelumnya harus kita buat sendiri. Sebutkanlah otentikasi pengguna dengan menggunakan Facebook, penyimpanan file dengan menggunakan Dropbox atau Google Drive, tracking lokasi dengan GoogleMap, data Cuaca dengan Yahoo Weather, dan masih banyak lagi.

### 3\. Plugin Development

Pada saat melakukan Kerja Praktek (KP), banyak sekali mahasiswa yang diminta untuk membuat WEB profil yang memiliki tambahan bla bla bla bla. Lucunya yang dilakukan mahasiswa pertama kali adalah membuat CMS baru (buatan mereka sendiri) kemudian menambahkan fitur yang diinginkan oleh tempat KP mereka. Hasilnya, karena waktu pelaksanaan KP yang terbatas, dan banyaknya pekerjaan yang harus dilakukan akhirnya tidak ada satu komponen pun yang dibuat dengan baik.

Masalahnya adalah, kenapa harus membuat CMS baru dari awal kalau sudah ada banyak CMS yang userfriendly dan sangat bagus seperti Wordpress, Joomla, Drupal, dll? Alasan mereka "Kan di CMS itu belum ada fitur bla bla bla yang diminta penyelia pak". Pertanyaan berlanjut, kenapa tidak menambahkan fitur tersebut pada CMS yang sudah matang?

CMS yang bagus seperti Wordpress, Joomla, Drupal, dll sudah merancang aplikasi mereka sebaik mungkin dengan orientasi _pluggable software design_. Artinya CMS yang sudah mereka buat dapat ditambahkan komponen baru oleh developer lain secara independen tanpa mengganggu komponen CMS yang lain. Komponen yang dapat ditambahakan bukan hanya komponen-komponen kecil pada widget blog, bahkan fitur sebesar fully managable E-Commerce dan E-Learning pun dapat dimasukkan ke dalam CMS tersebut. Ini yang seringkali tidak diketahui oleh mahasiswa.

CMS tersebut tersebut telah mengerjakan 80% dari total pekerjaan seperti autentikasi, halaman admin, manajemen database, validasi input, dan lain sebagainya. Sebagai developer kita tinggal berkonsentrasi pada 20% yang lain, yaitu fitur yang di-_request _oleh customer (penyelia KP dalam kasus tersebut). Waktu 1 hingga 3 bulan untuk menyelesaikan 20% tersebut harusnya jauh lebih dari cukup dibandingkan harus membuat semua dari awal.

### Penutup

_Does that come for free_? Tentu saja tidak, semua ada biayanya.

Kabar baiknya adalah kita tidak perlu mengeluarkan uang untuk membayar biaya tersebut. Kabar buruknya adalah kita harus membayar biaya tersebut dengan bekerja keras mempelajari hal-hal baru.

Di bangku kuliah mungkin yang diajarkan oleh dosen adalah bahasa pemrograman telanjang, tanpa variasi tambahan library atau metode apapun. Sedangkan untuk menggunakan framework kita harus belajar berbagai macam pustaka program yang disediakan masing-masing framework, belum lagi ditambah dengan aturan-aturan framework yang cukup rigid. Begitu juga untuk menggunakan web service, kita harus mempelajari pola request &amp; response yang berikan oleh provider, termasuk juga library untuk pemrograman yang kita gunakan kalau providernya berbaik hati membuatkan library pengakses untuk kita gunakan. Membuat plugin pun tidak jauh berbeda, agar terintegrasi dengan software induk, ada library dan aturan pemrograman baru yang harus diikuti.

Ya selamat datang di dunia _software development_. Saat ini bisa dibilang kalau kita tidak menggunakan komponen program milik orang lain, entah itu berupa library, plugin, atau service ya aplikasi yang kita buat hanya sampe di situ-situ aja. Sebulan penuh habis waktu untuk bikin CRUD tabel master aja, itupun belum dengan validasi inputan dan pengamanan aplikasinya. Sedangkan yang lain untuk bikin CRUD sudah bisa diselesaikan dalam beberapa hari aja. Tanpa menggunakan _reusable component_ yang sudah dibuat programmer lain, saat kita masih berkutat di input output, programmer lain sudah bikin integrasi aplikasi dengan sensor penangkap gelombang otak.

Selamat belajar hal-hal baru, dan tetap semangat mengarungi dunia pemrograman ...