---
title: Console Input Menggunakan Scanner (Java)
tags:
  - input console
  - java
  - Scanner
id: 97
categories:
  - Akademis
  - Pemrograman Berorientasi Obyek
date: 2014-04-07 10:45:13
---

Console input adalah cara program mendapatkan input langsung dari pengguna dengan menggunakan Command Prompt (istilah windows) atau Console (istilah Linux). Meskipun di pasaran bisa dibilang kita tidak pernah lagi ketemu dengan program yang melakukan input dengan menggunakan console (command prompt), namun untuk tujuan belajar dan testing aplikasi input melalui console masih banyak digunakan.

Semenjak versi 1.5, java memberikan cara input console yang lebih mudah (dibandingkan versi sebelumnya), yaitu dengan menggunakan class Scanner yang ada pada `java.util.*;`. Untuk dapat menggunakan class tersebut ada dua tahap yang perlu dilakukan, yaitu instantiasi object Scanner dan pemanggilan method input yang sesuai. <!--more-->Cara penggunaanya seperti ditunjukkan pada koding di bawah ini:

```java
import java.util.Scanner;
// ... ... ...
Scanner input = new Scanner(System.in);
input.nextLine();
input.next();
input.nextInt();
input.nextDouble();
// ... ... ...
```

Penjelasan program:

*   **import ...: ** untuk mengimport library Scanner dari `java.util`.
*   **Scanner input: ** deklarasi class Scanner ke dalam variabel input (tentu saja nama variabel boleh diubah yang lain).
*   **new Scanner(System.in): ** instantiasi object Scanner dengan parameter System.in.
*   **input.next ...: ** memerintahkan program untuk menerima input dari console dan mengonversinya ke dalam tipe data yang sesuai. Jika yang diinginkan adalah String maka bisa menggunakan method `nextLine()`, jika ingin input integer dapat menggunakan `nextInt()`, jika ingin input data desimal dapat menggunakan `nextDouble()` dan seterusnya.
Oke, saatnya melihat aksi class Scanner dalam program input sederhana:

```java
public class DemoInput
{
    public static void main(String [] args){
        Scanner input = new Scanner(System.in);                

        System.out.print("Inputkan Nama : ");
        String nama = input.nextLine();

        System.out.print("Inputkan Nilai 1 : ");
        int nilai1 = input.nextInt();
        System.out.print("Inputkan Nilai 2 : ");
        int nilai2 = input.nextInt();

        System.out.print("Penambahan dari nilai 1 dan nilai 2 : "+(nilai1+nilai2));
        System.out.println("Nama anda adalah: "+nama);
    }
}
```

Untuk mencobanya, tentu saja kita harus compile dan jalankan aplikasi tersebut.

Demikian tutorial singkat input console dengan menggunakan class Scanner pada Java. Semoga bermanfaat.