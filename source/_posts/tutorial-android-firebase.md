---
title: tutorial android firebase
date: 2017-06-07 09:01:31
tags:
  - android
  - firebase
  - cloud database
categories:
  - Pemrograman
  - Android
---

Firebase merupakan *backend* yang telah diakuisisi oleh Google pada Oktober 2014 dan memiliki beberapa layanan antara lain Realtime Database, Messaging Service, Authentication Service, Storage, dan Hosting. Keunggulan dari layanan ini adalah tentu saja menggunakan server google yang pastinya reliable dan auto scale. Dan yang paling menarik dari layanan ini adalah terdapat skema gratisan yang dapat digunakan untuk mulai membangun aplikasi dari skala kecil hingga menengah.

Pada tutorial ini kita akan membuat aplikasi Android yang menggunakan database firebase untuk menyimpan data keungan secara online. Selain itu pada tutorial ini juga akan dijelaskan penggunaan firebase auth untuk otentikasi pengguna sekaligus melakukan filtering terhadap siapa saja yang dapat mengakses data yang tersimpan pada firebase DB.

<!--more-->

## Menggunakan UI Template

Untuk memudahkan tutorial, silahkan menggunakan template project yang tersedia pada [repository Github](https://github.com/kaqfa/app_template). Pada project tersebut telah tersedia 6 Activity untuk aplikasi yaitu activity untuk tampilan **Register**, **Login**, **Dashboard**, **List Transaksi**, **Form Transaksi**, dan **Detail Transaksi**. Pada tutorial ini, kita hanya akan berkonsentrasi mengkoneksikan project tersebut dengan basis data Firebase.

## Membuat Project Pada Firebase

Langkah pertama untuk menggunakan Firebase pada aplikasi kita adalah mendafatar akun pada Firebase dan Membuat Project baru (*obviously*). Jika kita sudah memiliki akun google, kita bisa menggunakan akun tersebut untuk langsung mendaftar skema gratisan pada Firebase, cukup masuk ke https://firebase.google.com/ dan klik tombol **Get Started for Free**.

Selanjutnya untuk membuat proyek baru ikuti langkah berikut:

1. Klik link **Create New Project**
2. Isikan `Manajemen Keuangan Mandiri` pada **Project Name**, dan pilih `Indonesia` pada Region, selanjutnya klik **Create Project**
3. Karena kita ingin membuat aplikasi Android, maka pilih icon Android
4. selanjutnya pada **Package name** copy-kan package aplikasi kita (berada di baris paling atas berkas `MainActivity.java`), biarkan **signing certificate** tetap kosong dan klik **Add App*

## Menambahkan Library Firebase pada Project

Setelah membuat proyek (masih dalam proses pembuatan proyek), google firebase akan memberikan instruksi untuk memasukkan konfigurasi dan library dari Firebase ke dalam aplikasi. Langkahnya adalah sebagi berikut:

1. Download file konfigurasi `google-service.json`
2. Pada Android Studio, bagian file explorer-nya kita ubah tampilannya dari default **Android** ke **Project**, sehingga gambarnya tampil sebagaimana berikut

![](/images/17_andro_to_project.png)

3. Selanjutnya copykan / drag & drop file `google-service.json` yang sudah kita download ke dalam direktori `app`.
4. Buka file `build.gradle` yang ada pada direktory **Project** dan masukkan `classpath 'com.google.gms:google-services:3.0.0'` ke bagian `dependencies`
5. Buka juga file `build.gradle` yang ada pada direktori **app** dan masukkan `apply plugin: 'com.google.gms.google-services'` pada bagian paling bawah file
6. Masih pada file yang sama, cari bagian dependencies dan tambahkan kode berikut:
```
compile 'com.google.firebase:firebase-database:9.4.0'
compile 'com.google.firebase:firebase-auth:9.4.0'
```
7. Terakhir klik perintah **sync-project** atau pilih menu **Build > Rebuild**.

Android Studio (atau lebih tepatnya Graddle) akan memerlukan beberapa waktu untuk mensingkronkan konfigurasi yang kita tambahkan dan mendownload beberapa file yang diperlukan. Pastinya semakin cepat koneksi internet yang digunakan, semakin cepat pula konfigurasi ini selesai.

## Registrasi & Auth pada Firebase

Penggunaan otentikasi dengan menggunakan Firebase memerlukan pengaktifan modul Authetication terlebih dahulu dengan cara berikut:

1. Pada Firebase Console pilih menu **Authentication** yang ada di pinggir.
2. Pilih tab **Metode Masuk** untuk mendapatkan pilihan mode otentikasi yang akan diaktifkan.
3. Kemudian aktifkan Provider **Email/Password**.

Setelah menyelesaikan semua konfigurasi dan instalasi library yang diperlukan, kita bisa langsung mulai membuat skrip registrasi dan otentikasi (login) pada proyek aplikasi Android yang sudah kita siapkan sebelumnya. Karena kita sudah membuat tampilannya, yang kita perlukan saat ini adalah menambahkan aksi ketika tombol registrasi dan login ditekan.

Mari kita membuka file `RegisterActivity.java` dan menambahkan kode berikut:

```java
public class RegisterActivity extends AppCompatActivity {

    // ...

    public void register(View view){
        FirebaseAuth mauth = FirebaseAuth.getInstance();
        EditText edtEmail = (EditText) findViewById(R.id.reg_email);
        String email = edtEmail.getText().toString();
        EditText edtPassword = (EditText) findViewById(R.id.reg_password);
        String password = edtPassword.getText().toString();
        EditText edtConfirm = (EditText) findViewById(R.id.reg_confirm);
        String confirm = edtConfirm.getText().toString();

        if (password.equals(confirm) && !password.isEmpty())
            mauth.createUserWithEmailAndPassword(email, password)
            .addOnCompleteListener(this, new OnCompleteListener<AuthResult>() {
                @Override
                public void onComplete(@NonNull Task<AuthResult> task) {
                    if(task.isSuccessful()){
                        Intent intent = new Intent(getBaseContext(), MainActivity.class);
                        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                        intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
                        startActivity(intent);
                    } else {
                        Toast.makeText(getBaseContext(),
                                task.getException().getMessage(),
                                Toast.LENGTH_SHORT).show();
                    }
                }
            });
        else {
            Toast.makeText(this, "Password tidak boleh kosong, " +
                    "harus sama dengan konfirmasi", Toast.LENGTH_SHORT);
        }
    }

    public void toLogin(View view){
        finish();
    }
}
```

Setelah selesai menuliskan kode pemrogramannya, mari kita jalankan aplikasinya dan mencoba untuk registrasi dengan menulikan email dan password yang diinginkan. Berhasil? Mari kita cek bagian authentication di Firebase Console, jika kita mendapatkan data hasil registrasi kita koding registrasi kita telah berhasil. Selanjutnya kita akan membuat koding untuk otentikasi.

Selain menyediakan API untuk registrasi pengguna, Firebase juga menyediakan otentikasi online-nya. Untuk memberikan fitur login pada aplikasi kita, tambahkan kode berikut pada `LoginActivity.java`:

```java
public class LoginActivity extends AppCompatActivity {

    // ...

    public void toRegister(View view){
        startActivity(new Intent(this, RegisterActivity.class));
    }

    public void loginAct(View view){
        EditText edtEmail = (EditText) findViewById(R.id.log_email);
        EditText edtPassword = (EditText) findViewById(R.id.log_password);

        String email = edtEmail.getText().toString();
        String password = edtPassword.getText().toString();

        FirebaseAuth auth = FirebaseAuth.getInstance();
        auth.signInWithEmailAndPassword(email, password)
                .addOnCompleteListener(new OnCompleteListener<AuthResult>() {
                    @Override
                    public void onComplete(@NonNull Task<AuthResult> task) {
                        if(task.isSuccessful()){
                            Intent intent = new Intent(getBaseContext(),
                                                    MainActivity.class);
                            intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
                            startActivity(intent);
                        } else {
                            Toast.makeText(getBaseContext(),
                                    task.getException().getMessage(),
                                    Toast.LENGTH_LONG);
                        }
                    }
                });
    }
}
```

Setelah selasai, coba untuk kembali menjalankan aplikasi dan login menggunakan email dan password yang telah kita inputkan sebelumnya. Jika kita berhasil masuk pada halaman **Dashboard** berarti kita sampai di sini kita sudah berhasil menggunakan Firebase untuk keperluan registrasi pengguna dan otentikasi. Selanjutnya kita juga perlu melakukan pengecekan pada halaman Dashboard untuk memastikan yang dapat membuka halaman tersebut adalah yang sudah terotentikasi. Jika ada pengguna yang belum melakukan otentikasi, maka tampilan akan diarahkan ke halaman Login. Untuk keperluan tersebut, tambahkan kode berikut pada method `onCreate()` di class `MainActivity`.

```java
public class MainActivity extends AppCompatActivity {

    // ...
    FirebaseAuth auth;
    FirebaseUser fUser;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...

        auth = FirebaseAuth.getInstance();
        fUser = auth.getCurrentUser();

        if (fUser == null){
            Intent intent = new Intent(this, LoginActivity.class);
            intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
            startActivity(intent);
        }

        // ...

    }
}
```

## Keamanan Akses pada Firebase

Firebase memberikan pengaturan keamanan akses data yang fleksibel sekaligus mudah dilakukan (oleh programmer) dengan berbasis konfigurasi JSON, sehingga memungkinkan kita menentukan siapa saja yang dapat mengakses data dengan mudah. Default dari pengaturan akses ini adalah berikut:

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

Konfigurasi pengaturan tersebut memastikan hanya pengguna yang telah terdaftar dan login saja yang dapat mengakses semua data. Sedangkan jika kita ingin data kita dapat diakses oleh semua orang (aplikasi) tanpa harus login terlebih dahulu, maka aturan aksesnya adalah sebagai berikut:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

Terakhir, jika kita membuat aplikasi masal dan ingin mengkonfigurasi agar pengguna hanya dapat mengakses data milik mereka sendiri, bukan data yang dibuat oleh orang lain, maka pengaturannya adalah sebagi berikut:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    }
  }
}
```

## Menambahkan data Melalui Form

Proses penambahan data pada server memerlukan pengiriman data transaksi yang kemudian oleh server di-generate-kan sebuah id khusus. Sedangkan saat ini yang kita miliki pada class Transaksi adalah pengisian id secara manual, oleh karena itu kita perlu membuat constructor tambahan yang dapat membuat object transaksi tanpa menginputkan id. Tambahkan constructor baru pada class `Transaction` sebagai berikut:

```java
public class Transaction implements Serializable, BaseColumns {
    
