---
title: S.Kom (yang) Lebih dari Lulusan SMK
tags:
  - lulusan IT
  - pemrograman
  - rekayasa perangkat lunak
id: 263
categories:
  - Catatan
  - Inilah Hidup
date: 2015-10-20 10:52:50
---

Tulisan ini saya tujukan untuk mahasiswa yang sedang belajar di S1 jurusan ilmu komputer (terutama Teknik Informatika) dan juga HRD instansi yang sedang membandingkan kualifikasi kompetensi lulusan S1 bidang komputer dan lulusan SMK jurusan komputer (atau RPL). Disclaimer yang harus disadari saya (penulis) adalah dosen Teknik Informatika, sehingga tidak aneh kalau akan lebih mengunggulkan lulusan S1 komputer dibandingkan dengan sekedar lulusan SMK. Tapi bukankah memang itu yang kita harapkan, bertambah waktu belajar bukankah semakin pinter. Pertanyaannya adalah, lebih pinternya itu lebih pinter bagian apanya, skill apa yang dimiliki lulusan S1 yang lebih dari skill lulusan SMK?

Tidak bisa dipungkiri kemampuan anak SMK saat ini bisa dibilang sudah cukup mumpuni untuk bekerja secara langsung di bidang IT (pengembangan _Software_, manajemen jaringan, bahkan menjadi _Hacker_). Banyak anak-anak SMK yang sudah pinter bikin program dan setting server, bahkan banyak juga yang sudah mengerjakan proyek IT berbayar. Dari situ akhirnya banyak juga perusahaan yang lebih suka mempekerjakan lulusan SMK karena bayarannya lebih murah namun sudah memiliki skill yang cukup.<!--more-->

Apakah benar sudah cukup?

Oke kita bicaranya perlu mundur lagi kembali ke pertanyaan "skill apa yang dimiliki lulusan S1 yang lebih dari skill lulusan SMK?".

Kalau dulu dosen saya bilang, lulus S.Kom bidang RPL lebih baik dari lulusan SMK bidang RPL karena lulusan "S1 mampu membuat software yang sesuai dengan kaidah-kaidah pengembangan software yang benar, mulai dari pengumpulan kebutuhan, pembuatan desain aplikasinya, hingga pengujiannya". Saya tidak tahu dengan anda, tapi menurut saya jawaban tersebut masih terlalu abstrak. Karena saat saya kejar "memangnya kalau lulusan SMK bikinnya nggak sesuai kaidah ya pak?", jawabannya adalah "ya sebagian sudah ikut, tapi masih kurang sesuai ...". Absurd kan?

> Di sini saya nggak akan bercerita tentang bidang selain rekayasa perangkat lunak, karena itu bukan bidang saya. Nggak sopan rasanya kalau saya ikutan bahas yang bukan bidang saya.

Sekarang saya sudah menjadi dosen dan sudah bisa menjawab lebih detail pertanyaan tersebut. Ada beberapa skill yang (harusnya) dimiliki oleh lulusan S.Kom, namun by default tidak diajarkan di SMK. Beberapa diantaranya adalah:

1.  _Loosely coupled software design_
2.  Penerapan kecerdasan buatan pada  software
3.  _Multi Processing dengan Thread hingga Distributed System_
4.  dan lain sebagainya

Bagaimana, semakin absurd? Ya itu tujuan saya biar anda tidak berhenti sampai di sini membacanya.

### 1\. Loosely Coupled Software Design

Bahasa indonesianya perancangan perangkat lunak yang berhubungan secara renggang. Komponen perangkat lunak yang baik seharusnya mudah diintegrasikan dengan perangkat lunak yang lain, namun tidak terhubung secara erat dalam arti jika suatu saat komponen tersebut ingin dimasukkan pada perangkat lunak lain, developer tinggal melakukan _coplok tempel_.

Contoh mudahnya, perusahaan AB ingin membuat aplikasi Sisfo Inventori untuk perusahaannya. Tentu saja pada aplikasi tersebut ada komponen login dan validasi otorisasi pengguna untuk memastikan yang dapat mengakses aplikasi tersebut hanya pegawai yang berhak saja. Kemudian di lain waktu perusahaan tersebut juga ingin membuat aplikasi Sisfo Keuangan dengan model login dan validasi otorisasi pengguna yang kurang lebih sama dengan aplikasi Sisfo Inventori. Desain perangkat lunak yang bagus memungkinkan developer untuk men-coplok komponen login dari Sisfo Inventori dan menempelkannya pada aplikasi Sisfo Keuangan, meskipun daftar usernya berbeda. Sedangkan desain perangkat lunak yang kurang bagus, memaksa developer untuk membuat komponen tersebut dari awal lagi (atau minimal melakukan copy-paste-edit-debug) untuk menerapkan komponen login yang sama pada aplikasi yang berbeda.

Contoh komponen login saya gunakan karena (menurut saya) paling mudah dipahami, tentu saja komponen aplikasi di sebuah perusahaan tidak sesederhana komponen login. Nah skill yang harusnya dimiliki oleh lulusan S.Kom adalah mampu menganalisis proses bisnis global dan memecahnya ke dalam beberapa komponen yang independent untuk mempermudah penggunaan kembali pada kasus aplikasi yang berbeda. Penguasaan terhadap model desain ini akan mempercepat pengembangan software dan sekaligus meningkatkan kualitas dari software itu sendiri.

Saya ingin membahas topik ini lebih mendalam di postingan lain. Semoga tidak lupa, _InsyaAllah_.

