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

### Clone Template Aplikasi

Aplikasi Android yang kita kembangkan di sini akan menggunakan template aplikasi yang sudah tersedia di repository github saya ini. Tujuannya agar kita tidak perlu ribet membuat tampilan yang sudah kita pelajari di tutorial sebelumnya. Jika anda memiliki git di komputer, anda bisa melakukan *clone* terhadap aplikasi dengan menggunakan perintah berikut:

```
git clone 
```

Jika anda tidak memiliki aplikasi git, anda bisa mendownload template tersebut melalui link ini.

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

        String url = EndPoint.REGISTER_URL;

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

### Proses Login

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
        // ...
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
                            Toast.makeText(getBaseContext(),
                                    "Username / Password salah",
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

    public void saveToken(int uid, String token){
        SharedPreferences.Editor editor = preferences.edit();

        editor.putString("token", token);
        editor.putInt("uid", uid);
        editor.commit();
    }

    public void loginAct(View view) {
        sendRequest();
    }

    // ...
}
```

### Tampilan Dashboard

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

### Form Tambah Transaksi

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

### List Transaksi

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

### Detail Transaksi dan Hapus Data

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