    // ...

    public Transaction(){
        
    }
    
    public Transaction(String name, int type, int amount, String description) {
        this.name = name;
        this.type = type;
        this.amount = amount;
        this.description = description;
    }

    // ...

}
```

Pada aplikasi sebelumnya kita sudah membuat tampilan lengkap dengan fungsi untuk menjalankan aksi sederhana. Di sini saatnya kita menambahkan data pada firebase dengan mengubah koding pada `TransactionFormActivity`:

```java
public class TransactionFormActivity extends AppCompatActivity {

    // ...

    public void saveTransaction(View view){
        FirebaseUser fUser = FirebaseAuth.getInstance().getCurrentUser();
        FirebaseDatabase db = FirebaseDatabase.getInstance();

        Transaction transaction = new Transaction(
                edtName.getText().toString(),
                spnType.getSelectedItemPosition()+1,
                Integer.parseInt(edtAmount.getText().toString()),
                edtDescription.getText().toString());

        db.getReference().child("users").child(fUser.getUid())
                         .child("transactions").push().setValue(transaction);
        finish();
    }
}
```

Setelah menambahkan koding tersebut, jalankan kembali aplikasi dan tambahkan data melalui form, selanjutnya cek apakah data yang diinputkan sudah berhasil masuk pada Firebase Console.

## Menampilkan data Pada List Transaction

Untuk menampilkan data yang tersimpan pada firebase pada List transaction, dapat kita lakukan dengan cukup merubah teknik pengambilan data pada method `onResume()`. Untuk lebih jelasnya perhatikan kode berikut:

```java
public class TransactionListActivity extends AppCompatActivity {

