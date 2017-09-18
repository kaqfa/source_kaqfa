---
title: Koneksi Aplikasi Android dan MySQL dengan RESTful Service (2)
date: 2017-06-14 10:37:36
tags:
  - android
  - mysql
  - rest
  - web service
categories:
  - Pemrograman
  - Android
---

Tutorial ini menyambung dari tutorial sebelumnya tentang pengembangan aplikasi Android yang terkoneksi dengan basis data MySQL di server. Pada bagian pertama, kita sudah membahas bagaimana membuat service untuk aplikasi keuangan pribadi kita, dan pada bagian kedua ini kita akan membuat aplikasi Android-nya.

<!--more-->

### Clone Template Aplikasi

Aplikasi Android yang kita kembangkan di sini akan menggunakan template aplikasi yang sudah tersedia di [repository github saya ini](https://github.com/kaqfa/app_template). Tujuannya agar kita tidak perlu ribet membuat tampilan yang sudah kita pelajari di tutorial sebelumnya. Jika anda memiliki git di komputer, anda bisa melakukan *clone* terhadap aplikasi dengan menggunakan perintah berikut:

```
git clone https://github.com/kaqfa/app_template.git
```

Jika anda tidak memiliki aplikasi git, anda bisa mendownload template tersebut melalui [di sini](https://github.com/kaqfa/app_template/archive/master.zip).

### Mempersiapkan Android Volley

Koneksi ke web service dapat dilakukan dengan menggunakan pustaka jaringan bawaaan dari Android, namun pustaka tersebut sayangnya agak susah digunakan dan juga memiliki beberapa kekurangan. Sebagai alternatifnya, di sini kita akan menggunakan pustaka Android Volley untuk memudahkan kita terkoneksi dengan REST service yang telah kita buat.

Sebelum dapat menggunakan pustaka Volley, kita harus menambahkannya dependency berikut pada file `build.gradle` yang level `Module`:

```
compile 'com.android.volley:volley:1.0.0'
```

Selain itu, karena nantinya aplikasi ini akan terkoneksi dengan jaringan internet, maka kita juga perlu menambahkan permission khusus pada `AndroidManifest.xml` sebagai berikut:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Letakkan kode permission tersebut di luar tag `<application></application>`.

### EndPoint Class

Langkah selanjutnya dalam pengembangan aplikasi adalah membuat class baru bernama `EndPoint` yang digunakan untuk menyimpan URL dari web service yang akan kita tuju.

```java
public class EndPoint {
    public static final String HOST = "http://10.0.2.2/backend_keuangan/index.php";
    public static final String REGISTER_URL = HOST+"/register";
    public static final String LOGIN_URL = HOST+"/login";
    public static final String TRANSACTION_URL = HOST+"/transactions";
}
```

Alamat *host* yang kita gunakan saat ini adalah `10.0.2.2`, alamat tersebut adalah server localhost yang diakses melalui AVD virtual device. Artinya ketika kita sudah memindahkan service tersebut ke hosting, maka alamat tersebut akan digantikan dengan alamat hosting kita nantinya.

### Proses Register

Setelah menyelesaikan proses konfigurasi, saatnya mengedit activity untuk aplikasi manajemen keuangan. Activity pertama yang akan diedit adalah `RegisterActivity.java`. Proses ini menangani pendaftaran pengguna baru dengan menerima input email dan password kemudian mengirimkan data ke server untuk didaftarkan. Jika proses pendaftaran berjalan dengan baik maka tampilan akan diarahkan kembali ke tampilan Login, dan jika tidak akan menampilkan pesan kesalahan.

Perintah untuk proses tersebut terkumpul pada fungsi `register(View view)` yang akan dieksekusi ketika tombol registrasi diklik. Koding untuk proses tersebut adalah sebagaimana ditampilkan di bawah ini.

```java
public class RegisterActivity extends AppCompatActivity {

    // ...

    public void register(View view){

        String email = txtEmail.getText().toString();
        String password = txtPassword.getText().toString();
        String confirm = txtConfirm.getText().toString();

        HashMap<String, String> data = new HashMap<>();
        data.put("email", email);
        data.put("password", password);

        if(password.equals(confirm)){

            JsonObjectRequest request = new JsonObjectRequest(EndPoint.REGISTER_URL,
                    new JSONObject(data),
                    new Response.Listener<JSONObject>() {
                        @Override
                        public void onResponse(JSONObject response) {
                            Log.d("register.resp", response.toString());
                            Intent intent = new Intent(getBaseContext(), LoginActivity.class);
                            startActivity(intent);
                        }
                    },
                    new Response.ErrorListener(){
                        @Override
                        public void onErrorResponse(VolleyError error) {
                            Toast.makeText(getBaseContext(), "Register Error", Toast.LENGTH_SHORT);
                            error.printStackTrace();
                        }
                    }
            );

            RequestQueue rq = Volley.newRequestQueue(this);
            rq.add(request);

        } else {
            Toast.makeText(this, "Password dan Konfirmasi harus sama", Toast.LENGTH_SHORT).show();
            txtPassword.selectAll();
        }
    }

    // ...
}
```

Baris ke 7 hingga 9 adalah perintah untuk mendapatkan nilai inputan dan menyimpannya ke dalam variabel untuk digunakan pada proses selanjutnya. Selanjutnya pada baris 11 - 13 kita menggunakan `HashMap` untuk mempersiapkan data dari form tadi dikirimkan ke server dengan format **object JSON**.

Perintah `JSONObjectRequest` pada baris 21 merupakan class yang dari pustaka Volley yang digunakan untuk melakukan request ke web service dengan mengirimkan data dalam bentuk JSON object dan mengharapkan *response* dalam bentuk JSON object juga. *Class* tersebut biasa digunakan untuk *method* POST, sehingga kita tidak perlu secara eksplisit *method* yang digunakan.

Empat parameter yang diperlukan oleh *class* `JSONObjectRequest` adalah alamat URL untuk parameter pertama, sedangkan parameter kedua menggunakan tipe data `HashMap` serta digunakan untuk input data yang akan dikirimkan ke server. Adapun parameter ketiga dan keempat memerlukan object `Response.Listener` dan `Response.ErrorListener` untuk menetukan aksi apa yang harus dilakukan ketika mendapatkan *response* dari web service.

### Proses Login

Setelah menyelesaikan proses registrasi, saatny kita memodifikasi `LoginActivity` untuk melakukan handling terhadap proses login. Proses ini bisa dibilang sedikit lebih rumit dibandingkan proses lain, karena selain melakukan *request* dan *response* pada server, proses ini juga bertanggung jawab untuk menyimpan **token** dan **uid** pada memori handphone, dan juga melakukan pengecekan apakah sudah ada pengguna yang login pada aplikasi atau tidak. Berikut adalah koding untuk `LoginActivity.java`.

```java
public class LoginActivity extends AppCompatActivity {

    public static final String AUTH_SESSION = "PERF_AUTH_SESSION";
    SharedPreferences preferences;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_login);

      preferences = getSharedPreferences(AUTH_SESSION, Context.MODE_PRIVATE);
      String token = preferences.getString("token", null);
      if(token != null){
          Intent it = new Intent(this, MainActivity.class);
          it.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
          it.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
          startActivity(it);
      }
      // ...
    }

    public void saveToken(int uid, String token){
      SharedPreferences.Editor editor = preferences.edit();

      editor.putString("token", token);
      editor.putInt("uid", uid);
      editor.commit();
    }

    public void sendRequest(){
      String email = edtEmail.getText().toString();
      String password = edtPassword.getText().toString();
      HashMap<String, String> param = new HashMap<>();
      param.put("email", email);
      param.put("password", password);
      JsonObjectRequest request = new JsonObjectRequest(EndPoint.LOGIN_URL,
          new JSONObject(param), new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
              int userid = 0;
              String token = "0";
              Log.d("response.login", response.toString());
              try {
                userid = response.getInt("uid");
                token = response.getString("token");
              } catch (JSONException e) {
                e.printStackTrace();
              }

              if(userid > 0){
                saveToken(userid, token);
                Intent it = new Intent(getBaseContext(), MainActivity.class);
                it.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
                it.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                startActivity(it);
              } else {
                Toast.makeText(getBaseContext(), "Username / Password salah",
                               Toast.LENGTH_SHORT).show();
              }
            }
          },
          new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
              Log.d("json.response", error.getMessage());
            }
          }
      );
      RequestQueue rq = Volley.newRequestQueue(this);
      rq.add(request);
  }

  public void loginAct(View view) {
    sendRequest();
  }

  // ...
}
```

Proses yang pertama harus dilakukan adalah mengecek apakah sudah ada user yang login atau tidak. Kalau pada aplikasi berbasis web biasanya kita menggunakan Session atau Cookies, sedangkan pada Android kita bisa menggunakan `SharedPreferences` untuk menyimpan data sederhana. Inisialisasi variable `preferences` dan juga pengecekan pengguna aktif dilakukan pada method `onCreate`, karena tentu saja method tersebut adalah method yang pertama kali dijalankan dalam *Life Cycle* sebuah *activity*.

Langkah selanjutnya, sebelum melakukan pengiriman *request* ke web service, kita menyiapkan method `saveToken` terlebih dahulu untuk persiapan penyimpanan **token** dan **uid** yang nantinya kita dapatkan dari web service. Selanjutnya baru kita buat fungsi `sendRequest` yang bertugas untuk mengirimkan *request* ke server dengan menggunakan class `JsonObjectRequest` seperti halnya ketika proses registrasi. Meski demikian ketika class tersebut mendapatkan *response* terdapat kemungkinan kombinasi **email** dan **password** yang diberikan tidak tepat (diindikasikan dengan *response* **uid** bernilai 0). Jika hal tersebut terjadi, maka aplikasi akan memberikan peringatan kesalahan **username** & **password** menggunakan `Toast`. Adapun jika login berhasil (diindikasikan dengan *response* **uid** bernilai lebih dari 0), maka aplikasi akan memanggil method `saveToken` untuk menyimpan token ke dalam `SharedPreferences` dan juga mengarahkan aplikasi ke `MainActivity`.

### Tampilan Dashboard

Tampilan dashboard memiliki beberapa komponen yang berhubungan dengan transaksi yaitu Saldo, transaksi terendah dan transaksi tertinggi. Oleh karena itu, mulai dari dashboard kita sudah harus melakukan *request* data transaksi yang ada pada server. Namun sebelum itu, karena transaksi yang ditampilkan adalah transaksi milik pengguna yang sudah login saja, maka kita perlu mengambil data pengguna dari `SharedPreferences`. Jika ternyata belum ada data pengguna yang tersimpan, maka aplikasi akan dikembalikan ke tampilan login. Kode untuk `MainActivity.java` adalah sebagaimana di bawah ini:

```java
public class MainActivity extends AppCompatActivity {

    SharedPreferences preferences;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...

        preferences = getSharedPreferences(LoginActivity.AUTH_SESSION, Context.MODE_PRIVATE);
        String token = preferences.getString("token", null);
        if(token == null){
            Intent it = new Intent(this, LoginActivity.class);
            it.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
            it.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            startActivity(it);
        }

        // ...
    }

    // ...

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();

        if (id == R.id.action_logout) {
            preferences.edit().clear().commit();
            Intent it = new Intent(this, LoginActivity.class);
            it.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
            it.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            startActivity(it);
        }

        return super.onOptionsItemSelected(item);
    }

    @Override
    protected void onResume() {
      super.onResume();
      sendRequest();
    }

    public void sendRequest(){
        JsonArrayRequest request = new JsonArrayRequest(EndPoint.TRANSACTION_URL,
                new Response.Listener<JSONArray>() {
                    @Override
                    public void onResponse(JSONArray response) {
                        requestAction(response);
                        updateStat(transList);
                    }
                },
                new Response.ErrorListener() {
                    @Override
                    public void onErrorResponse(VolleyError error) {
                        error.printStackTrace();
                    }
                }
        ){
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> params = new HashMap<String, String>();
                String token = preferences.getString("token", null);
                String uid = Integer.toString(preferences.getInt("uid", 0));
                params.put("token", token);
                params.put("uid", uid);

                return params;
            }
        };

        RequestQueue rq = Volley.newRequestQueue(this);
        rq.add(request);
    }

    private void requestAction(JSONArray response){
        Log.d("jobs.result",response.toString());
        Transaction trans;
        transList = new ArrayList<>();
        try {
            for (int i = 0; i < response.length(); i++) {
                JSONObject objRes = (JSONObject) response.get(i);
                trans = new Transaction(objRes.getString("id"),
                        objRes.getString("name"),
                        objRes.getInt("type"),
                        objRes.getInt("amount"),
                        objRes.getString("description"));
                transList.add(trans);
            }
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }

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

}
```

Seperti halnya pada `LoginActivity` inisialisasi nilai untuk `SharedPreferences` dilakukan pada method `onCreate`, begitu juga untuk pengecekan pengguna aktif namun menggunakan logika terbalik, yaitu jika tidak ada data pengguna yang tersimpan maka aplikasi akan memanggil `LoginActivity`. Selanjutnya pada method `onOptionsItemSelected` kita menambahkan kode untuk aksi logout, yaitu menghapus semua data yang ada pada `SharedPreferences` kemudian mengarahkan tampilan aplikasi kembali ke Login.

Proses pemanggilan *request* ke server dan menampilkan statistik hasilnya kita lakukan pada *life cycle* `onResume` untuk memastikan fungsi tersebut selalu dijalankan ketika Dashboard ditampilkan kembali. Method `sendRequest` melakukan pemanggilan class `JsonArrayRequest` untuk mendapatkan data dari server dalam bentuk JSONArray (bukan object seperti sebelumnya), kemudian memanggil fungsi `requestAction` ketika sudah mendapatkan data dari server. 

Ada sedikit perbedaan pada *request* yang dilakukan kali ini, yaitu kita harus melakukan *override* method `getHeaders` sebagaimana terlihat pada baris 61 - 69. Tujuan dari *overriding* ini adalah mengisikan kode **token** dan **uid** ke dalam request yang dikirim, sehingga server dapat melakukan otentikasi atas pengguna yang sedang aktif.

Adapun fungsi `requestAction` berfungsi untuk memasukkan data array yang didapatkan dari server ke dalam `ArrayList` of `Transaction` untuk dapat dilakukan manipulasi selanjutnya. Dan yang terakhir adalah method `updateStat` melakukan perhitungan statistik menggunakan data yang telah tersimpan pada `ArrayList` sebelumnya.

### Form Tambah Transaksi

Proses menambahkan transaksi dan mengirimkan datanya ke server meliputi pengambilan data pengguna aktif melalui `SharedPreferences` yang dilakukan di baris 11, kemudian menyimpan data form ke dalam `HashMap` sebagaimana ditunjukkan pada baris 17 - 26, dan mengirimkannya menggunakan `JsonObjectRequest` pada baris 28. Seperti halnya `request` pengambilan data, operasi penulisan data di server juga memerlukan proses otentikasi yang dilakukan dengan mengirimkan data *token* dan *uid* melalui *header* sebagaimana ditunjukkan pada baris 46 - 54.

```java
public class TransactionFormActivity extends AppCompatActivity {
    // ...

    SharedPreferences preferences;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_transaction_form);

        preferences = getSharedPreferences(LoginActivity.AUTH_SESSION, Context.MODE_PRIVATE);

        // ...
    }

    public void saveTransaction(View view){
        int type = spnType.getSelectedItemPosition()+1;
        String name = edtName.getText().toString();
        String description = edtDescription.getText().toString();
        int amount = Integer.parseInt(edtAmount.getText().toString());

        HashMap<String, String> data = new HashMap<>();
        data.put("name", name);
        data.put("type", Integer.toString(type));
        data.put("amount", Integer.toString(amount));
        data.put("description", description);

        JsonObjectRequest request = new JsonObjectRequest(EndPoint.TRANSACTION_URL,
                new JSONObject(data),
                new Response.Listener<JSONObject>() {
                    @Override
                    public void onResponse(JSONObject response) {
                        Log.d("transaction.resp", response.toString());
                        Intent intent = new Intent(getBaseContext(), LoginActivity.class);
                        startActivity(intent);
                    }
                },
                new Response.ErrorListener(){
                    @Override
                    public void onErrorResponse(VolleyError error) {
                        error.printStackTrace();
                    }
                }
        ){
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> params = new HashMap<String, String>();
                String token = preferences.getString("token", null);
                String uid = Integer.toString(preferences.getInt("uid", 0));
                params.put("token", token);
                params.put("uid", uid);

                return params;
            }
        };

        RequestQueue rq = Volley.newRequestQueue(this);
        rq.add(request);
    }
}
```

Setelah menyelesaikan bagian ini, kita bisa mencoba menjalankan aplikasi melakukan Register, Login, menambahkan data transaksi baru dan melihat apakah data yang barusan kita masukkan sudah direkap statistiknya pada Dashboard.

### List Transaksi

Class `TransactionListActivity` bertugas untuk menampilkan data transaksi yang didapatkan dari server dalam bentuk list. Pada tutorial ini kita menggunakan list yang paling sederhana yaitu ListView dengan tampilan **String** saja. Langkah awal yang dilakukan sama seperti sebelumnya yaitu mengambil data pengguna aktif dari `SharedPreferences` untuk dapat dimasukkan ke dalam data header yang dikirim. Selanjutnya melakukan *request* ke server dengan menggunakan `JsonArrayRequest` dan ketika sudah mendapatkan datanya. Kemudian langkah terakhir mengeksekusi fungsi `requestAction` untuk menampilkan dalam bentuk List. Koding untuk proses tersebut sebagaimana ditampilkan di bawah ini:

```java
public class TransactionListActivity extends AppCompatActivity {

    private ArrayList<Transaction> transList = new ArrayList<>();
    SharedPreferences preferences;

    // ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_transaction_list);

        preferences = getSharedPreferences(LoginActivity.AUTH_SESSION, Context.MODE_PRIVATE);

        // ...
    }

    @Override
    protected void onResume() {
        super.onResume();
        sendRequest();
    }

    public void sendRequest(){
        JsonArrayRequest request = new JsonArrayRequest(EndPoint.TRANSACTION_URL,
                new Response.Listener<JSONArray>() {
                    @Override
                    public void onResponse(JSONArray response) {
                        requestAction(response);
                    }
                },
                new Response.ErrorListener() {
                    @Override
                    public void onErrorResponse(VolleyError error) {
                        error.printStackTrace();
                    }
                }
        ){
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> params = new HashMap<String, String>();
                String token = preferences.getString("token", null);
                String uid = Integer.toString(preferences.getInt("uid", 0));
                params.put("token", token);
                params.put("uid", uid);

                return params;
            }
        };

        RequestQueue rq = Volley.newRequestQueue(this);
        rq.add(request);
    }

    private void requestAction(JSONArray response){
        Log.d("jobs.result",response.toString());
        Transaction trans;
        transList = new ArrayList<>();
        try {
            for (int i = 0; i < response.length(); i++) {
                JSONObject objRes = (JSONObject) response.get(i);
                trans = new Transaction(objRes.getString("id"),
                        objRes.getString("name"),
                        objRes.getInt("type"),
                        objRes.getInt("amount"),
                        objRes.getString("description"));
                transList.add(trans);
            }
            adapter = new ArrayAdapter(this, android.R.layout.simple_list_item_1, transList);
            transactionList.setAdapter(adapter);
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }

}
```

Method `requestAction` bertanggung jawab memasukkan data yang diterima dari server ke dalam `ArrayList` of `Transaction` yang sudah dipersiapkan sebelumnya dan juga mengeset nilai tersebut sebagai acuan pada adapter milik `ListView` pada tampilan. Secara otomatis, list tampilan akan berubah dan menampilkan semua data yang didapatkan dari server.

### Detail Transaksi dan Hapus Data

Proses terakhir yang perlu kita lakukan adalah mengirimkan *request* penghapusan data ke server. Sebelum dapat menghapus data, masih sama seperti proses sebelumnya kita harus mengirimkan **token** dan **uid** yang sudah tersimpan pada `SharedPreferences` dengan melakukan *overriding* pada header. Koding lengkap proses penghapusan transaksi tertentu ditunjukkan pada koding di bawah ini:

```java
public class TransactionDetailActivity extends AppCompatActivity {

    // ...

    SharedPreferences preferences;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_transaction_detail);

        preferences = getSharedPreferences(LoginActivity.AUTH_SESSION, Context.MODE_PRIVATE);

        // ...
    }

    public void delTransaction(View view){

        StringRequest request = new StringRequest(Request.Method.DELETE,
                EndPoint.TRANSACTION_URL +"/"+transaction.getId(),
                new Response.Listener<String>() {
                    @Override
                    public void onResponse(String response) {
                        Log.d("delete.response", response.toString());
                        finish();
                    }
                },
                new Response.ErrorListener() {
                    @Override
                    public void onErrorResponse(VolleyError error) {
                        Log.e("delete.error", error.getMessage());
                    }
                }
        ){
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> params = new HashMap<String, String>();
                String token = preferences.getString("token", null);
                String uid = Integer.toString(preferences.getInt("uid", 0));
                params.put("token", token);
                params.put("uid", uid);

                return params;
            }
        };
        RequestQueue queue = Volley.newRequestQueue(this);
        queue.add(request);

    }

}
```

Khusus untuk kasus pengahapusan kita tidak menggunakan `JsonObjectRequest` atau `JsonArrayRequest`, tapi kita menggunakan *class* yang lebih umum yaitu `StringRequest`. Selain itu, karena pada web service kita sudah mensyaratkan pengahapusan menggunakan method **DELETE**, maka pada saat melakukan request kita juga menggunakan method tersebut sebagaimana ditunjukkan pada baris 19 pada koding di atas.