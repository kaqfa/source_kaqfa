---
title: Input dan Event Handling pada Android
id: 104
categories:
  - Catatan
tags:
---

Kecuali kita ingin terus-terusan membuat aplikasi HelloWorld, cepat atau lambat kita akan perlu menangani inputan dari pengguna atau sekedar berinteraksi dengan pengguna melalui event-event tertentu. Beberapa tutorial mungkin akan memisahkan artikel ini menjadi dua bagian, yaitu Input Handling dan Event Handling. Namun karena saya ingin menjadikan tutorial ini implementable dan mudah diikuti, saya putuskan untuk menuliskannya menjadi satu tutorial saja.

Aplikasi yang akan saya gunakan sebagai contoh pada artikel ini adalah aplikasi kalkulator sederhana dengan 2 teks input dan satu menu pilihan dropdown. Tampilan dari aplikasi adalah seperti ditunjukkan gambar di bawah ini.
[![kalkulator1](http://fahrifirdaus.web.id/wp-content/uploads/2014/04/kalkulator1-192x300.jpg)](http://fahrifirdaus.web.id/wp-content/uploads/2014/04/kalkulator1.jpg)

### State Awal Aplikasi

Saya tidak ingin kembali membahas masalah tampilan, jika anda ingin mempelajari lebih tentang bagaimana mendesain tampilan pada aplikasi android menjadi seperti itu, silahkan cari tutorial di Google (atau di blog ini kalau sudah ada). Tapi karena saya baik hati, saya akan tetap menuliskan file layout dan potongan koding class yang digunakan untuk menampilkan tampilan.

Tampilan layout tersebut, dibentuk melalui desain XML sebagai berikut :
<pre>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".LinearDemo" >   

    <TextView
        android:id="@+id/text1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Input angka 1" />

    <EditText
        android:id="@+id/angka1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10" />

    <TextView
        android:id="@+id/text2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Input angka 2" />

    <EditText
        android:id="@+id/angka2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10" />

    <Spinner
        android:id="@+id/operator"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <Button
        android:id="@+id/btnHitung"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hitung" />

    <TextView
        android:id="@+id/txtHasil"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="___"
        android:layout_marginTop="50dp"
        android:textStyle="bold"
        android:gravity="center" 
        android:textSize="30dp"/>

</LinearLayout>
</pre>

Sedangkan untuk mengisi list pada Spinner, kita juga perlu menambahkan kode berikut pada Activity-nya:
<pre>
Spinner spinOp;
EditText angka1, angka2;
TextView txtHasil;

@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_linear_demo);

	spinOp = (Spinner) findViewById(R.id.operator);
	ArrayList<String> ops = new ArrayList<String>();

	ops.add("Tambah");
	ops.add("Kurang");
	ops.add("Kali");
	ops.add("Bagi");

	ArrayAdapter<String> dtAdapter = new ArrayAdapter<String>(this, android.R.layout.simple_spinner_item, ops);
	dtAdapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);		

	spinOp.setAdapter(dtAdapter);
}
</pre>

### Event Handling (firing onClick)

Sebenarnya ada beberapa cara event handling yang dapat digunakan untuk menangani event onClick sebuah komponen, di sini saya hanya akan mencontohkan cara paling mudah saja, yaitu dengan menggunakan pemanggilan method pada layout.

Pada aplikasi calculator yang sudah dibuat layoutnya sudah bisa ditebak, yang akan diberikan event handling adalah pada komponen button dengan event onClick. Dengan menggunakan pemanggilan method pada layout caranya menjadi sangat straightforward, yaitu tambahkan saja atribut android:onClick pada komponen button sehingga koding dari komponen tersebut menjadi seperti di bawah ini:
<pre>
<Button
    android:id="@+id/btnHitung"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="Hitung"
    android:onClick="operasiHitung" />
</pre>

Nilai "operasiHitung" pada atribut android:onClick (baris 6) adalah nama method yang akan dipanggil saat komponen button tersebut diklik.

Langkah selanjutnya adalah menambahkan method `operasiHitung(View v){ ... }` pada class activity-nya. Koding lengkap dari method `operasiHitung() `adalah sebagai berikut:
<pre>
public void operasiHitung(View v){
	double nilai1, nilai2, hasil;
	String operator;

	angka1 = (EditText) findViewById(R.id.angka1);
	angka2 = (EditText) findViewById(R.id.angka2);

	nilai1 = Double.parseDouble(angka1.getText().toString());
	nilai2 = Double.parseDouble(angka2.getText().toString());

	spinOp = (Spinner) findViewById(R.id.operator);
	operator = spinOp.getSelectedItem().toString();

	if(operator.equalsIgnoreCase("tambah")){
		hasil = nilai1 + nilai2;
	} else if(operator.equalsIgnoreCase("kurang")){
		hasil = nilai1 - nilai2;
	} else if(operator.equalsIgnoreCase("kali")){
		hasil = nilai1 * nilai2;
	} else {
		hasil = nilai1 / nilai2;
	}

	txtHasil = (TextView) findViewById(R.id.txtHasil);
	txtHasil.setText(Double.toString(hasil));	
}
</pre>

Oke karena sudah ketahuan jeroan method `operasiHitung()` yang terdiri dari beberapa input handling, maka kita sudah harus masuk ke pembahasan selanjutnya, yaitu input handling.

<h3>Input Handling<h3>