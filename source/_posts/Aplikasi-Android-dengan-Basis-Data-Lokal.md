---
title: Aplikasi Android dengan Basis Data Lokal
date: 2017-06-05 15:32:20
tags:
  - android
  - sqlite
  - koding
categories:
  - Pemrograman
  - Android
---

Tutorial ini akan menggunakan contoh aplikasi manajemen keuangan pribadi. Aplikasi ini merupakan aplikasi sederhana yang terdiri dari beberapa bagian penting yaitu:

- **Dashboard**: berisi statistik dan informasi singkat tentang kondisi keuangan
- **List Transaksi**: catatan transaksi pemasukan dan pengeluaran yang telah dilakukan
- **Form Transaksi**: digunakan untuk menambahkan data catatan transaksi
- **Detail Transaksi**: digunakan untuk menampilkan data detail dari tiap transaksi dan tombol untuk menghapus transaksi tersebut
<!--more-->
## Inisialisasi Aplikasi

Langkah pertama yang dilakukan dalam pengembangan aplikasi adalah membuat proyek dengan Android Studio. Berikut adalah langkah pembuatan proyek untuk aplikasi kita:

1. Pilih menu **New Apllication**
![New Application](/images/01_welcome_AS.png)
2. Selanjutnya isikan **Application Name** dengan `Pengurus Uang` dan **Domain Name** dengan `app.manajemenuang.net`. Jangan lupa untuk memilih alamat direktori sesuai dengan yang kita inginkan.
![](/images/02_create_new_project.png)
3. Selanjutnya kita akan diminta untuk memilih template dari **Activity** yang akan digunakan, di sini sebaiknya kita gunakan **Basic Activity** untuk memudahkan pembuatan aplikasi nanti.
![](/images/04_select_activity.png)
4. Terakhir biarkan nama activity utamanya `MainActivity` begitu juga nama file layoutnya `activity_main.xml`, Kemudian klik finish.
![](/images/05_name_activity.png)

Pada tahap ini kita sudah memiliki kerangka aplikasi yang sudah siap di-*compile* dan dijalankan. Untuk menguji coba aplikasi dasar tersebut, kita bisa mengklik tombol Run. Selanjutnya Android Studio akan memerlukan beberapa saat untuk melakukan kompilasi, membuka AVD (jika belum dijalankan sebelumnya) dan mengirim hasil aplikasi yang kita buat ke Device atau Virtual Device yang dipilih.

![](/images/06_run_app.png)

Ketika proses kompilasi selesai, kita akan mendapatkan hasil sebagaimana ditunjukkan pada gambar di bawah ini. Biarkan AVD tetap berjalan (untuk menghemat waktu starting AVD) dan kita lanjutkan ke langkah pengembangan proyek selanjutnya.

![](/images/09_hello_world.png)

## Membuat Activity & Layout

Setelah project dibuat dan bisa berjalan dengan baik, saatnya kita menambahkan komponen lain ke dalam project kita. Rencananya aplikasi yang akan kita buat memiliki beberapa tampilan dengan workflow sebagaimana ditampilkan pada gambar di bawah ini.

![](/images/diagram.png)

Berdasarkan gambar tersebut, berarti kita memerlukan paling tidak 4 buah antarmuka tampilan yang dalam pemrograman Android disebut sebagai Activity. Saat ini kita sudah mendapatkan 1 tampilan sebagai halaman utama dan halaman yang pertama kali dibuka ketika aplikasi dijalankan, berarti kita memerlukan tambahan 3 Activity lagi.

### TransactionListActivity

1. Klik kanan pada direktori java project, kemudian pilih menu **New > Activity > Empty Activity**
![](/images/08_create_activity.png)
2. Pada inputan **Activity Class** inputkan `TransactionListActivity`, dan pastikan pada **Layout file** bernilai `activity_transaction_list.xml`
![](/images/10_TransactionListActivity.png)
3. Kemudian setelah Android Studio selesai men-*generate* file activity java dan layout, isikan kode layout berikut pada pada file `activity_transaction_list.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="id.co.uang.urusanuang.TransactionListActivity">

    <ListView
        android:id="@+id/list_transactions"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</RelativeLayout>
```

### TransactionFormActivity