    // ...

    @Override
    protected void onResume() {
        FirebaseUser fUser = FirebaseAuth.getInstance().getCurrentUser();
        FirebaseDatabase db = FirebaseDatabase.getInstance();
        DatabaseReference ref = db.getReference().child("users")
                                .child(fUser.getUid()).child("transactions");

        transList = new ArrayList<>();

        adapter = new ArrayAdapter(this, android.R.layout.simple_list_item_1, transList);
        transactionList.setAdapter(adapter);

        ref.addChildEventListener(new ChildEventListener() {
            @Override
            public void onChildAdded(DataSnapshot dataSnapshot, String s) {
                Log.d("transaction.detail", dataSnapshot.getValue().toString());
                Transaction aTrans = dataSnapshot.getValue(Transaction.class);
                aTrans.setId(dataSnapshot.getKey());
                transList.add(aTrans);
                adapter.notifyDataSetChanged();
            }

            @Override
            public void onChildChanged(DataSnapshot dataSnapshot, String s) { }

            @Override
            public void onChildRemoved(DataSnapshot dataSnapshot) { }

            @Override
            public void onChildMoved(DataSnapshot dataSnapshot, String s) { }

            @Override
            public void onCancelled(DatabaseError databaseError) { }
        });

        super.onResume();
    }

