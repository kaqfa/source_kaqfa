---
title: Array dan ArrayList dalam Java
tags:
  - array
  - arraylist
  - collection
  - java
id: 89
categories:
  - Akademis
  - Pemrograman Berorientasi Obyek
date: 2015-08-24 16:48:01
banner: /images/java.png
thumbnail: /images/java.png
---

Sama seperti bahasa pemrograman lain, bahasa pemrograman java menyediakan cara untuk mengelompokkan variabel dengan tipe data sejenis dengan menggunakan Array. Hanya saja, dalam pemrograman modern tipe data array sering dirasa kurang fleksibel karena harus mendeklarasikan jumlah data di awal. Di dalam java, permasalahan tersebut diselesaikan dengan menggunakan kelas Collection. Pada artikel ini akan dibahas pengelompokan tipe data (primitif &amp; class) dengan cara konvensional menggunakan Array, dan cara yang lebih dinamis dengan menggunakan Collection (dalam artikel ini lebih spesifik ArrayList).
<!--more-->

### Array Dalam Java

Untuk dapat menggunakan array dalam java, tahap pertama yang harus dilakukan adalah deklarasi dengan menggunakan cara sebagai berikut :

<pre>tipe-data identifier [] = new tipe-data[jumlah-data];</pre>

*   **tipe-data: ** tipe yang ingin digunakan dan dapat berupa Class.
*   **identifier: ** nama variabel yang ingin dijadikan array. Tambahan "[]" untuk memberitahu compiler bahwa variabel tersebut adalah array.
*   **new [jumlah-data]: ** perlu dibedakan antara kata kunci `new` yang digunakan pada array, dan `new` yang digunakan pada instantiasi class. Pada array `new` diikuti dengan tipe data dan jumlah data yang diapit kurung kotak "[]", sedangkan pada instantiasi class, `new` diikuti nama class dan kurung biasa "()" (jika terdapat parameter, perlu ditambahkan juga).

Setelah deklarasi, tahap selanjutnya adalah menggunakan array tersebut dengan mengakses identifier-nya menggunakan indeks tertentu. Contoh deklarasi dan penggunaan array adalah sebagai berikut:

```java
public class ArrayDemo
{
    public static void main(String args[]){
        int nilai [] = new int[5];
        String nama [] = new String[5];

        nilai[0] = 10;
        nilai[4] = 15;
        nilai[2] = 200;

        nama[1] = "Ahmad";
        nama[0] = "Andri";
        nama[3] = "Anis";

        System.out.println("Isi dari nilai[0] adalah: "+nilai[0]);
        int jumlah = nilai[4] + nilai[2];
        System.out.println("Jumlahnya adalah: "+jumlah);

        System.out.println("Nama pertama adalah: "+nama[0]);
        System.out.println("Nama keempat adalah: "+nama[3]);
        System.out.println("Nama kedua adalah: "+nama[1]);
    }
}
```

Jika program tersebut dijalankan, maka akan menghasilkan output sebagai berikut:

```
Isi dari nilai[0] adalah: 10
Jumlahnya adalah: 215
Nama pertama adalah: Andri
Nama keempat adalah: Anis
Nama kedua adalah: Ahmad
```

Lebih lanjut tentang array dalam Java dapat dibaca pada :