1. Klik kanan pada direktori java project, kemudian pilih menu **New > Activity > Empty Activity**
2. Pada inputan **Activity Class** inputkan `TransactionFormActivity`, dan pastikan pada **Layout file** bernilai `activity_transaction_form.xml`
3. Kemudian setelah Android Studio selesai men-*generate* file activity java dan layout, isikan kode layout berikut pada pada file `activity_transaction_form.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="15dp"
    android:orientation="vertical"
    tools:context="us.firda.fahri.pengurusuang.TransactionFormActivity">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Nama Transaksi" />

    <EditText
        android:id="@+id/edt_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Jenis Transaksi" />

    <Spinner
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/spn_type"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Jumlah" />

    <EditText
        android:id="@+id/edt_amount"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:inputType="number"/>


    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Keterangan" />

    <EditText
        android:id="@+id/edt_description"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:inputType="text" />

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Simpan"
        android:onClick="saveTransaction"/>
</LinearLayout>
```

Khusus untuk form, karena kita menggunakan Spinner (dropdown selection) kita harus menambahkan kode berikut pada `TransactionFormActivity` agar dapat berjalan dengan baik:

```java
public class TransactionFormActivity extends AppCompatActivity {

    Spinner spnType;
    EditText edtName;
    EditText edtAmount;
    EditText edtDescription;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_transaction_form);

        spnType = (Spinner) findViewById(R.id.spn_type);
        edtName = (EditText) findViewById(R.id.edt_name);
        edtAmount = (EditText) findViewById(R.id.edt_amount);
        edtDescription = (EditText) findViewById(R.id.edt_description);

        String type [] = {"Pengeluaran", "Pemasukan"};
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_spinner_item, type);
        adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
        spnType.setAdapter(adapter);
    }
}
```

### TransactionDetailActivity

1. Klik kanan pada direktori java project, kemudian pilih menu **New > Activity > Empty Activity**
2. Pada inputan **Activity Class** inputkan `TransactionDetailActivity`, dan pastikan pada **Layout file** bernilai `activity_transaction_detail.xml`
3. Kemudian setelah Android Studio selesai men-*generate* file activity java dan layout, isikan kode layout berikut pada pada file `activity_transaction_detail.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_margin="15dp"
    android:orientation="vertical"
    tools:context="us.firda.fahri.pengurusuang.TransactionDetailActivity">

    <TextView
        android:id="@+id/txt_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        android:textSize="24sp"
        android:textStyle="bold"
        android:textAlignment="center"
        android:textColor="@color/colorPrimaryDark"
        android:text="Nama Transaksi" />

    <LinearLayout
        android:layout_marginTop="10dp"
        android:layout_marginBottom="10dp"
        android:padding="10dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <TextView
            android:id="@+id/txt_type"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:textSize="18sp"
            android:textStyle="bold"
            android:text="Pengeluaran"/>

        <TextView
            android:id="@+id/txt_amount"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="18sp"
            android:layout_weight="1"
            android:textStyle="bold"
            android:textAlignment="textEnd"
            android:layout_gravity="end"
            android:text="20.000"/>
    </LinearLayout>

    <TextView
        android:id="@+id/txt_description"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:padding="10dp"
        android:text="Deskripsi agak panjang dari transaksi yang dilakukan"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="delTransaction"
        android:text="Hapus Transaksi"/>

</LinearLayout>
```

### MainActivity

Khusus untuk `MainActivity` kita tidak perlu membuatnya, karena dari awal sudah dibuatkan oleh Android Studio saat kita membuat proyek. Yang perlu dilakukan adalah menambahkan koding pada `content_main.xml` menjadi seperti di bawah ini:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:context="us.firda.fahri.pengurusuang.MainActivity"
    android:orientation="vertical"
    android:layout_margin="15dp"
    tools:showIn="@layout/activity_main">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Aplikasi Manajemen Keuangan"
        android:textAlignment="center"
        android:textSize="30sp"
        android:textStyle="bold" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Saldo Terakhir"
        android:textAlignment="center"
        android:textSize="18sp"
        android:textStyle="bold"
        android:layout_marginTop="20dp"/>

    <TextView
        android:id="@+id/txt_saldo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="2.500.000"
        android:textAlignment="center"
        android:textSize="24sp"
        android:textStyle="bold" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Pengeluaran Terkecil"
        android:textAlignment="center"
        android:textSize="18sp"
        android:textStyle="bold"
        android:layout_marginTop="20dp"/>

    <TextView
        android:id="@+id/txt_kecil"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="2.500.000"
        android:textAlignment="center"
        android:textSize="24sp"
        android:textStyle="bold"
        tools:layout_editor_absoluteX="8dp"
        tools:layout_editor_absoluteY="299dp" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Pengeluaran Terbesar"
        android:textAlignment="center"
        android:textSize="18sp"
        android:textStyle="bold"
        android:layout_marginTop="20dp"/>

    <TextView
        android:id="@+id/txt_besar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="2.500.000"
        android:textAlignment="center"
        android:textSize="24sp"
        android:textStyle="bold"
        tools:layout_editor_absoluteX="8dp"
        tools:layout_editor_absoluteY="223dp" />

    <Button
        android:id="@+id/btn_list_transactions"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Tampilkan Semua Transaksi"
        android:layout_marginTop="20dp" />

