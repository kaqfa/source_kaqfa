---
title: Compile Aplikasi Android ke Smartphone
date: 2017-09-18 13:05:10
categories:
	- akademik
	- pemrograman
	- android
tags:
	- android
	- compile
	- smartphone
	- tutorial
---

Pemrograman Android merupakan salah satu pemrograman yang memerlukan spek komputer (atau laptop) yang cukup tinggi. Berdasarkan referensi dari situs resminya, Android Studio yang merupakan *de-facto* IDE untuk pengembangan aplikasi Android memerlukan RAM minimal 3 GB untuk menjalankannya, meskipun untuk pemakaian secara stabil sebaiknya minimal RAM yang digunakan adalah 8 GB.

Meski demikian, rata-rata spesifikasi laptop yang beredar di pasaran memiliki kapasitas RAM bawaan sebesar 4 GB. Hasilnya ketika kita dijalankan Android Studio akan bekerja cukup lambat, belum lagi ketika melakukan kompilasi aplikasi dan dijalankan di emulator maka komputer akan bekerja sangat lambah sekali, atau malah berhenti bekerja. Sebagai alternatifnya untuk mengurangi beban kompilasi dan penggunaan emulator untuk uji coba aplikasi, kita dapat menggunakan real device (smartphone) untuk menjalankan aplikasi yang sedang dikembangkan.<!--more-->

Pada tutorial ini akan dijelaskan cara menggunakan smartphone Android untuk uji coba perangkat lunak Android yang dikembangkan pada sistem operasi Windows. Berikut langkah-langkahnya:

### Install USB Driver melalui SDK Manager

1. Klik menu `Tools` > `Android` > `SDK Manager`
2. Pilih tab `SDK Tools`
3. Cari pilihan `USB Driver` kemudian centang
4. Klik `Apply` untuk menjalankan perintah download

Tunggu beberapa saat hingga download proses download selesai.

### Install Driver ADB Interface melalui Windows Device Manager

1. Ketikkan `devmgmt.msc` pada run box windows
2. Klik pada `Other Devices` untuk mencari `ADB Interface` yang belum terinstall sempurna (ditunjukkan icon warning)
3. Klik kanan pada `ADB Interface` dan pilih menu `Update Driver`
4. Klik pilihan `Browse my computer for driver software`
5. Klik `browse` dan arahkan ke folder `USB driver` yang ada pada direktori instalasi Android SDK
6. Klik `next` hingga driver Android anda terinstall dengan baik

### Aktifkan USB Debugging pada Smartphone

1. Masuk ke dalam menu Settings smartphone Android anda
2. Carilah menu `Additional setting` > `Developer options`, jika menu tersebut tidak ada lanjutkan ke langkah nomer 4.
3. Centang / aktifkan pilihan `USB Debugging` pada group `Debugging` pada menu developer options tersebut.
4. Jika menu `Developer options` tidak ada, maka kita perlu mengaktifkannya terlebih dahulu dengan masuk ke menu `About Phone` atau `About Device`
5. Cari menu `Build Number` dan ketuk 7 kali. 
6. Khusus pada smartphone Xiaomi, cari `MIUI version` dan ketuk 7 kali
7. Setelah ada info developer mode telah aktif, kembali ke menu Settings serta jalankan langkah kedua dan seterusnya.

### Jalankan aplikasi Android dengan Smartphone

1. Seperti biasa klik icon `Run` atau menu `Run` > `Run 'app'`
2. Pada jendela `Select Deployment Target` pastikan smartphone android anda sudah terhubungan dengan komputer / atau laptop dan terdeteksi pada pilihan `Connected Device`
3. Pilih device yang diinginkan, kemudian klik `OK`

Tunggu beberapa saat hingga Android Studio menyelesaikan kompilasi dan menginstall aplikasi di smartphone kita.

Dengan menggunakan smartphone sebagai sarana uji coba saat mengembangkan aplikasi Android, kita sedikit merasa lega karena tidak harus mengupgrade laptop / komputer untuk dapat membuat (atau belajar membuat) aplikasi Android. Meski demikian, jika memungkinkan, sebaiknya tetap usahakan untuk mengupgrade RAM pada komputer menjadi minimal 8 GB, agar performa meningkat serta kesabaran dan kewarasan tetap terjaga.