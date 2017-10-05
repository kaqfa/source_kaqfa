---
title: Tutorial Git & Github (Command Prompt)
date: 2017-09-15 15:17:58
tags:
	- git
	- github
	- command prompt
---

Pada postingan sebelumnya saya telah menjelaskan [pentingnya penggunaan Github untuk programmer](http://fahrifirdaus.web.id/2015/02/github-curriculum-vitae-untuk-programmer/), selain itu saya juga sudah pernah menjelaskan [cara menggunakan Github dengan Github Desktop](http://fahrifirdaus.web.id/2015/09/tutorial-github-dasar-windows/). Nah pada tutorial ini saya ingin menjelaskan bagaimana cara menggunakan Github dengan menggunakan *command prompt*.

Pada tutorial ini saya akan menggunakan sebuah IDE keren bernama Android Studio untuk memberikan contoh pembuatan dan perubahan koding sebuah aplikasi. Sebagai sebuah IDE dengan status keren, Android Studio pastinya sudah memiliki fitur Git & Github Integration. Namun seperti yang saya jelaskan di atas, pada tutorial ini fitur tersebut tidak akan digunakan dan malah akan menggunakan antarmuka yang sangat sederhana yaitu antarmuka console (atau di windows disebut dengan *command prompt*).<!--more-->

### Langkah 1. Membuat Project di Android Studio

Berbeda dari beberapa tutorial lain, kita tidak memulai dengan Git dan Github, tapi kita akan mulai dari membuat project dulu. Alasannya adalah, kita akan mengasumsikan diri kita sendiri sebagai orang yang sedang bertaubat, ingin *move on* dari aliran sesat yaitu coding tanpa menggunakan CVS, menuju ke jalan yang lebih lurus yaitu menggunakan Github untuk repository dan versioning code.

Langkah pertama ini boleh diganti dengan membuat project pemrograman apapun dengan bahasa pemrograman apapun dan IDE apapun jika dirasa Android Studio terlalu berat. Atau bahkan jika kita pengen ngirit, boleh saja pake Editor seperti SublimeText, Atom, atau bahkan Notepad++. Selain itu karena tutorial ini bukanlah tutorial Android, maka saya akan menjelaskan langkah pertama ini secara cepat:

1. Buka Android Studio
2. Pilih menu `Start new Android Studio project`
3. Berikan isian `Demo Github` pada nama aplikasi, dan langsung klik tombol `next`
4. Centang pilihan `Phone & Tablet` serta pilih minimum SDK `API 16: Android 4.1 (Jelly Bean)` kemudian klik lagi tombol `next`
5. Biar terlihat keren, pilih saja `Navigation Drawer Activity` pada pilihan Activity-nya kemudian klik tombol `next`
6. Terakhir biarkan saja inputan yang ada, dan klik tombol `Finish`

Tunggu beberapa saat hingga Android Studio selesai membuatkan struktur minimal dari project android yang telah kita pilih. Setelah selesai, boleh-boleh saja kita menekan tombol `Run` untuk menjalankan aplikasi Android minmalis yang telah kita buat untuk dijalankan di *Emulator* atau *Real Device*.

### Langkah 2. Membuat repository baru di Github

Oke di tengah perjalanan membuat aplikasi Android yang sangat keren, tiba-tiba kita teringat kalau kita belum menyimpan source code aplikasi tersebut di Github. Jadi langkah selanjutnya adalah membuat repositori baru di Github (diasumsikan semua udah bikin akun Github ya). Langkahnya adalah:

1. Pastikan saat ini lokasi kita ada di Dashboard github, kemudian klik tombol `New Repository` sebagaimana di bawah ini.
2. Masukkan nama repositori yang kita inginkan (hanya menerima karakter dash `-` dan underscore `_`).
3. Inputan deskripsi bersifat opsional, jadi boleh diisi atau dikosongkan.
4. Klik tombol `Create Repository`, dan kita akan mendapatkan hasil sebagaimana di bawah ini

### Langkah 3. Instalasi Git client

Git client merupakan aplikasi desktop yang digunakan untuk membuat repository lokal sekaligus berinteraksi dengan hosted git seperti Github. Ada banyak pilihan Git client, termasuk diantaranya adalah Github for Windows seperti yang pernah dijelaskan pada [tutorial saya sebelumnya](http://fahrifirdaus.web.id/2015/09/tutorial-github-dasar-windows/). Kali ini Git client yang kita gunakan adalah Git client buatan dari Git sendiri yang bisa didownload pada [link ini](git-scm.com).

Seperti halnya dengan instalasi kebanyakan aplikasi Windows lain, kita cukup klik tombol `next` beberapa kali, kemudian klik tombol `Finish` di akhirnya.

### Langkah 4. Membuat repository lokal dan menghubungkan dengan Github

Langkah selanjutnya adalah membuat repositori lokal pada project android yang telah kita buat dan menghubungkannya dengan akun Github yang telah kita buat. Caranya buka console / command prompt kemudian ketikkan perintah berikut baris per baris:

```
> cd /path/to/android/project
> git init
> git add -A
> git set global user.name "fahri firdausillah"
> git set global user.email "elfaatta@gmail.com"
> git remote add origin https://github.com/repository-anda
> git commit -m "catatan penyimpanan"
> git push set origin master
```

1. Perintah baris pertama untuk memastikan kita berada pada folder project android kita 
2. Perintah baris kedua berguna untuk membuat repositori baru pada folder saat ini
3. Sedangkan baris perintah ketiga untuk menambahkan semua file yang telah kita buat pada stagging area
4. Perintah baris ke 4 dan 5 hanya dijakanlan jika ini adalah pertama kali kita melakukan operasi git (setelah instalasi git client), perintah tersebut digunakan untuk mengeset nama dan email yang diasosiasikan dengan pemakaian git
5. Perintah baris ke 6 Untuk menyimpan perubahan di repository lokal
6. Sedangkan baris perintah ke 7 digunakan untuk menghubungkan repositori lokal kita dengan repositori github
7. Terakhir baris perintah ke 8 digunakan untuk meng-upload semua pekerjaan kita ke Github

Setelah menjalankan baris perintah ke 8 kita akan diminta memasukkan username & password github untuk otentikasi. Kemudian berhasil menjalankan proses itu semua, maka kita bisa mengecek perubahannya di repository github kita.

### Langkah 5. Melakukan perubahan dan mengupload perubahan source code

Langkah panjang yang kita lakukan pada langkah ke 4 hanya dilakukan pada saat pertama kali menghubungkan repositori lokal dengan repositori Github. Selanjutnya jika kita membuat perubahan apapun kita cukup menjalankan tiga baris perintah berikut pada console:

```
> git add -A
> git commit -m "catatan commit"
> git push
```

Ketiga perintah tersebut berarti adalah menambahkan hasil pekerjaan ke stagging area, menyimpan perubahan pada repositori lokal, dan mengupload perubahan ke Github. Ketiga perintah ini kemungkinan adalah perintah git yang paling sering kita eksekusi.

### Mengecek status git

Sebagai tambahan proses, jika kita ingin mengecek status repositori git lokal kita saat ini (misalkan untuk mencari tahu apakah perubahan sudah di-commit atau belum), kita dapat menjalankan perintah `git status` (tentu saja) pada direktori repositori kita.

Adapun jika kita ingin mengecek saat ini repository lokal kita terhubung ke mana saja, kita dapat menggunakan perintah `git remote -v`.

### Penutup

Cukup sudah tutorial paling dasar penggunaan github dengan menggunakan command prompt. Tentu saja alur kerja pengembangan software dengan menggunakan Git sebagai version control-nya tidak hanya berhenti sampai di situ saja. Beberapa proses lain antara lain **branching**, **merging**, **conflict resolution**, **rollback**, dan lain sebagainya. Di bawah ini adalah beberapa sumber tambahan yang bisa dijadikan referensi untuk mempelajari lebih lanjut penggunaan Git dan Github untuk version control.

*Wallahu A'lam*