</LinearLayout>
```

Agar aplikasi dapat melakukan navigasi ke activity lain dengan baik, tambahkan kode berikut pada `MainActivity`:

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {

        // ...

        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                // hapus semua code di dalam fungsi ini, dan ganti dengan kode berikut
                Intent intent = new Intent(getBaseContext(), TransactionFormActivity.class);
                startActivity(intent);
            }
        });

        // tambahkan juga kode ini
        Button btnListTransactions = (Button) findViewById(R.id.btn_list_transactions);
        btnListTransactions.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(getBaseContext(), TransactionListActivity.class);
                startActivity(intent);
            }
        });
    }

    // ...
}
```

## Membuat Model

Langkah selanjutnya setelah kita membuat tampilan untuk aplikasi adalah membuat class model yang merupakan representasi dari tabel basis data SQLite yang akan kita gunakan. Pada proyek ini kita akan menyimpan data transaksi berupa nama transaksi, jenis transaksi (pemasukan/pengeluaran), jumlah, dan juga keterangan.

Class model yang dimaksud adalah standard class Java yang diberikan attribute dan method untuk keperluan manipulasi data. Langkah pembuatannya adalah sebagai berikut:

1. Klik kanan pada direktori Java kemudian pilih menu **New > Class**
![](./images/12_new_java_class.png)
2. Isikan `Transaction` pada inputan **Class Name**, biarkan inputan lain, dan klik **Finish**
![](./images/13_create_class_name.png)
3. Selanjutnya pada `Transaksi.java` isikan koding berikut:

```java
public class Transaction implements Serializable {

    public static final int PENGELUARAN = 1;
    public static final int PEMASUKAN = 1;

    private String id;
    private String name;
    private int type;
    private int amount;
    private String description;

    public Transaction(String id, String name, int type, int amount, String description) {
        this.id = id;
        this.name = name;
        this.type = type;
        this.amount = amount;
        this.description = description;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getType() {
        return type;
    }

    public void setType(int type) {
        this.type = type;
    }

    public int getAmount() {
        return amount;
    }

    public void setAmount(int amount) {
        this.amount = amount;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String stringType(){
        if (this.type == PEMASUKAN)
            return "Pemasukan";
        else
            return "Pengeluaran";
    }

    @Override
    public String toString() {
        return this.name+" ["+this.stringType()+"] : "+this.amount;
    }
}
```

## Menampilkan Transaksi dengan ListView dan DetailView

Setelah membuat `class Transaksi` saatnya kita membuat ListView untuk menampilkan data *dummy* (bukan data beneran). Untuk melakukannya kita harus menambahkan fungsi atribut `ArrayList<Transaksi> listTrans` dan method `void dummyData()` pada `class TransactionListActivity` seperti ditunjukkan pada koding berikut:

```java
class TransactionListActivty extends AppCompatActivity{

    private ArrayList<Transaction> transList = new ArrayList<>();

    // ...

    private void dummyData(){
        transList.add(new Transaction("1", "Makan malam",
                Transaction.PENGELUARAN, 20000, "Makan malam sendirian"));
        transList.add(new Transaction("2", "Action Figure",
                Transaction.PENGELUARAN, 1200000, "Action figure Sumanto"));
        transList.add(new Transaction("3", "Gaji Bulanan",
                Transaction.PEMASUKAN, 10000000, "Gaji reguler tiap bulan"));
        transList.add(new Transaction("4", "Bonus Lembur",
                Transaction.PEMASUKAN, 5000000, "Lemburan penelitian bersama"));
        transList.add(new Transaction("5", "Oleh-oleh kampung",
                Transaction.PENGELUARAN, 2000000, "Beli oleh-oleh buat keluarga"));
    }
}
```

Setelah menambahkan attribute dan method tersebut, kita perlu memanggil method `dummyData` pada method `ocCreate()` sebagaimana ditunjukkan pada koding di bawah ini:

```java
class TransactionListActivty extends AppCompatActivity{

    // ...

    ArrayAdapter adapter;
    ListView transactionList;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...

        transactionList = (ListView) findViewById(R.id.list_transactions);

        dummyData();
        adapter = new ArrayAdapter(this, android.R.layout.simple_list_item_1, transList);
        transactionList.setAdapter(adapter);
    }

    // ...
}
```