    // ...
}
```

Yang perlu diperhatikan ketika melakukan operasi pada Firebase adalah perintah yang dijalankan bersifat asingkron, maksudnya ketika kita menjalankan perintah, hasilnya tidak langsung kita dapatkan dan seringkali aplikasi akan menjalankan aktifitas lain. Setelah perintah selesai dijalankan, kita perlu memanggil fungsi lain untuk memberikan *trigger* agar tampilan berubah, dalam kasus ini triggernya adalah `adapter.notifyDataSetChanged();`.

## Menghitung Statistik pada Dashboard

Sama seperti proses menampilkan data pada Transaction List, untuk menghitung statistik pada Dashboard yang kita perlukan adalah menjalankan `ChildEventListner` dan memanggil fungsi *trigger* untuk merubah tampilan ketika operasi pengambilan data sudah selesai. Untuk itu, kita perlu merubah koding method `onResume` pada `MainActivity` menjadi sebagaimana berikut:

```java
public class MainActivity extends AppCompatActivity {

    // ...

    private void updateStat(ArrayList<Transaction> transactions){
        TextView txtSaldo = (TextView) findViewById(R.id.txt_saldo);
        TextView txtKecil = (TextView) findViewById(R.id.txt_kecil);
        TextView txtBesar = (TextView) findViewById(R.id.txt_besar);

        int saldo = 0, besar = 0, kecil = 0;
        Transaction trans;

        if (transactions.size() > 0)
            besar = kecil = transactions.get(0).getAmount();

        for(int i = 0; i < transactions.size(); i++){
            trans = transactions.get(i);
            if (trans.getType() == 1){
                saldo -= trans.getAmount();
            } else {
                saldo += trans.getAmount();
            }

            if (trans.getAmount() > besar)
                besar = trans.getAmount();
            if(trans.getAmount() < kecil)
                kecil = trans.getAmount();
        }

        txtSaldo.setText(Integer.toString(saldo));
        txtBesar.setText(Integer.toString(besar));
        txtKecil.setText(Integer.toString(kecil));
    }

    @Override
    protected void onResume() {
        super.onResume();

        FirebaseDatabase db = FirebaseDatabase.getInstance();
        DatabaseReference ref = db.getReference().child("users")
                            .child(fUser.getUid()).child("transactions");

        transList = new ArrayList<>();
        updateStat(transList);

        ref.addChildEventListener(new ChildEventListener() {
            @Override
            public void onChildAdded(DataSnapshot dataSnapshot, String s) {
                Transaction aTrans = dataSnapshot.getValue(Transaction.class);
                aTrans.setId(dataSnapshot.getKey());
                transList.add(aTrans);
                updateStat(transList);
            }

            @Override
            public void onChildChanged(DataSnapshot dataSnapshot, String s) { }

            @Override
            public void onChildRemoved(DataSnapshot dataSnapshot) { }

            @Override
            public void onChildMoved(DataSnapshot dataSnapshot, String s) { }

            @Override
            public void onCancelled(DatabaseError databaseError) { }
        });
    }

    // ...
}
```

## Menampilkan Detail Data & Menghapus Data

Penghapusan data pada Firebase lebih baiknya dilakukan berdasarkan identifier yang unik. Oleh karena itu seperti ditampilkan pada koding di bawah ini, kita telah memasukkan id dari data transaksi kita ke dalam atribut `id` yang ada pada `class Transaction`.

```java
// ...
public void onChildAdded(DataSnapshot dataSnapshot, String s) {
    Transaction aTrans = dataSnapshot.getValue(Transaction.class);
    aTrans.setId(dataSnapshot.getKey());
    transList.add(aTrans);
    adapter.notifyDataSetChanged();
}
// ...
```

Selanjutnya untuk menghapus data dari tampilan Detail Transaksi, kita hanya perlu menjadikan id tersebut sebagai referensi dan menghapus nilainya. Lebih jelasnya berikut ada kode yang perlu ditambahkan pada `TransactionDetailActivity`:

```java
public class TransactionDetailActivity extends AppCompatActivity {

    // ...

    public void delTransaction(View view){
        FirebaseUser fUser = FirebaseAuth.getInstance().getCurrentUser();
        FirebaseDatabase db = FirebaseDatabase.getInstance();
        DatabaseReference ref = db.getReference().child("users").child(fUser.getUid())
                                    .child("transactions").child(transaction.getId());
        ref.removeValue();
        finish();
    }
}
```