### 2\. Penerapan Kecerdasan Buatan pada _Software_

Software (dan hardware) yang menggunakan kecerdasan buatan sudah banyak di sekitar kita, hanya saja sering kita tidak menyadari karena terlalu pandainya pengembang menyembunyikan kerumitan kecerdasan buatan tersebut. Konteks kecerdasan buatan yang saya maksud di sini, bukan hanya terbatas pada robot Asimo, algoritma PC Game, dan digital asisten seperti Siri saja, namun juga penerapan kecerdasan buatan pada timeline facebook, pencarian koleksi pada digital library, dan juga widget iklan yang sering kita lihat di web favorit.

Jika kita hanya ngomong menyimpan data posting dan menampilkannya di timeline teman, struktur dan teknologi facebook bukanlah hal yang susah untuk dibuat. Masalahnya adalah di facebook saya punya ribuan teman dan ratusan teman diantaranya sangat aktif meng-update status. Jika semua postingan tersebut tampil di halaman saya, pastinya timeline saya penuh banget. Nah yang susah dari teknologi facebook adalah logika melakukan filter untuk hanya menampilkan postingan yang menurut facebook menarik bagi saya. Itu adalah salah satu bentuk kecerdasan buatan yang rumit namun terlihat sederhana.

Nah lulusan S.Kom di Indonesia pastinya pernah mendapatkan mata kuliah yang berbau kecerdasan buatan seperti _Data Mining_, _Machine Learning_, _Image Processing_, atau _Information Retrieval_. Sehingga ketika perusahaan ingin mengembangkan software yang memerlukan komponen kecerdasan buatan, misalkan aplikasi untuk mensortir dokumen lama dan secara otomatis mengelompokkannya ke dalam kategori tertentu, atau membuat aplikasi rekomendasi penentuan harga sebuah produk berdasarkan tren harga pasar, maka pilihan yang tepat adalah mempekerjakan lulusan S.Kom.

### 3\. _Multi Processing_ dengan _Thread_ hingga _Distributed System_

Sebuah perusahaan menengah dan besar saat ini tidak bisa lepas dengan urusan data skala besar terutama yang berhubungan dengan data transaksional pembelian harian dan manajemen dokumen. Selain itu, aplikasi modern saat ini tidak bisa dilepaskan dari proses berat yang harus dijalankan secara kontinyu namun tidak boleh mengganggu misalkan mengirimkan email newsletter berkala ke customer, atau secara berkala mengecek informasi dari web kompetitor untuk memastikan strategi yang kita pilih tidak salah.

Dalam kondisi seperti itu aplikasi konvensional yang hanya mengandalkan satu processor akan menyebabkan aplikasi yang dibuat berjalan sangat lambat. Aplikasi harus dibagi ke dalam beberapa bagian dan disebar pengerjaannya ke dalam beberapa thread, processor, atau bahkan ke dalam beberapa server terpisah. Konsep ini yang kemudian dikenal dengan multiprocessing dan distributed system.

Saat ini memang tidak semua S1 ilmu komputer di Indonesia mengajarkan secara aplikatif dan praktis skill pemrograman dengan multiprocessing dan distributed system, namun semakin hari adopsi terhadap kemampuan tersebut sudah semakin banyak dilakukan. Ini juga merupakan salah satu skill pembeda lulusan S1 dengan lulusan SMK.

### Penutup

Sampai di sini menurut saya sudah cukup jelas apa perbedaan skill yang dimiliki oleh lulusan S1 komputer (RPL) dengan lulusan SMK RPL. Kalau masih belum jelas mungkin kita harus diskusi lewat komentar atau _japri_.

Nah sedikit permasalahan di sini (dan ini permasalahan klasik). Apakah semua lulusan S1 sudah memenuhi standar tersebut? Jawabannya tentu saja sangat relatif dengan kemauan dan kemampuan masing-masing lulusan. Dan sayangnya seringkali bahkan kemampuan hard skill susah untuk diukur hanya dengan ukuran IPK.

Untuk mahasiswa yang merasa sudah hampir lulus tapi masih belum menguasai salah satu kemampuan di atas, berarti ada yang salah dengan anda. Mungkin tidak sepenuhnya kesalahan anda, mungkin salah di kurikulum, di dosen, atau di pergaulan anda sehari-hari. Jika anda merasa ini kesalahan dosen (yang tidak pernah menyampaikan tersebut di kelas), kejarlah dosen di luar kelas, belajar mandiri sambil berdiskusi dan brainstorming dengan dosen favorit anda adalah merupakan cara yang terbaik untuk belajar di Universitas. Yang membedakan orang yang belajar secara otodidak dan mahasiswa salah satunya adalah karena adanya bimbingan dari dosen, manfaatkanlah itu sebaik mungkin. Jangan sampai anda baru tersadar setelah lulus dan akhirnya kalah bersaing.

Selanjutnya sekedar informasi untuk HRD instansi, apakah mempekerjakan lulusan SMK sudah cukup atau perlu mempekerjakan S1? Ya semuanya bergantung pada kebutuhan perusahaan anda. Jika yang diinginkan adalah developer yang sekedar bisa bikin software input data dan keluar laporan, mungkin lulusan SMK sudah cukup. Namun jika perusahaan juga mementingkan kualitas software yang dikembangkan baik dari validitas aplikasi maupun ketahanan aplikasi terhadap keamanan, perubahan data secara signifikan. Maka lulusan S1 merupakan pilihan yang lebih tepat untuk dipekerjakan.