### Menampilkan Detail Satu Data Pada Detail View

Setelah data ditampilkan pada ListView, sekarang kita akan menampilkan data lengkap sebuah transaksi dengan menggunakan DetailView. Untuk melakukannya yang pertama kali dilakukan adalan menambahkan aksi ketika ada salah satu item list yang disentuh (atau diklik) dengan menambahkan `onItemClickListener` pada listview:

```java
class TransactionListActivty extends AppCompatActivity{

    // ...

    public void onCreate(){
        // ...

        transactionList.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Intent intent = new Intent(getBaseContext(),
                        TransactionDetailActivity.class);
                intent.putExtra("transaction.detail", transList.get(position));
                startActivity(intent);
            }
        });
    }

    // ...
}
```

Terakhir kita juga harus mengubah `TransactionDetailActivity` untuk dapat menerima data dan menampilkan sesuai dengan layout yang telah dibuat sebelumnya:

```java
public class TransactionDetailActivity extends AppCompatActivity {

    private Transaction transaction;
    private TextView txtName;
    private TextView txtType;
    private TextView txtAmount;
    private TextView txtDescription;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_transaction_detail);

        txtName = (TextView) findViewById(R.id.txt_name);
        txtType = (TextView) findViewById(R.id.txt_type);
        txtAmount = (TextView) findViewById(R.id.txt_amount);
        txtDescription = (TextView) findViewById(R.id.txt_description);

        Intent intent = getIntent();
        transaction = (Transaction) intent.getSerializableExtra("transaction.detail");

        txtName.setText(transaction.getName());
        txtType.setText(transaction.stringType());
        txtAmount.setText(Integer.toString(transaction.getAmount()));
        txtDescription.setText(transaction.getDescription());
    }
}
```

## Menambahkan Contract & DBHelper

Kita telah berhasil menampilkan data uji coba pada listView, selanjutnya kita tampilan tersebut kita ganti dengan koneksi ke database lokal.

Pada Android koneksi database lokal menggunakan **SQLite** yang merupakan database lokal tanpa server (atau istilahnya adalah *embedded database*). Karena modelnya berbeda dengan basis data berbasis server (serperti MySQL), model koneksi dan manipulasi SQLite juga berbeda. Kongkritnya dalam pemrograman Android, untuk dapat melakukan akses SQLite kita perlu menyiapkan 2 hal, yaitu class Database Contract dan class Database Helper.

### Class Database Contract

Contract class adalah Java Class standar yang digunakan untuk mendefinisikan konstanta dan beberapa query yang berhubungan dengan pembentukan dan pengaksesan basis tabel. Untuk membuat table contract untuk aplikasi yang kita kerjakan, kita bisa memodifikasi class model `Transaction` yang sudah kita buat sebelumnya menjadi seperti di bawah ini:

```java
class Transaction implements Serializable, BaseColumns {
    // ...

    public static final String TABLE_NAME = "transactions";
    public static final String COL_NAME = "name";
    public static final String COL_TYPE = "type";
    public static final String COL_AMOUNT = "amount";
    public static final String COL_DESCRIPTION = "description";

    public static final String SQL_CREATE = "create table transactions " +
                                    "(_id integer primary key autoincrement, "+
                                    "name text, type integer, amount integer, description text)";
    public static final String SQL_DELETE = "drop table if exists transactions";
}
```

### Class untuk Database Helper

Class kedua yang harus kita siapkan untuk akses database adalah class Helper, class ini berfungsi untuk mengeksekusi perintah-perintah SQL baik yang berhubungan dengan struktur tabel maupun yang berhubungan dengan data tabel. Class Helper yang kita gunakan untuk aplikasi kita adalah berikut:

```java
public class DatabaseHelper extends SQLiteOpenHelper {

    public static final String DB_NAME = "keuangan.db";
    public static final int DB_VERSION = 1;

    private SQLiteDatabase db;

    public DatabaseHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(Transaction.SQL_CREATE);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL(Transaction.SQL_DELETE);
        onCreate(db);
    }

    public void insertTransaction(String name, int type, int amount, String description){
        db = getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put(Transaction.COL_NAME, name);
        values.put(Transaction.COL_TYPE, type);
        values.put(Transaction.COL_AMOUNT, amount);
        values.put(Transaction.COL_DESCRIPTION, description);

        db.insert(Transaction.TABLE_NAME, null, values);
    }

    public List<Transaction> getTransactions(){
        db = getReadableDatabase();
        List<Transaction> transactions = new ArrayList<>();

        Cursor cursor = db.rawQuery("select * from "+Transaction.TABLE_NAME+
                                    " order by "+Transaction._ID, null);

        Transaction newTrans;
        try{
            while (cursor.moveToNext()){
                newTrans = new Transaction(Long.toString(cursor.getLong(0)), cursor.getString(1),
                        cursor.getInt(2), cursor.getInt(3), cursor.getString(4));
                transactions.add(newTrans);
            }
        } finally {
            cursor.close();
        }

        return transactions;
    }

    public void deleteTransaction(Transaction transaction){
        db = getWritableDatabase();

        db.delete(Transaction.TABLE_NAME, Transaction._ID+" = ?",
                new String[]{transaction.getId()});
    }
}
```

