---
title: WP Rest API CLient dengan Python
date: 2017-03-29 03:04:06
tags:
  - python
  - wordpress
  - rest
categories:
  - Pemrograman
  - Web Development
  - Wordpress
banner: /images/banner.jpg
---

Wordpress merupakan salah satu content management system (CMS) yang paling banyak digunakan untuk membuat web (apapun) secara mudah dan cepat. Salah satu faktor yang membuat wordpress sangat disukai baik oleh developer maupun casual user adalah karena kemudahan instalasi dan kelengkapan fiturnya.

Salah satu fitur yang diusung wordpress versi terbaru (Versi 4.7) adalah REST API yang memungkinkan aplikasi lain (dengan bahasa pemrograman apapun) untuk berinteraksi secara aktif dengan website Wordpress kita. Yang saya maksudkan interaksi di sini bukan hanya sekedar menampilkan dan membaca postingan saja tapi bahkan juga menambah, edit, dan hapus post secara secara langsung melalui REST API ini.

Pada artikel ini saya akan menjelaskan langkah penggunaan REST API ini dan juga pembuatan client sederhana untuk berinteraksi dengan wordpress menggunakan **Python 3**.<!--more-->

## Persiapan

*By default* jika kita menggunakan Wordpress versi 4.7 ke atas REST API ini sudah terpasang dan siap digunakan sejak kita menginstall wordpress (Versi sebelumnya kita harus menginstall plugin [WP-API V2](http://v2.wp-api.org/). Hanya saja untuk beberapa akses penulisan, kita harus melakukan otentikasi pengguna dengan menggunakan plugin tambahan.

Jika kita menggunakan REST API ini untuk keperluan sungguhan, sebaiknya kita menggunakan plugin [OAuth 1.0a Server](https://wordpress.org/plugins/rest-api-oauth1/) untuk memastikan hanya aplikasi yang sudah terdaftar dan mendapat izin saja yang diperbolehkan mengakses REST API kita. Namun demi kemudahan tutorial, kali ini kita akan menggunakan [JSON Basic Authentication](https://github.com/WP-API/Basic-Auth) untuk menggawangi otentikasi. Langkah instalasi dari plugin tersebut:

1. Download plugin di https://github.com/WP-API/Basic-Auth/archive/master.zip
2. Pada Dashboard admin, pilih menu `Plugins > Add New`
3. Klik tombol `Upload Plugin` yang berada di atas halaman, kemudian upload file plugin yang telah kita download tadi
4. Setelah proses instalasi selesai jangan lupa mengaktifkan plugin tersebut

## Menampilkan Semua Postingan

Untuk menampilkan postingan yang ada pada web wordpress kita tidak perlu menggunakan otentikasi, cukup mengarahkan API Client ke alamat http://url.websitekita.id/wp-json/wp/v2/posts dan menampilkan hasilnya. Berikut adalah koding python untuk menampilkan semua postingan pada website wordpress yang sudah saya install sebelumnya pada alamat http://localhost/wordpress

```python
import base64
import json
import requests

def get_posts():
    url = 'http://localhost/wordpress/wp-json/wp/v2/posts'
    r = requests.get(url)
    return r.content

# pemanggilan fungsi
get_posts()
```

Jika kita menampilkan hasil dari pemanggilan fungsi `get_posts()` dengan perintah `print()` atau yang lainnya, kita akan mendapatkan hasil yang berisi semua postingan pada web wordpress kita dalam format JSON.

## Membuat Post Baru

Proses pembuatan postingan baru melalui REST API memerlukan otentikasi berupa `username` dan `password` yang telah di encode dengan menggunakan base64. Berikut adalah kodingnya dengan Python:

```python
def create_post(title, content):
    url = 'http://localhost/wordpress/wp-json/wp/v2/posts/'
    auth = base64.b64encode(b'username:password').decode('ascii')
    data = {
        'title': title,
        'type': 'post',
        'content': content,
        'status': 'publish',
    }
    headers = {
        'Accept': 'application/json',
        'Content-Type': 'application/json',
        'Authorization': 'Basic '+auth,
    }
    r = requests.post(url, data = json.dumps(data), headers=headers)
    return r

# pemanggilan fungsi
create_post("best content", "this is the best content ever created")
```

Panggil fungsi `create_post` yang barusan kita buat, dan cek hasilnya dengan mengunjungi website wordpress kita. Jika tidak terjadi Error, maka postingan baru akan muncul pada website kita.

## Upload File Media

Wordpress membatasi penggunaan *Featured Image* hanya melalui gambar yang telah diupload ke media. Oleh karena itu, sebelum kita bisa menggunakannya ke dalam postingan kita harus meng-*upload* file gambar yang kita miliki dulu ke website wordpress kita. Berikut adalah kodingnya dengan python

```python
def post_media(image_url, file_name):
    url = 'http://cheapedias.com/wp-json/wp/v2/media/'
    auth = base64.b64encode(b'kaqfa:firdaus123').decode('ascii')

    img = open(image_url,'rb').read()
    headers = {
        'Content-Type': 'multipart/form-data',
        'Authorization': 'Basic '+auth,
        'Content-Disposition': 'attachment; filename='+file_name,
    }

    r = requests.post(url, data=img, headers=headers)
    return r.content

# Pemanggilan fungsi
post_media('c:/alamat/file/gambar.jpg', 'gambar.jpg')
```

Sama seperti posting, setelah menjalankan fungsi `post_media` cek kembali media pada website wordpress kita. Jika tidak ada kesalahan, seharusnya media yang kita inginkan sudah terupload.

## Penutup

Demikian tutorial singkat pembuatan Client untuk WP REST API, tentu saja masih banyak lagi yang harus dibuat jika kita ingin membuat WP Client dengan python secara lengkap. Pada tutorial ini saya hanya menunjukkan beberapa bagian penting saja, sedangkan lebih lengkap cara penggunaan REST API untuk wordpress bisa dilihat pada [Dokumentasi Resmi](https://developer.wordpress.org/rest-api/).

Jika masih bingung dan belum jelas, jangan sungkan-sungkan untuk bertanya. ~~Meskipun juga belum tentu saya jawab sih~~