1.  [Java Arrays (tutorialspoint)](http://www.tutorialspoint.com/java/java_arrays.htm "Java Arrays (tutorialspoint)")
2.  [Introduction to Programming in Java (Array)](http://introcs.cs.princeton.edu/java/14array/ "Introduction to Programming in Java (Array)")

### Menggunakan Collection ArrayList

Seperti dijelaskan sebelumnya, kelemahan dari Array konvensional adalah ukurannya harus sudah ditentukan pada waktu deklarasi. Jika tipe data Array ini digunakan untuk menyimpan data yang ukurannya tidak dapat ditentukan, tentunya akan sangat repot untuk pendeklarasiannya. Mulai dari Java versi 5 (kalau nggak salah), Java menambahkan library khusus untuk dapat menyimpan kumpulan tipe data dengan ukuran yang dinamis, yaitu Collection.

Sebenarnya ada beberapa Class yang masuk dalam Collection ini, antara lain `LinkedList`, `ArrayList`, `HashSet`, `TreeSet`, dan lain sebagainya. Pada artikel ini, implementasi tipe Collection yang akan dijelaskan hanya ArrayList saja.

Sama seperti array (dan semua jenis variabel yang lain), ada dua tahap penggunaan ArrayList, yaitu deklarasi dan penggunaan. Cara pendeklarasian ArrayList adalah sebagai berikut:

```java
import java.util.ArrayList;
// ... ... ...
ArrayList  identifier = new ArrayList();
```

*   **import ...: ** untuk dapat menggunakan ArrayList kita harus mengimport dari `java.util.ArrayList`, jika tidak compiler tidak akan mengenali class tersebut.
*   **ArrayList: ** tipe data yang digunakan juga bisa berupa Class.
*   **new ArrayList(): ** karena ArrayList adalah sebuah class, maka `new` pada bagian ini bisa kita anggap sebagai instantiasi. Yang di-instantiasikan adalah ArrayList dengan tipe data yang diinginkan.

Setelah dideklarasikan ArrayList, kita dapat menambahkan data pada ArrayList dengan menggunakan `indentifier.add(data)` dan mendapatkan data yang telah disimpan dengan menggunakan `indentifier.get(indeks)`. Cukup mudah bukan?

Ukuran dari ArrayList akan berkembang dan menyusut sesuai dengan data yang dimasukkan. Jika kita ingin mengetahui jumlah data yang telah dimasukkan ke dalam ArrayList, kita dapat menggunakan perintah `indentifier.size()`. Contoh aksi ArrayList dalam sebuah program Java dapat dilihat pada koding di bawah ini:

```java
import java.util.ArrayList;

public class ArrayListDemo
{
    public static void main(String [] args){
        ArrayList nilai = new ArrayList();
        ArrayList nama = new ArrayList();

        nilai.add(5);
        nilai.add(20);
        nilai.add(14);
        nilai.add(90);

        nama.add("Kamto");
        nama.add("Sri");
        nama.add("Narto");
        nama.add("Siti");
        nama.add("Andarini");
        nama.add("Sukma");

        int jumlah = nilai.get(0)+nilai.get(2);
        System.out.println("Nilai ke 1 ditambah nilai ke 3 = "+jumlah);

        System.out.println("Nama ke 3 adalah "+nama.get(2));
        System.out.println("Nama ke 5 adalah "+nama.get(4));

        System.out.println("Jumlah nama yang dimasukkan adalah "+nama.size());

        System.out.println("Semua nama yang dimasukkan adalah:");
        for(int i = 0; i &lt; nama.size(); i++){
            System.out.println("Nama ke "+(i+1)+" adalah "+nama.get(i));
        }
    }
}
```

Jika program tersebut dijalankan, maka hasilnya adalah seperti di bawah ini :

```
Nilai ke 1 ditambah nilai ke 3 = 19
Nama ke 3 adalah Narto
Nama ke 5 adalah Andarini
Jumlah nama yang dimasukkan adalah 6
Semua nama yang dimasukkan adalah:
Nama ke 1 adalah Kamto
Nama ke 2 adalah Sri
Nama ke 3 adalah Narto
Nama ke 4 adalah Siti
Nama ke 5 adalah Andarini
Nama ke 6 adalah Sukma
```

Masih banyak fitur dari ArrayList dan Colection yang belum saya bahas di sini, termasuk diantaranya penggunaan foreach pada Collection. Jika ingin mempelajari Collection dan ArrayList lebih lanjut dapat membaca link berikut:

1.  [Java - The ArrayList Class](http://www.tutorialspoint.com/java/java_arraylist_class.htm "Java - The ArrayList Class")
2.  [10 example of using ArrayList in Java](http://javarevisited.blogspot.com/2011/05/example-of-arraylist-in-java-tutorial.html "10 example of using ArrayList in Java")

Oke, sepertinya cukup sekian dulu penjelasan tentang kumpulan tipe data / obyek dalam Java. Semoga cukup jelas dan dapat bermanfaat. Jika ada pertanyaan lebih lanjut, bisa ditanyakan di bagian komentar.