## Menggabungkan Semuanya pada Aplikasi

Setelah menyelesaikan kedua class untuk membantu koneksi basis data, selanjutnya kita akan menggunakan class tersebut pada aplikasi kita. Pertama kita akan menggunakannya untuk menginputkan data melalui Form, kemudian menampilkan data melalui MainActivity dan ListView, dan terakhir menggunakannya untuk menghapus data pada tampilan detail.

### Input Data Melalui Form

Pada bagian pembentukan activity kita sudah membuat desain untuk form-nya, sehingga yang perlu dilakukan saat ini adalah menghubungkan tampilan form tersebut ke dalam logic Java-nya. Berikut adalah modifikasi class `FormTransactionActivity` untuk menyimpan data ke dalam basis data lokal.

```java
public class TransactionFormActivity extends AppCompatActivity {

    Spinner spnType;
    EditText edtName;
    EditText edtAmount;
    EditText edtDescription;

    // ...

    public void saveTransaction(View view){
        String name = edtName.getText().toString();
        int type = spnType.getSelectedItemPosition()+1;
        int amount = Integer.parseInt(edtAmount.getText().toString());
        String description = edtDescription.getText().toString();

        DatabaseHelper database = new DatabaseHelper(this);
        database.insertTransaction(name, type, amount, description);

        Toast.makeText(this, "Transaksi "+name+" berhasil disimpan", Toast.LENGTH_SHORT).show();
        finish();
    }
}
```

### Membaca Data dan Menampilkan Pada Main

Setelah menginputkan data di form, langkah selanjutnya adalah menampilkan 5 data terbaru di Dashboard dan menampilkan semua data di ListView. Karena kita telah membuat layout pada bagian sebelumnya, dan bahkan menampilkan list dengan menggunakan ArrayList, pada bagian ini kita hanya perlu mengganti bagian ArrayList untuk mengambil data dari basis data.

```java
public class MainActivity extends AppCompatActivity {

    DatabaseHelper dbHelper = new DatabaseHelper(this);
    ArrayList transList;

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

        transList = (ArrayList<Transaction>) dbHelper.getTransactions();
        updateStat(transList);
    }

}

```

### Menampilkan semua data pada ListView

Untuk menampilkan semua data pada ListView, kita perlu membuat method baru untuk mengambil data dari database dan memanggilnya pada saat Activity dibuat. Koding lengkapnya ditunjukkan di bawah ini:

```java
public class TransactionListActivity extends AppCompatActivity {

    private ArrayList<Transaction> transList = new ArrayList<>();
    ArrayAdapter adapter;
    ListView transactionList;

    // ...

    @Override
    protected void onResume() {
        super.onResume();
        DatabaseHelper dbHelper = new DatabaseHelper(this);
        transList = (ArrayList<Transaction>) dbHelper.getTransactions();
        adapter = new ArrayAdapter(this, android.R.layout.simple_list_item_1, transList);
        transactionList.setAdapter(adapter);
    }
}
```

### Menghapus Data Pada Detail

Proses penghapusan data pada detail memerlukan `_id`, untuk memudahkan kita akan menambahkan atribut `trans_id` untuk menyimpan data tersebut dan fungsi `hapus` yang dipanggil ketika tombol hapus ditekan diisi dengan kode berikut:

```java
public class TransactionDetailActivity extends AppCompatActivity{

    // ...

    public void delTransaction(View view){
        DatabaseHelper dbHelper = new DatabaseHelper(this);
        dbHelper.deleteTransaction(transaction);
        finish();
    }
}
```

## Membuat Activity Login & Registrasi

Step by Step membuat halaman login dan registrasi, termasuk peringatan jika login gagal.

### Membuat Halaman Registrasi dan Halaman Login



## Otentikasi Sederhana Aplikasi

Membuat DBHelper User untuk registrasi dan login aplikasi

## Menambahkan Tabel dan Relasi Tabel

Mengubah model Transaksi dan DB Helper untuk relasi sehingga user hanya melihat transaksi yang dia buat saja